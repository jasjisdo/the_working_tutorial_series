# helm prometheus chart install - The Working Tutorial 2022

Get kubenetes pod status

```
kubectl get pod
```

Get kubenetes pod and helm status (all namespaces)

```
kubectl get pods --all-namespaces
helm ls --all-namespaces
```

Install prometheus helm chart

```
helm install [RELEASE_NAME] prometheus-community/kube-prometheus-stack
```

Install prometheus helm chart with release name `stable`

```
helm install stable prometheus-community/kube-prometheus-stack
```

Get pod status of `release=stable` related pods

```
kubectl --namespace default get pods -l "release=stable"
```

Get kubenetes cluster service status

```
kubectl get svc
```

Get kubenetes cluster pod status

```
kubectl get pod
```

Set portwarding for prometheus pod

```
kubectl --namespace default port-forward prometheus-stable-kube-prometheus-sta-prometheus-0 9090 &
```

Get pod name of grafana. !KEEP IN MIND the pod name of Grafana has two random generated suffixes!

```
export GRAFANA_POD=$(kubectl get pod -l "app.kubernetes.io/name==grafana" -o jsonpath="{.items[0].metadata.name}")
```

Set portwarding for grafana pod

```
kubectl --namespace default port-forward $GRAFANA_POD 3000 &
```

get grafana admin password from kubenetes secret store

```
kubectl get secret --namespace default stable-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Uninstall helm chart `community/kube-prometheus-stack` with release `prometheus`

```
helm uninstall prometheus
```