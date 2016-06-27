# Monitorama Demo Setup

### Database

```
gcloud compute disks create mysql-data
```

```
kubectl create -f deployments/mysql.yaml
```

```
kubectl port-forward <mysql-pod> 3306:3306
```

```
mysql -h 127.0.0.1 -u root
```

```
CREATE DATABASE app;
```

```
CREATE USER 'app'@'%' IDENTIFIED BY 'm0n1t0rAmA';GRANT ALL PRIVILEGES ON app.* TO 'app'@'%';
```

### Vault

```
gcloud compute disks create vault-data
```

```
kubectl create configmap vault-conf --from-file=configs/vault.hcl
```

```
kubectl create -f deployments/vault.yaml
```

```
kubectl get pods
```

```
kubectl exec --tty -i <vault-pod> /bin/bash
```

```
export VAULT_ADDR="http://127.0.0.1:8200"
```

```
vault init
```

```
vault unseal
```

```
vault status
```

```
vault auth <root-token>
```

```
vault policy-write app app-policy.hcl
```

```
vault token-create \
  -policy="app" \
  -display-name="app"
```

```
kubectl create secret generic app-vault-token \
  --from-literal='vault-token=<app-token>'
```

```
vault mount mysql
```

```
vault write mysql/config/connection \
  connection_url="root:m0n1t0rAmA@tcp(mysql:3306)/"
```

```
vault write mysql/roles/app \
  sql="CREATE USER '{{name}}'@'%' IDENTIFIED BY '{{password}}';GRANT ALL PRIVILEGES ON app.* TO '{{name}}'@'%';"
```

### Kubernetes Secrets

```
kubectl create secret generic mysql \
  --from-literal='root-password=m0n1t0rAmA'
```

```
kubectl create secret generic app \
  --from-literal='database-password=m0n1t0rAmA'
```
