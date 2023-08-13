### Verify that argument is provided

```makefile
foo:
ifndef MY_FLAG
$(error MY_FLAG is not set)
endif
```
