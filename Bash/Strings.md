### Split text on delimiter and take part

```bash
fieldPath="some.thing"
first=$(cut -d'.' -f1 <<< "$fieldPath") # Split by dot and take first part as section
second=$(cut -d'.' -f2 <<< "$fieldPath") # Split by dot and take second part as field
# $first: "some"
# $second: "thing"
```

### Check if variable starts with substring

```bash
result="ERROR: Bad stuff"
if [[ "$result" = ERROR* ]]; then
	echo "handle error"
else
  echo "did not start with ERROR"
fi
```

Note: Lack of quotes around ERROR\* and single = - it’s not a string match.

### Check if substring is in string

```bash
result="[ERROR] 2023/06/16 14:25:34 "my.secret/to/something" isn't a field in the .sekret item"
if [[ "$result" == *"[ERROR]"* ]] && [[ "$result" == *"isn't a field"* ]]; then
  echo "is this particular error"
else
  echo "is not this error"
fi
```

### Extract substring from string

```bash
contractFileName="rp_task_api_gateway.swagger.json"
appName=${contractFileName%".swagger.json"}
# $appName: rp_task_api_gateway
```
