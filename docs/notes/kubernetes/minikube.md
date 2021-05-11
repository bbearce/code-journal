# Minikube

## Installation
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

## Start Your Cluster
```bash
minikube start
```

## Interact With Your Cluster
```bash
kubectl get po -A

NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE
kube-system   coredns-74ff55c5b-w8nkf            1/1     Running   0          57m
kube-system   etcd-minikube                      1/1     Running   0          57m
kube-system   kube-apiserver-minikube            1/1     Running   0          57m
kube-system   kube-controller-manager-minikube   1/1     Running   0          57m
kube-system   kube-proxy-tqb79                   1/1     Running   0          57m
kube-system   kube-scheduler-minikube            1/1     Running   0          57m
kube-system   storage-provisioner                1/1     Running   1          58m
```

## Deploy Applications
```bash
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-minikube --type=NodePort --port=8080
```