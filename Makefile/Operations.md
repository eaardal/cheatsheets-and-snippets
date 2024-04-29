### Delete existing file/folder or continue if it doesn't exist

```makefile
foo:
	rm app.zip || true
  ...
```
