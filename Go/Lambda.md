### Lambda-like list operations

```go
package lambda

func Map[T, U any](data []T, f func(T) U) []U {
	res := make([]U, 0, len(data))
	for _, e := range data {
		res = append(res, f(e))
	}
	return res
}

func Filter[T any](s []T, f func(T) bool) []T {
	var result []T
	for _, e := range s {
		if f(e) {
			result = append(result, e)
		}
	}
	return result
}
```
