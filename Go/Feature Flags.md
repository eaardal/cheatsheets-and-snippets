### Feature flags

```go
package featureflags

import (
	"context"
	"strings"

	"golang.tv2.no/rp-authorization-lib/auth0"
	"golang.tv2.no/tv2-go-observability/log"
)

const (
	On  = "true"
	Off = "false"
)

type featureFlagContextKey string

var (
	EnrichBookingReports = featureFlagContextKey("FEATURE_ENRICH_BOOKING_REPORTS")
	SearchWithFilters    = featureFlagContextKey("FEATURE_SEARCH_WITH_FILTERS")
	NoBookingApiCalls    = featureFlagContextKey("FEATURE_NO_BOOKINGAPI_CALLS")
)

type featureFlagState struct {
	flagKey featureFlagContextKey
	value   string
}

type FeatureFlagOption func(ctx context.Context) featureFlagState

func Set(ctx context.Context, opts ...FeatureFlagOption) context.Context {
	flags := []featureFlagState{}

	for _, opt := range opts {
		flags = append(flags, opt(ctx))
	}

	ctxWithFeatureFlags := ctx
	for _, flagState := range flags {
		ctxWithFeatureFlags = context.WithValue(ctxWithFeatureFlags, flagState.flagKey, flagState.value)
	}

	return ctxWithFeatureFlags
}

func EnableForUser(feature featureFlagContextKey, email ...string) FeatureFlagOption {
	return func(ctx context.Context) featureFlagState {
		return setFeatureFlagStateForUser(ctx, feature, true, email...)
	}
}

func DisableForUser(feature featureFlagContextKey, email ...string) FeatureFlagOption {
	return func(ctx context.Context) featureFlagState {
		return setFeatureFlagStateForUser(ctx, feature, false, email...)
	}
}

func EnableForAllUsers(feature featureFlagContextKey) FeatureFlagOption {
	return func(ctx context.Context) featureFlagState {
		return featureFlagState{flagKey: feature, value: On}
	}
}

func DisableForAllUsers(feature featureFlagContextKey) FeatureFlagOption {
	return func(ctx context.Context) featureFlagState {
		return featureFlagState{flagKey: feature, value: Off}
	}
}

func IsEnabled(ctx context.Context, feature featureFlagContextKey) bool {
	if value, ok := ctx.Value(feature).(string); ok {
		return value == On
	}

	return false
}

func IsDisabled(ctx context.Context, feature featureFlagContextKey) bool {
	if value, ok := ctx.Value(feature).(string); ok {
		return value == Off
	}

	return true
}

func setFeatureFlagStateForUser(ctx context.Context, feature featureFlagContextKey, enabled bool, email ...string) featureFlagState {
	featureFlag := featureFlagState{flagKey: feature}

	if claims, err := auth0.ExtractAccessTokenClaimsFromContext(ctx); err != nil || claims == nil {
		log.Errorf("error extracting claims from context: %v", err)
	} else if isEmailInList(claims.Email, email) && enabled {
		log.Infof("Enabling feature %s for %s", feature, claims.Email)
		featureFlag.value = On
	} else if isEmailInList(claims.Email, email) && !enabled {
		log.Infof("Disabling feature %s for %s", feature, claims.Email)
		featureFlag.value = Off
	} else {
		log.Infof("Disabling feature %s for %s (not in allowed list)", feature, claims.Email)
		featureFlag.value = Off
	}

	return featureFlag
}

func isEmailInList(email string, list []string) bool {
	for _, e := range list {
		if strings.EqualFold(e, email) {
			return true
		}
	}

	return false
}

```
