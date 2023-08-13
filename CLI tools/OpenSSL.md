### Nøkkelpar og sertifikat generering

Lage privatnøkkel:

```bash
openssl genrsa -out private_key.pem 4096
```

Lage sertifikat basert på privatnøkkel:

```bash
openssl req -new -x509 -key private_key.pem -out ca.crt
```

…som utløper om 1 år:

```bash
openssl req -new -x509 -key private_key.pem -out ca.crt -days 365
```

Skrive ut sertifikat:

```bash
echo `cat ca.crt | sed '1,1d;$ d' | tr -d '\\n'`
```

Lage publickey fra privatekey:

```bash
openssl rsa -in private_key.pem -pubout > public_key.pub
```

Erstatte line breaks med \n (ikke ta med trailing \n):

```bash
awk '{printf "%s\\\\n", $0}' private_key.pem
```

Base64 enkode key:

```bash
base64 -i private_key.pem
```

### Bruke key som miljøvariabel

1. Lage privatnøkkel (maks 2048)
2. Base64 enkode nøkkelen
3. Bruke base64 enkodet nøkkel som miljøvariabel
4. I config i kode, decode base64 til plain text og bruk som vanlig

Base64 enkode innholdet i en fil og legge på clipboard:

```bash
cat {file} | base64 | pbpaste
```

### Sammenligne cert med privatnøkkel

https://www.sslshopper.com/certificate-key-matcher.html

```bash
$openssl pkey -in ~/dev/certs/subaio/tls/prod/private_key.pem -pubout -outform pem | sha256sum
$openssl x509 -in ~/dev/certs/subaio/tls/prod/ca.crt -pubkey -noout -outform pem | sha256sum
```
