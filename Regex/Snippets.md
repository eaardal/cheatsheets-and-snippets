### Semver

Regex som matcher alle varianter av semver: https://regexr.com/4auap

```go
var semver = regexp.MustCompile(`^(\\d+\\.)?(\\d+\\.)?(\\*|\\d+)(-(0|[1-9]\\d*|\\d*[a-zA-Z-][0-9a-zA-Z-]*)(\\.(0|[1-9]\\d*|\\d*[a-zA-Z-][0-9a-zA-Z-]*))*)?(\\+[0-9a-zA-Z-]+(\\.[0-9a-zA-Z-]+)*)?$`)
```
