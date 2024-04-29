### Loop over array of strings

Loops over an array where each item is a semicolon separated string: `<user>;<role>`.

For each item in the array, split by semicolon and extract each part.

```bash
declare -a users=(
  "Bob;user"
  "Anne;sysadmin"
)

for i in ${!users[@]}; do
  userAndRole="${users[$i]}"

  IFS=';' read -a parts <<< "${userAndRole}"

  name=${parts[0]}
  role=${parts[1]}

  if [[ ! -z "$name" ]]
  then
    # do something with $name
  else
    # $name is not set
  fi
done
```

### Loop over each line in a file

https://stackoverflow.com/a/1521498

[Bash `read` command](https://linuxhint.com/bash_read_command/)

[`IFS read -r` info](https://unix.stackexchange.com/questions/209123/understanding-ifs-read-r-line)

```bash
while IFS="" read -r p || [ -n "$p" ]
do
  printf '%s\n' "$p"
done < myfile.txt
```

Example where $filePath is a file where each line is a key-value pair separated by `=`.

Split each line by `=` and read each side of the split.

```bash
filePath="path/to/file.txt"
while IFS="=" read -r -a kvp || [ -n "$kvp" ]
do
  key=${kvp[0]}
  value=${kvp[1]}
done < $filePath
```
