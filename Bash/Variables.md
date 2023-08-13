### Variable is not required, but when set, the path must be to a valid file on disk.

- `-n $var`: True if the variable is not empty. (`-z $var` is true if it _is_ empty).
- `-f $path`: True if the path is a file on disk. (`-d $path` for directory).

```bash
if [ -n "$manifest" ] && [ ! -f "$manifest" ]
then
    echo "manifest $manifest does not exist on disk"
    exit 1
fi
```

### Variable required

- `-z $var`: True if string is empty
- `-n $var`: True if string is non-empty

```bash
if [ -z "$BITBUCKET_REPO_SLUG" ]
then
    echo "Evironment variable BITBUCKET_REPO_SLUG not set"
    exit 1
fi
```

Check if function arg is provided, print error and help if not and set local variable if it is.

```bash
somefunc() {
  if [ -z "$1" ]; then echo "No <field> provided" && print_help; fi
  local fieldPath=$1
}
```
