### Stringify error

```typescript
// eslint-disable-next-line @typescript-eslint/no-explicit-any
export function stringifyError(error: any, stack?: boolean): string {
  if (error instanceof Error) {
    if (stack && error.stack) {
      return error.stack;
    }
    return error.message;
  }

  if (typeof error === "string") {
    return error;
  }

  if (typeof error === "object") {
    return JSON.stringify(error);
  }

  return `Unknown error: ${error}`;
}
```
