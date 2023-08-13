### Keygen

```bash
age-keygen -o key.txt
# public key == "recipient"
```

### Encrypt

Recipient

```bash
age -e FILE -r RECEIPIENT
```

Password

```bash
age -e FILE -p PASSPHRASE
```

### Decrypt

Using key

```bash
age -d -i path/to/key.txt file.yaml.age > file.yaml
```

Using password

```bash
age -d -p PASSPHRASE file.yaml.age > file.yaml
```
