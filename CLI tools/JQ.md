### Flags

```jsx
-r -- Fjerner double quotes rundt strings etter filtrering.
-c -- Compact, resultatet er på 1 linje
```

### Map list of objects to list of comma-separated strings

```jsx
// users = [{ "user_id": "user123" }, { "user_id": "user456" }]
memberIds=$(echo $users | jq -r -c 'to_entries | map([.value.user_id] | join(","))')
// ["user123", "user456"]
```

### Find item in list and map field

```jsx
// roles = [{ "name": "OrgAdmin", "id": "role111" }, { "name": "SystemUser", "id": "role222" }]
orgAdminRoleId=$(echo $roles | jq -r '.[] | select(.name=="OrgAdmin") | .id')
// role111
```

### Using variables in filters

```jsx
// users = [{ "name": "Bob", "user_id": "user123" }, { "name": "John", "user_id": "user456" }]
// userName="Bob"
userId=$(echo $users | jq --arg NAME "$userName" '.[] | select(.name==$NAME) | .user_id')
// user123
// IMPORTANT: No double quotes around $NAME in select filter.
// If NAME was hardcoded, it would have double quotes: select(.name=="Bob")
// But this won't work when we're using a --arg variable.
```

### Count number of matches in select filter

```jsx
// users = [{ "name": "Bob", "user_id": "user123" }, { "name": "Bob", "user_id": "user456" }]
// name="Bob"
numUsersWithEmail=$(echo $users | jq -r --arg NAME "$name" '[.[] | select(.name==$NAME)] | length')
// 2
```

### Iterate over JSON object

Map a JSON object and iterate over each item, accessing the fields on each item

```bash
# 1. .fields[] - Select the items in the array named fields.
# 2. select(.label != "notesPlain") - Don't include the built-in field notesPlain which is for plain text notes (think of "select" as "where" or "filter" in other languages).
# 3. map the .label and .section.label fields into a new json object with 'field' and 'section' properties.
fields=$(echo "$itemJson" | $JQ -r '.fields[] | select(.label != "notesPlain") | { field: .label, section: .section.label, ref: .reference }')

echo "$fields" | $JQ -c '.' | while read -r i; do
  field=$(echo "$i" | $JQ -r '.field')
  section=$(echo "$i" | $JQ -r '.section')
  ref=$(echo "$i" | $JQ -r '.ref')

  if [ -z "$section" ] || [ "$section" = "null" ]
  then
    echo "$field -- $ref"
  else
    echo "$section.$field -- $ref"
  fi
done
```
