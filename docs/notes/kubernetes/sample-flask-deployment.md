# Flask_Deploy

We're going to document this [demo](https://github.com/HoussemDellai/kubernetes-ingress-tls-ssl-https). First we need to create an aks cluster.

> [Source](https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-cluster)

> Be sure to login before beginning: ```$ az login```

## Create resource group
```bash
myResourceGroup="flask_demo"
location="eastus"
az group create --name $myResourceGroup --location $location
```

## Create azure container registry
```bash
az acr create --resource-group $myResourceGroup \
  --name bbearcecontainerregistry --sku Basic
```

## Create azure kubernetes cluster

By default, the Azure CLI automatically enables RBAC (Role Based Access Control) when you create an AKS cluster

```bash
myAKSCluster="flaskAKSCluster"
acrName="bbearcecontainerregistry"

az aks create \
    --resource-group $myResourceGroup \
    --name $myAKSCluster \
    --node-count 1 \
    --generate-ssh-keys \
    --attach-acr $acrName
```

## Install ```kubectl```

If not already installed, install with:

```bash
az aks install-cli
```

## Connect to cluster using kubectl

```bash
az aks get-credentials --resource-group $myResourceGroup --name $myAKSCluster
```

Test connection:

```bash
kubectl get nodes
```

Now we are all setup to begin the real tutorial:

## Kubernetes Ingress with TLS/SSL

git clone this [project](https://github.com/HoussemDellai/kubernetes-ingress-tls-ssl-https).

You should have these files:

* app-namespace.yaml  
* app1-deploy-svc.yaml  
* app1-deploy-svc.yaml  
* app-ingress.yaml  
* ssl-tls-cluster-issuer.yaml  
* ssl-tls-ingress.yaml  

### Dockerfile
Build an image for deployment:

Dockerfile:
```bash
FROM python:latest

WORKDIR /app

COPY app.py .
COPY requirements.txt .

RUN pip install -r requirements.txt

ENV FLASK_APP=app.py

ENTRYPOINT ["flask","run"]
CMD ["--host","0.0.0.0", "--port", "5000"]
```

### Build

Build Step:
```bash
docker build -t bbearcecontainerregistry.azurecr.io/flaskapp .
```

### Push

Login:
```bash
az acr login -n bbearcecontainerregistry
```

Push:
```bash
docker push bbearcecontainerregistry.azurecr.io/flaskapp
```

### Deploy


#### App
Set these files up to get ready for deployment:
> I editied the app-deploy to this from the two that are default:

> It's important to run this file with ```LoadBalancer``` as the ```type``` in the ```Service``` section.

**app-deploy-svc.yaml**:
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp-deploy
  namespace: app 
spec:
  selector:
    matchLabels:
      app: webapp
  replicas: 1
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: bbearcecontainerregistry.azurecr.io/flaskapp
        ports:
        - containerPort: 5000
---
kind: Service
apiVersion: v1
metadata:
  name: webapp-svc
  namespace: app
spec:
  selector:
    app: webapp
  ports:
  - port: 80
  type: LoadBalancer # ClusterIP # NodePort # 
```

#### Ingress
Then I edited the **app-ingress.yaml** to this **app-ingress-modified.yaml**:

```bash
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: webapp-ingress
  namespace: app
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: webapp-svc
          servicePort: 80
        path: /
  - host: front.end.20.185.73.238.nip.io # change the IP address here

```

Now deploy everything. Note that we delete and re-apply the **app-deploy-svc.yaml** with ```type: ClusterIP```.


app-namespace.yaml:
```bash
apiVersion: v1
kind: Namespace
metadata:
  name: app
```

```bash
# Create a namespace for the apps
kubectl apply -f app-namespace.yaml

# Deploy flask app
kubectl apply -f app-deploy-svc.yaml

# Deploy the 2 sample apps into Kubernetes to follow original demo
#kubectl apply -f app1-deploy-svc.yaml 
#kubectl apply -f app2-deploy-svc.yaml

# Get the 2 public IP addresses 
kubectl get services --namespace app

# Add the Helm chart for Nginx Ingress
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

# Install the Helm (v3) chart for nginx ingress controller
# (If using Bash instead of Powershell, replace ` with \)
helm install app-ingress ingress-nginx/ingress-nginx \
     --namespace ingress \
     --create-namespace \
     --set controller.replicaCount=2 \
     --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
     --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux

# Get the Ingress Controller public IP address
kubectl get services --namespace ingress

# Update the service type to ClusterIP instead of LoadBalancer, ignore if it was always ClusterIP 
# in app-deploy-svc.yaml file
# Delete and redeploy the service for the update to take effect
kubectl delete -f app-deploy-svc.yaml 
kubectl apply -f app-deploy-svc.yaml 

# This is the original tutorial:
# kubectl delete -f app1-deploy-svc.yaml 
# kubectl delete -f app2-deploy-svc.yaml
# kubectl apply -f app1-dekubectl apply -f app-ingress-modified.yaml ploy-svc.yaml 
# kubectl apply -f app2-deploy-svc.yaml

# Deploy the Ingress resource into Kubernetes
kubectl apply -f app-ingress-modified.yaml 

# Cleanup resources
kubectl delete -f app-deploy-svc.yaml 
kubectl delete -f app-deploy-svc.yaml
kubectl delete -f app-namespace.yaml
helm delete app-ingress --namespace ingress
kubectl delete namespace ingress
```

## Cert Manager

In this second part of the lab, we will enable HTTPS in Kubernetes using Cert Manager and Lets Encrypt. The Cert Manager is used to automatically generate and configure Let's Encrypt certificates.

### Create a namespace for Cert Manager

```bash
kubectl create namespace cert-manager
```

### Get the Helm Chart for Cert Manager
```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

### Install Cert Manager using Helm charts

> Use option [3], [1] and [2] didn't work for me (10/27/2020)

[1]
 > Warning
> Install CRDs manually for v0.14.0
```bash
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.14.1/cert-manager.crds.yaml
```

Then install cert-manager
```bash
helm install cert-manager jetstack/cert-manager \
    --namespace cert-manager \
    --set installCRDs=true \
    --version v0.14.0
```

or maybe:

[2]
 > Warning
Label the cert-manager namespace to disable resource validation
```bash
kubectl label namespace ingress cert-manager.io/disable-validation=true
```
```bash
helm install \
  cert-manager \
  --namespace ingress \
  --version v0.16.1 \
  --set installCRDs=true \
  --set nodeSelector."beta\.kubernetes\.io/os"=linux \
  jetstack/cert-manager
```

or maybe

[3] Installing with regular manifests (tried at 8:15pm)
> [source](https://cert-manager.io/v0.16-docs/installation/kubernetes/)

> Note: If you’re using a kubectl version below v1.19.0-rc.1 you will have issues updating the CRDs. For more info see the v0.16 upgrade notes

Kubernetes 1.15+:
```bash
$ kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.16.1/cert-manager.yaml
```



### Check the created Pods
```bash
kubectl get pods --namespace cert-manager
```

### Install the Cluster Issuer
> WARNING, you must change the email address to yours. The one in there might work if it is live, but only real email addresses are allowed.

> Also: Must install CRDs (disregard if you used step [3] during cert-manager install)

> What worked best for me was using the staging cert to practice and then the real one. I believe there is a limit for real certs so you don't want to make them make you wait for another real one to issue while you work out the kinks. 


Make sure ```ssl-tls-cluster-issuer.yaml``` has ```-staging``` appended to the ```letsencrypt```  authority.

sst-tls-cluster-issuer.yaml:
```bash
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: bbearce@gmail.com
    privateKeySecretRef:
      name: letsencrypt-staging
    solvers:
    - http01:
        ingress:
          class: nginx
```

Apply cluster issuer:
```bash
kubectl apply --namespace app -f ssl-tls-cluster-issuer.yaml
```

### Install the Ingress resource configured with TLS/SSL

[1] remove old non-https ingress *app-ingress-modified.yaml*
```bash
kubectl delete -f app-ingress-modified.yaml
```

Then apply new ssl-tls for https certs. This is the same ingress yaml but we changed the parameters to make a more secure ingress.
```bash
kubectl apply --namespace app -f ssl-tls-ingress.yaml
```


[2]
sync this deploy with the cert-manager by using ```-staging```:

```bash
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ssl-tls-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-staging
spec:
  tls:
    - hosts:
      - front.end.52.190.30.229.nip.io # update IP address here
      secretName: app-web-cert
  rules:
  - host: front.end.52.190.30.229.nip.io # update IP address here
    http:
      paths:
      - backend:
          serviceName: webapp-svc
          servicePort: 80
        path: /
```

### Verify that the certificate was issued

```bash
kubectl get certificate --namespace app
```

```bash
kubectl describe cert app-web-cert --namespace app
```

If all this works go back and edit ```ssl-tls-ingress.yaml``` and ```ssl-tls-cluster-issuer.yaml``` and remove ```-staging``` from ```letsencrypt``` and re-apply them.

ssl-tls-ingress.yaml:
```bash
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ssl-tls-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
    - hosts:
      - front.end.52.190.30.229.nip.io # update IP address here
      secretName: app-web-cert
  rules:
  - host: front.end.52.190.30.229.nip.io # update IP address here
    http:
      paths:
      - backend:
          serviceName: webapp-svc
          servicePort: 80
        path: /

```

ssl-tls-cluster-issuer.yaml:
```bash
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: bbearce@gmail.com
    privateKeySecretRef:
      name: letsencrypt
    solvers:
    - http01:
        ingress:
          class: nginx
```

Re-apply new ingress and cluser issuer:
```bash
kubectl apply --namespace app -f ssl-tls-ingress.yaml
kubectl apply --namespace app -f ssl-tls-cluster-issuer.yaml
```

### Check the services
```bash
kubectl get services -n app
```

Now test the app with HTTPS: https://frontend.&lt;ip-address&gt;.nip.io
