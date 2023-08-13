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

# To align everything at MISSING+1 space = everything must be indented 8 spaces.
# DIFF is printed with %-4s = 8 spaces
# OK is printed with %-6s = 8 spaces
# MISSING is printed with %-1s = 8 spaces

printf "OK:%-6s$envyKey matches Envy value $envyValue\n"
printf "DIFF:%-4s$envyKey value $shellValue differs from Envy value $envyValue\n"
printf "MISSING:%-1s$envyKey is not an active environment variable\n"

OK:      OVH_APPLICATION_KEY matches Envy value 4Y4zPjKjedoIVjue
OK:      OVH_APPLICATION_SECRET matches Envy value CXUhVoF69RxDsQJ5oipiJr72L6vPfddX
OK:      OVH_CONSUMER_KEY matches Envy value mr08KRs3jJhPdfDAuZZKTiz75iA1eyws
OK:      OVH_ENDPOINT matches Envy value ovh-eu
OK:      OS_PASSWORD matches Envy value QsUx8p2pcdYtm2pdCg8y26fRa5KGWQ2D
DIFF:    AUTH0_DOMAIN value tullogfjas differs from Envy value reklameplattform.tv2norway-stage.auth0.com
OK:      AUTH0_CLIENT_ID matches Envy value ObqefbiwTYFRh3T6rrHHgd7Hwb9uKyC4
MISSING: AUTH0_CLIENT_SECRET is not an active environment variable
OK:      TF_VAR_auth0_client_id matches Envy value ObqefbiwTYFRh3T6rrHHgd7Hwb9uKyC4
OK:      TF_VAR_auth0_domain matches Envy value reklameplattform.tv2norway-stage.auth0.com
```

### Color output text

```bash
RED=$(tput setaf 1) GREEN=$(tput setaf 2) YELLOW=$(tput setaf 3) NC=$(tput sgr0)

printf "${GREEN}Success${NC} This is not colored"
printf "${YELLOW}Warning${NC}"
printf "${RED}Error${NC}"
```
