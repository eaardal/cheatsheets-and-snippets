### Verify that a program has a specific version

```makefile
mockery_version = $(shell mockery --version)

generate: clean
ifneq ($(mockery_version), 2.5.1)
$(error "Bad mockery version, please install 2.5.1")
endif
```
