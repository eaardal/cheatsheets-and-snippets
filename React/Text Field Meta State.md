### Text Field Meta State Hook

Hook for managing the meta state for a text field, such as focus, blur and so on.

```typescript
import { useEffect, useState } from "react";

export type FieldValue = string | number | boolean;

export interface MetaState {
  // True if the user has touched the field or the field is currently focused.
  isTouched: boolean;
  // True if the field is currently focused.
  inFocus: boolean;
  // True if the user has touched and left the field. Useful when you want to display an error message only after the user has left the field and not while typing.
  isFirstTouchDone: boolean;
  // True if the field has an error. This is the result of invoking the hasErrorCallback function.
  hasError: boolean;
  // True if an error message should be displayed. Note that there might not be an error to show even if this is true (hasError might be false).
  // This value only indicate that an eventual error should be displayed if present given the applied strategies, not whether there is an error to display.
  // It is entirely separate from hasError.
  showError: boolean;
  // Convenience field, true if both hasError and showError is true.
  hasErrorToShow: boolean;
}

export interface MetaStateActions {
  // Callback for when the field gains focus. Assign to the input field's onFocus prop.
  onFocus(): void;
  // Callback for when the field loses focus. Assign to the input field's onBlur prop.
  onBlur(): void;
  // Callback for when the field's value changes. Assign to the input field's onChange prop.
  onChange(value: FieldValue): void;
  // Reset the state to its initial values.
  reset(): void;
}

export type ErrorStrategy =
  // Show the error as long as there is an error, regardless of the focus, touch or dirty state.
  | "showErrorWhenError"
  // Show the error when the field is touched or focused.
  | "showErrorWhenTouched"
  // Show the error when the field is touched once and the user has left the field.
  | "showErrorWhenFirstTouchDone";

export type ReFocusStrategy =
  // Reset the focus and error state when the field is focused again. This hides the error when the user is attempting to fix it.
  | "resetState"
  // Keep the focus and error state when the field is focused again. This is the same as leaving reFocusStrategy undefined.
  | "keepState";

export type ValidateArgs = {
  value: FieldValue;
  isTouched: boolean;
  isFocused: boolean;
  isFirstTouchDone: boolean;
  options: MetaStateOptions;
};

function makeOptions(opts?: MetaStateOptions): MetaStateOptions {
  const options: MetaStateOptions = {
    errorStrategy: "showErrorWhenFirstTouchDone",
    reFocusStrategy: "keepState",
  };

  if (opts?.errorStrategy) {
    options.errorStrategy = opts.errorStrategy;
  }

  if (opts?.reFocusStrategy) {
    options.reFocusStrategy = opts.reFocusStrategy;
  }

  return options;
}

export interface MetaStateOptions {
  errorStrategy?: ErrorStrategy;
  reFocusStrategy?: ReFocusStrategy;
}

export function useTextFieldMetaState(
  hasErrorCallback: (args: ValidateArgs) => boolean,
  opts?: MetaStateOptions
): MetaState & MetaStateActions {
  const options = makeOptions(opts);

  const [isTouched, setTouched] = useState(false);
  const [isFocused, setFocused] = useState(false);
  const [isFirstTouchDone, setFirstTouchDone] = useState(false);
  const [hasError, setHasError] = useState(false);
  const [showError, setShowError] = useState(false);
  const [fieldValue, setFieldValue] = useState<FieldValue>("");

  function checkForError(value: FieldValue) {
    const isError = hasErrorCallback({
      value,
      isTouched,
      isFocused,
      isFirstTouchDone,
      options,
    });

    if (isError) {
      setHasError(true);

      if (opts?.errorStrategy === "showErrorWhenError") {
        setShowError(true);
      }
      if (opts?.errorStrategy === "showErrorWhenTouched" && isTouched) {
        setShowError(true);
      }
      if (
        opts?.errorStrategy === "showErrorWhenFirstTouchDone" &&
        isFirstTouchDone
      ) {
        setShowError(true);
      }
    } else {
      setHasError(false);
      setShowError(false);
    }
  }

  function onFocus() {
    setTouched(true);
    setFocused(true);

    if (isTouched && opts?.reFocusStrategy === "resetState") {
      setHasError(false);
      setShowError(false);
      setFirstTouchDone(false);
    }
  }

  function onBlur() {
    if (isFocused && !isFirstTouchDone) {
      setFirstTouchDone(true);
    }

    setTouched(true);
    setFocused(false);

    checkForError(fieldValue);
  }

  function reset() {
    setTouched(false);
    setFocused(false);
    setFirstTouchDone(false);
  }

  function onChange(value: FieldValue) {
    setFieldValue(value);
    checkForError(value);
  }

  useEffect(() => {
    if (
      opts?.errorStrategy === "showErrorWhenFirstTouchDone" &&
      isFirstTouchDone
    ) {
      setShowError(true);
    } else {
      setShowError(false);
    }
  }, [isFirstTouchDone]);

  return {
    isTouched: isTouched,
    inFocus: isFocused,
    isFirstTouchDone: isFirstTouchDone,
    onFocus: onFocus,
    onBlur: onBlur,
    onChange: onChange,
    reset: reset,
    showError: showError,
    hasError: hasError,
    hasErrorToShow: hasError && showError,
  };
}
```
