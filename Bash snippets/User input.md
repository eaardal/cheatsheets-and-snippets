### Yes/no confirmation from user

```bash
read -p "Did you do the thing?  " -n 1 -r
echo    # (optional) move to a new line
if [[ ! $REPLY =~ ^[Yy]$ ]]
then
    echo "Did not press Y, exiting"
    exit 1
else
  echo "Pressed Y!"
fi
```
