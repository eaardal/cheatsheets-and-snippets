### Verify that argument is provided

```makefile
foo:
ifndef MY_FLAG
$(error MY_FLAG is not set)
endif
```

Shorter version using bash. Note that this is indented with one tab whereas the `ifndef` is not indented.

```makefile
foo:
  [ -n "$(VERSION)" ] || (echo "VERSION is not set" && false)
```

Using native Make to check that a argument is set

```makefile
# Check that given variables are set and all have non-empty values,
# die with an error otherwise.
#
# Params:
#   1. Variable name(s) to test.
#   2. (optional) Error message to print.
check_defined = \
    $(strip $(foreach 1,$1, \
        $(call __check_defined,$1,$(strip $(value 2)))))
__check_defined = \
    $(if $(value $1),, \
      $(error Undefined $1$(if $2, ($2))))

# Usage, check that VERSION is set:
something:
  $(call check_defined, VERSION)
  ...
```
