### Align output text

https://unix.stackexchange.com/questions/396223/bash-shell-script-output-alignment

```bash
$ printf '|%-4s|\n' a ab abc abcd abcde
|a   |
|ab  |
|abc |
|abcd|
|abcde|
 printf '|%4s|\n' a ab abc abcd abcde
|   a|
|  ab|
| abc|
|abcd|
|abcde|
```

```bash
# MISSING: 7 characters
# DIFF: 4 characters
# OK: 2 characters

# To align everything at MISSING+1 space, everything must be indented 8 spaces (7+1).
# DIFF is printed with %-4s = 8 spaces
# OK is printed with %-6s = 8 spaces
# MISSING is printed with %-1s = 8 spaces

printf "OK:%-6sBla bla bla\n"
printf "DIFF:%-4sBla bla bla\n"
printf "MISSING:%-1sBla bla bla\n"

OK:      Bla bla bla
DIFF:    Bla bla bla
OK:      Bla bla bla
MISSING: Bla bla bla
OK:      Bla bla bla
OK:      Bla bla bla
```

### Color output text

```bash
RED=$(tput setaf 1) GREEN=$(tput setaf 2) YELLOW=$(tput setaf 3) NC=$(tput sgr0)

printf "${GREEN}Success${NC} This is not colored"
printf "${YELLOW}Warning${NC}"
printf "${RED}Error${NC}"
```
