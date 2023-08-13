### Check if command/executable exists

```bash
if ! command -v brew &> /dev/null
then
  echo "Install Homebrew"
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
else
  echo "Update Homebrew"
  brew update --auto-update
fi
```

```bash
if ! command -v tubeify &> /dev/null
then
  echo "Tubeify is not in PATH"
  return 1
fi
```
