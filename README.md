# prometheus-gke

### create monitoring namespace

```
$ kubectl apply -f namespace.yml
```

### RBAC
google cloud - Role-Based Access Control

```
$ kubectl create clusterrolebinding cluster-admin-binding \
--clusterrole cluster-admin \
--user $(gcloud config get-value account)
```

```
$ kubectl create serviceaccount monitor --namespace monitoring
```

```
$ kubectl apply -f https://github.com/seo-3/prometheus-gke/role.yml
```

```
$ kubectl create clusterrolebinding monitor --clusterrole=all-reader --serviceaccount=monitoring:monitor
```

### Configmap

```
$ kubectl -n monitoring apply -f https://github.com/seo-3/prometheus-gke/prometheus-configmap.yml
```

```
$ kubectl -n monitoring apply -f https://github.com/seo-3/prometheus-gke/alertmanager-configmap.yaml
```

### Volume
```
$ kubectl -n monitoring apply -f https://github.com/seo-3/prometheus-gke/volume.yml
```

### Grafana config
set grafana admin user password.

```
$ kubectl create secret generic gf-security-admin-password --from-literal=password=<YOUR_ADMIN_PASSWORD> -n=monitoring
```

### Deployments
Include grafana and prometheus in deployment.

```
$ kubectl -n monitoring apply -f https://github.com/seo-3/prometheus-gke/prometheus-deployment.yml
```

### DaemonSet

```
$ kubectl -n monitoring apply -f https://github.com/seo-3/prometheus-gke/daemonset.yaml
```


### Service

```
$ kubectl -n monitoring apply -f https://github.com/seo-3/prometheus-gke/prometheus-service.yml
```

### Ingress

```
$ kubectl -n monitoring apply -f https://github.com/seo-3/prometheus-gke/ingress.yml
```
