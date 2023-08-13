### File exists check

- `[` is alias for `test` command.
- `[[` is newer version of `[`.
- https://unix.stackexchange.com/a/306115
- https://unix.stackexchange.com/questions/369850/bash-what-do-the-brackets-in-if-statements-do

```bash
FILE=path/to/file
if test -f "$FILE"; then echo "file exists"; fi
if [ -f "$FILE" ]; then echo "file exists"; fi
if [[ -f "$FILE" ]]; then echo "file exists"; fi
if [[ ! -f "$FILE" ]]; then echo "file does not exist"; fi
```

### Directory exists check

```bash
[ -d "/path/to/dir" ] && echo "Directory /path/to/dir exists."

## OR ##
[ ! -d "/path/to/dir" ] && echo "Directory /path/to/dir DOES NOT exists."

## OR combine both of them in a single go: ##
[ -d "/path/to/dir" ] && echo "Directory /path/to/dir exists." || echo "Error: Directory /path/to/dir does not exists.
```

### Directory is empty check

```bash
find /dir/name -empty -type -f -exec command {} \;
find /dir/name -empty -type -d -exec command {} \;

# GNU/BSD find command syntax: #
find /path/to/dir -maxdepth 0 -empty -exec echo {} is empty. \;

# OR
find /path/to/dir -type d -empty -exec command1 arg1 {} \;
```

### Check if symlink exists and is not broken

- `-L` Returns true if the "file" exists and is a symbolic link (the linked file may or may not exist).
- `-f` Returns true if file exists and is a regular file.
- `-e` Returns true if file exists regardless of type.

```bash
# https://stackoverflow.com/a/36180056

if [ -L ${my_link} ] ; then
   if [ -e ${my_link} ] ; then
      echo "Good link"
   else
      echo "Broken link"
   fi
elif [ -e ${my_link} ] ; then
   echo "Not a link"
else
   echo "Missing"
fi
```
