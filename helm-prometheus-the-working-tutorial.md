# helm prometheus chart install - The Working Tutorial 2022

Starting with a clean and fresh kubernetes environment.

Get kubernetes pod status to verify that current kubernetes environment is fresh and clean.

```
$ kubectl get pod
```
expected output:

![2022-03-12_11h05_01](https://user-images.githubusercontent.com/5826641/158013667-c7e2a4b3-3b86-47ad-9052-1723eb894b85.png)

Get helm chart list to verify that kubernetes environment has no helm charts installed.

```
helm ls
```
expected output:

![2022-03-12_11h10_12](https://user-images.githubusercontent.com/5826641/158013832-d278a8fe-5ed7-4c21-bbee-634fbe45b267.png)

Add helm chart [repo](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) to get the offical [kube-prometheus-stack](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack) and update it to the latest version

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

expected output:

![2022-03-12_11h22_26](https://user-images.githubusercontent.com/5826641/158014238-73bed839-c5be-4c33-835d-a34bd3a31cf7.png)


Install prometheus helm chart [kube-prometheus-stack](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack) with release name `stable`

```
helm install stable prometheus-community/kube-prometheus-stack
```

expected output:

![2022-03-12_11h24_33](https://user-images.githubusercontent.com/5826641/158014288-bab941d8-f071-425b-ac6f-bc1bffe052d8.png)

Get pods status of `release=stable` related new installed pods

```
kubectl --namespace default get pods -l "release=stable"
```

expected output (*Please note that some results may differ as they are randomly generated.*):

![2022-03-12_11h28_56](https://user-images.githubusercontent.com/5826641/158014404-f65e61fe-3da6-4784-a011-9ed11eee639d.png)

Set port forwarding for prometheus pod and run it in background

```
kubectl --namespace default port-forward prometheus-stable-kube-prometheus-sta-prometheus-0 9090 &
bg
```

expected output:

![2022-03-12_11h34_45](https://user-images.githubusercontent.com/5826641/158014605-9bb2928f-9a87-47b8-926a-e1b35a17d0cc.png)

Get pod name of grafana and store it in a bash variable `$GRAFANA_POD`. !KEEP IN MIND the pod name of grafana has two random generated suffixes!

```
export GRAFANA_POD=$(kubectl get pod -l "app.kubernetes.io/name==grafana" -o jsonpath="{.items[0].metadata.name}")
```

Verify that the bash variable `$GRAFANA_POD` is set. !KEEP IN MIND the pod name of grafana has two random generated suffixes!

```
echo $GRAFANA_POD
```

expected output:

![2022-03-12_11h40_48](https://user-images.githubusercontent.com/5826641/158014814-a139b657-c7b7-48ed-b05e-2b940e3295bd.png)

Set port forwarding for grafana pod by using the bash variable `$GRAFANA_POD` and run it in background.

```
kubectl --namespace default port-forward $GRAFANA_POD 3000 &
bg
```

expected output:

![2022-03-12_11h42_50](https://user-images.githubusercontent.com/5826641/158014891-c717c7a0-005c-4ded-8ae4-560f0da72b4a.png)

Get grafana admin password from kubenetes secret store

```
kubectl get secret --namespace default stable-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

expected output:

![2022-03-12_11h44_46](https://user-images.githubusercontent.com/5826641/158014958-91812266-ba1c-4e25-a220-baff7de035ec.png)

Visit the prometheus web UI by using a browser and visit `http://localhost:9090/`

![2022-03-12_11h50_30](https://user-images.githubusercontent.com/5826641/158015089-aecd473f-6ec3-4ffd-825a-73978f3c9441.png)

Visit the grafane web UI by using a browser and visit `http://localhost:3000/`

login with user: `admin` and password: `prom-operator`

![2022-03-12_11h53_42](https://user-images.githubusercontent.com/5826641/158015165-9e40bb65-c257-4b86-8796-173a578dc3c9.png)

---

Uninstall helm chart `community/kube-prometheus-stack` with release `prometheus`

```
helm uninstall stable
```
