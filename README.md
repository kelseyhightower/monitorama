# Monitorama

Demo script for Monitorama 2016 talk.

## Setup

* [Demo Setup](labs/setup.md)

## Demo

```
kubectl create -f services/
```

```
kubectl create -f deployments/app.yaml
```

```
kubectl create -f deployments/mysql.yaml
```

```
kubectl create -f deployments/app-canary.yaml
```

```
kubectl create -f deployments/vault.yaml
```
