### Script template 1

```bash
#!/usr/bin/env bash

set -o errexit
set -o nounset
set -o pipefail
if [[ "${TRACE-0}" == "1" ]]; then
    set -o xtrace
fi

if [[ "${1-}" =~ ^-*h(elp)?$ ]]; then
    echo 'Usage: ./script.sh arg-one arg-two

This is an awesome bash script to make your life better.

'
    exit
fi

cd "$(dirname "$0")"

main() {
    echo do awesome stuff
}

main "$@"
```

### Script template 2

```bash
#!/bin/bash

print_help() {
  echo -e ""
  echo -e "Usage:"
  echo -e ""
	echo -e "first command - describe what it does"
	echo -e "second command - describe what it does"
	echo -e "etc..."
}

# Fail script immediately on any errors
set -e

# Set any default environment variables
FOO=bar

# Handle empty input
if [ -z "$1" ]
then
  echo "No command provided, here's the help command instead:";
  print_help
  exit 0
fi

# Required environment variables:
if [ -z "$REQUIRED_ENV" ]
then
  echo "Environment variable REQUIRED_ENV is not set. It should be set to TODO";
  print_help
  exit 1
fi

# Optional environment variables with default values as fallback:
if [ -n "$OPTIONAL_ENV" ]
then
  OPTIONAL=$OPTIONAL_ENV
else
  OPTIONAL="some default value"
fi

first_command() {
  # TODO: Command handler
}

second_command() {
  # TODO: Command handler
}

case $1 in
  "help")
    print_help
    exit 0
    ;;
  "first_command")
    first_command "$@"
    ;;
  "second_command")
    second_command "$@"
    ;;
  *)
    echo "Unknown command '$1'"
    print_help
    exit 1
    ;;
esac

# Restore bash error flag
set +e
```

### Options to a .sh script

```bash
options=':k:m:jyn:'
while getopts $options option
do
    case "$option" in
        n  ) namespace=$OPTARG;;
        j  ) job=true;;
        h  ) usage; exit;;
        y  ) yq=true;;
        m  ) manifest=$OPTARG;;
        k  ) kustomize=$OPTARG;;
        \? ) echo "Unknown option: -$OPTARG" >&2; exit 1;;
        :  ) echo "Missing option argument for -$OPTARG" >&2; exit 1;;
        *  ) echo "Unimplemented option: -$OPTARG" >&2; exit 1;;
    esac
done
```

`OPTSTRING` is string with list of expected arguments,

- `h` - check for option `h` **without** parameters; gives error on unsupported options;
- `h:` - check for option `h` **with** parameter; gives errors on unsupported options;
- `abc` - check for options `a`, `b`, `c`; **gives** errors on unsupported options;
- `:abc` - check for options `a`, `b`, `c`; **silences** errors on unsupported options;
  Notes: In other words, colon in front of options allows you handle the errors in your code. Variable will contain `?` in the case of unsupported option, `:` in the case of missing value.
- `OPTARG` - is set to current argument value,
- `OPTERR` - indicates if Bash should display error messages.

### Ensure function argument exists

```bash
field_exists() {
  if [ -z "$1" ]; then echo "No <field> provided" && print_help && exit 1; fi
  local fieldPath=$1
	# use $fieldPath...
}
```
