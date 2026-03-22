# kargo

```
ADMIN_ACCOUNT_TOKEN_SIGNING_KEY=$(openssl rand -base64 48 | tr -d "=+/" | head -c 32)
ADMIN_ACCOUNT_PASSWORD=$(openssl rand -base64 48 | tr -d "=+/" | head -c 32)
ADMIN_ACCOUNT_PASSWORD_HASH=$(htpasswd -bnBC 10 "" $ADMIN_ACCOUNT_PASSWORD | tr -d ':\n')

kc -n base-kargo create secret generic kargo-api-admin \
--from-literal=ADMIN_ACCOUNT_TOKEN_SIGNING_KEY=$ADMIN_ACCOUNT_TOKEN_SIGNING_KEY \
--from-literal=ADMIN_ACCOUNT_PASSWORD=$ADMIN_ACCOUNT_PASSWORD \
--from-literal=ADMIN_ACCOUNT_PASSWORD_HASH=$ADMIN_ACCOUNT_PASSWORD_HASH

echo $(kc -n base-kargo get secret kargo-api-admin -o json | jq -r .data.ADMIN_ACCOUNT_PASSWORD | base64 -d)

kc -n base-kargo port-forward svc/kargo-api 8443:443

kargo login https://localhost:8443/ --admin --insecure-skip-tls-verify
kargo config view
kargo get projects
kargo get warehouses --project=webapp-01
kargo get stages --project=webapp-01
kargo get freight --project=webapp-01
kargo get promotions --project=webapp-01
```
