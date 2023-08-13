### Kopiere en secret fra et namespace til et annet namespace i samme cluster

```bash
kubectl get secret sa-transaction-engine-key --namespace=default --export -o yaml | kubectl apply --namespace=test -f -
```

### Create new secret

```bash
kubectl create secret generic {name} \
--from-literal subscription_key="foo" \
--from-literal oauth_resource="foo" \
--from-literal oauth_client_id="foo" \
--from-literal oauth_client_secret="foo"
```

### Update existing secret

```bash
kubectl create secret generic {name} -n prod \
--from-literal subscription_key="bar" \
--from-literal oauth_resource="bar" \
--from-literal oauth_client_id="bar" \
--from-literal oauth_client_secret="bar" \
-o yaml --dry-run | kubectl replace -f -
```

### Dry run

```bash
kubectl create secret generic test-secret \
--from-literal subscription_key="foo" \
--from-literal oauth_resource="foo" \
--from-literal oauth_client_id="foo" \
--from-literal oauth_client_secret="foo"
-o yaml --dry-run
```

### Lese ut secret

Output er base64 enkodet så hver secret må dekodes for å få ut den egentlige verdien:

```bash
kubectl get secret my-secret -o yaml
```

### Hente ut siste 100 logger fra flere podder samtidig

```bash
kubectl -n <namespace> logs deployment/<app-name> --all-containers=true --tail=100
```

### Følge logs fra alle podder

```bash
kubectl -n <namespace> logs -f deployment/<app-name> --all-containers=true --since=10m
```
