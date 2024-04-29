### Check if port is occupied

Prints the process that occupies a port:

```bash
lsof -i :3000 | grep LISTEN
```

Silently check the port (no stdout):

```bash
lsof -i :3000 | grep LISTEN > /dev/null
```

Do something if port is occupied or open:

```bash
lsof -i :3000 | grep LISTEN > /dev/null && echo "port occupied" || echo "port is open"
```
