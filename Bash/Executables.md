### Check if command/executable exists

```bash
if ! command -v brew &> /dev/null
then
  echo "brew does not exist"
else
  echo "brew exists"
fi
```

```bash
if ! command -v tubeify &> /dev/null
then
  echo "Tubeify is not in PATH"
  return 1
fi
```
