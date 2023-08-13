### Base64

Base64-enkode innholdet i en fil og legge det på clipboard

```bash
cat {file} | base64 | pbpaste
```

### Kill Process

https://stackoverflow.com/questions/3855127/find-and-kill-process-locking-port-3000-on-mac

```bash
lsof -t -i tcp:{port} | xargs kill
```

### Process substitution

- http://www.gnu.org/software/bash/manual/bash.html#Process-Substitution

```bash
diff <(cat old) <(cat new)
```
