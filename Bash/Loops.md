### Loop over array of strings

```bash
declare -a usersToAdd=(
  "foo;$systemAdminRoleId"
  "Eirik Bell;$systemAdminRoleId"
)

for i in ${!usersToAdd[@]}; do
  userAndRole="${usersToAdd[$i]}"

  IFS=';' read -a parts <<< "${userAndRole}"

  userName=${parts[0]}
  roleId=${parts[1]}

  if [[ ! -z "$userId" ]]
  then
    assign_roles_to_member_in_organization $baseUrl $accessToken $organizationId $userId "[$systemAdminRoleId]"
    echo "assigned role $roleId to user $userId ($userName) in org $organizationId"
  else
    echo "No user found for $userName. Does the user profile in Auth0 have is_reklameplattform_member=true set in its user metadata dictionary?"
  fi
done
```

### Loop over each line in a file

https://stackoverflow.com/a/1521498

read command: https://linuxhint.com/bash_read_command/

IFS read details: https://unix.stackexchange.com/questions/209123/understanding-ifs-read-r-line

```bash
while IFS="" read -r p || [ -n "$p" ]
do
  printf '%s\n' "$p"
done < myfile.txt
```

Split by “=” and read each side of the split

```bash
while IFS="=" read -r -a kvp || [ -n "$kvp" ]
do
  key=${kvp[0]}
  value=${kvp[1]}
  pathEnv=".$path"  valueEnv="$key: $value" yq 'eval(strenv(pathEnv)) = env(valueEnv)' ./env.yaml
done < $filePath
```
