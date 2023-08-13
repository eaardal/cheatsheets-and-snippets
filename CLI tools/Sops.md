### Dekrypter kubernetes secrets fil med sops og apply til k8s cluster

```bash
cd kubernetes/secrets/rp-test
sops -d secrets.enc.yaml | kubectl --namespace=rp-test apply -f -
```
