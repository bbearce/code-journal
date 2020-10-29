# Sample Django Deployment

> Source [medium](https://medium.com/boilercode/deploying-a-django-application-in-azure-aks-with-an-ingress-controller-and-tls-266a1e2dc697)

## Stack-Scope

As I already revealed we are going to deploy a Django web application on a managed Kubernetes cluster on Azure Kubernetes Service. The Django application is written in Python 3, and built locally with Docker. To manage some Azure resources, like Azure Container Registry (ACR), Load Balancer, static IP and the AKS cluster itself we will use the Azure CLI. In order to manage deployments and services within the cluster we use kubectl, the official Kubernetes command-line tool. For installing the NGINX Ingress controller and setting up Jetstack/Cert-Manager we utilize the power of Helm 3, a Kubernetes package manager.

* python3
* docker
* Azure Container Registry (ACR)
* Load Balancer
* Azure CLI
* kubectl
* helm 3



## Kubernetes Concepts

Before we roll up our sleeves, let’s review some of the main Kubernetes concepts. Make sure to peek at the official documentation if you have some spare time, to obtain a better understanding of the various parts and abstractions of Kubernetes.

* Pod — A Pod represents a single instance of an application in Kubernetes and is the basic used unit of deployment. In common use cases the “one-container-per-pod” model is the way to go, but you can also run multiple containers in a single Pod. You can think of the Pod as a wrapper around your — Docker — container which describes it’s resources, network configuration and other options.

* Service — A Service in Kubernetes is an abstract networking layer for multiple Pods. It has the ability to target multiple Pods by their label and defines the policy of how Pods can access each other, also known as microservice context management. The Service can be given an unique DNS name as a single Service internal access point.

* ReplicaSet — A ReplicaSet describes how many replica Pods of a specific Pod should be running at any given time. With the replica Pods of your application running you can make sure that your service will be available when one of the Pods crashes. Because a Kubernetes Deployment is a higher-level conceptual controller that manages ReplicaSets, you will most likely not use this controller directly in your projects. However, abstract knowledge about this concept helps understanding the basic concepts and utilities of Kubernetes.

* Deployment — A Deployment describes a desired state and can be applied as declarative update or release for Pods and ReplicaSets. Wait what? Imagine that your Django web application is running in a Pod on your Kubernetes cluster. When you decide to update your application with a newer version of your code, you can declare the new desired state — which most likely points to an updated container image — in a Deployment description which you can apply in your cluster. The native Kubernetes Deployment Controller will detect the changes and will handle the rollout by changing the current state of a Pod or ReplicaSet to the desired state.

* Ingress — An Ingress describes the routing rules for your cluster. It is an advanced routing layer between your cluster services and the internet. Next to the Ingress you need an Ingress controller to satisfy the rules that you described in the Ingress. Together they form a very powerful couple to expose your services. An Ingress also has many advantages relative to using other external traffic routing methods i.e. the primitive NodePort or costly LoadBalancer.

* Node — Last but not least, a Node represents a virtual or physical machine where Kubernetes will run your Pods’ container. Most of the time the nodes will be Linux machines, however Azure offers the possibility now to use Windows machines as well!
Excellent! We learned to walk, now we can start to run!




## Step 1: Build Locally

Clone:
```bash
$ git clone https://github.com/wmarcuse/django-azure-aks-ingress.git
```

Build:
```bash
$ docker build -t bbearce/django-aks:v1.0.0 .
```

Run:
```bash
$ docker run --rm -it -p 8010:8010 bbearce/django-aks:v1.0.0
```

Visit:
```bash
$ Visit http://127.0.0.1:8010 
```

## Step 2: Manage ACR

### Login to Azure:

```bash
$ az login

Opening in existing browser session.
[1009/183807.113943:ERROR:nacl_helper_linux.cc(308)] NaCl helper process running without a sandbox!
Most likely you need to configure your SUID sandbox correctly
You have logged in. Now let us find all the subscriptions to which you have access...
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "####################################",
    "id": "####################################",    
    "isDefault": true,
    "managedByTenants": [],
    "name": "Pay-As-You-Go",
    "state": "Enabled",
    "tenantId": "####################################",
    "user": {
      "name": "someone@mail.com",
      "type": "user"
    }
  }
]

```

### Create a resource group:

```bash
$ az group create --name djangoAKS --location westeurope
djangoAKS --location westeurope
{
  "id": "/subscriptions/####################################/resourceGroups/djangoAKS",
  "location": "westeurope",
  "managedBy": null,
  "name": "djangoAKS",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}

```

### Create the ACR resource

Create the ACR resource in the newly created resource group specifying the basic [service level](https://docs.microsoft.com/nl-nl/azure/container-registry/container-registry-skus):

```bash
$ az acr create --resource-group djangoAKS --name djangoAksRegistryBB --sku Basic

{- Finished ..
  "adminUserEnabled": false,
  "creationDate": "2020-10-09T22:45:49.761838+00:00",
  "dataEndpointEnabled": false,
  "dataEndpointHostNames": [],
  "encryption": {
    "keyVaultProperties": null,
    "status": "disabled"
  },
  "id": "/subscriptions/####################################/resourceGroups/djangoAKS/providers/Microsoft.ContainerRegistry/registries/djangoAksRegistryBB",
  "identity": null,
  "location": "westeurope",
  "loginServer": "djangoaksregistrybb.azurecr.io",
  "name": "djangoAksRegistryBB",
  "networkRuleSet": null,
  "policies": {
    "quarantinePolicy": {
      "status": "disabled"
    },
    "retentionPolicy": {
      "days": 7,
      "lastUpdatedTime": "2020-10-09T22:45:54.821697+00:00",
      "status": "disabled"
    },
    "trustPolicy": {
      "status": "disabled",
      "type": "Notary"
    }
  },
  "privateEndpointConnections": [],
  "provisioningState": "Succeeded",
  "publicNetworkAccess": "Enabled",
  "resourceGroup": "djangoAKS",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "systemData": {
    "createdAt": "2020-10-09T22:45:49.7618387+00:00",
    "createdBy": "someone@mail.com",
    "createdByType": "User",
    "lastModifiedAt": "2020-10-09T22:45:49.7618387+00:00",
    "lastModifiedBy": "someone@mail.com",
    "lastModifiedByType": "User"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}

```

Take note of loginServer: ```djangoaksregistrybb.azurecr.io```.

### Pushing the application to ACR:

Login:
```bash
$ az acr login --name djangoaksregistrybb
```

Tag image:
```bash
$ docker tag bbearce/django-aks:v1.0.0 djangoaksregistrybb.azurecr.io/django-aks:v1.0.0
```

Push image:
```bash
$ docker push djangoaksregistrybb.azurecr.io/django-aks:v1.0.0
```


## Step 3: Setting up a Kubernetes cluster with AKS

### Creating the AKS cluster

We specify a few parameters to let Azure know how we want to configure the Kubernetes cluster. We make sure that the AKS cluster is put in the same resource group as the ACR registry. 

```bash
$ az aks create \
  --resource-group djangoAKS \
  --name djangoaks-cluster \
  --node-count 2 \
  --node-vm-size Standard_B2s \
  --generate-ssh-keys \
  --kubernetes-version 1.16.8
```

> In western Europe (my resource location) kubernetes version 1.16.8 wasn't available. Using ```az aks get-versions --location westeurope```, you can see what versions are available.

So instead I used:

```bash
$ az aks create \
  --resource-group djangoAKS \
  --name djangoaks-cluster \
  --node-count 2 \
  --node-vm-size Standard_B2s \
  --generate-ssh-keys \
  --kubernetes-version 1.19.0
```
> Also there is a section and or flag related to supplying your own ssh key. 


Brief example which doesn't apply to this tutorial as the ```--generate-ssh-keys``` worked for me...

```bash
$ ssh-keygen -f ~/.ssh/django-aks-ssh
```

Then create the AKS cluster with almost the same command as we tried before but now pointing towards the manually created SSH key.

```bash
$ az aks create \
  --resource-group djangoAKS \
  --name djangoaks-cluster \
  --node-count 2 \
  --node-vm-size Standard_B2s \
  --ssh-key-value ~\.ssh\django-aks-ssh.pub \
  --kubernetes-version 1.16.8
```

moving on...

### Connecting to the AKS cluster

```bash
$ az aks install-cli
```

Now configure kubectl to connect to your AKS cluster, the credentials will be downloaded on the background and the context of the Kubernetes command-line tool will be set to your cluster.
```bash
$ az aks get-credentials --resource-group djangoAKS --name djangoaks-cluster
```

Verify that the connection from your development machine to the AKS cluster is working by checking the status of the nodes you created previously.
```bash
$ kubectl get nodes
```

### Namespace

Now as your first act as cluster-manager, create a Kubernetes namespace for the resources we are going to create in the next steps.

```bash
$ kubectl create namespace djaks
```

## Step 4: Set up Helm 3

### Install Helm 3

Helm 3 is a package manager for Kubernetes and helps you manage applications on your cluster. Helm 3 works with so called ‘charts’. *A chart describes the application resources and provides easy accessible and repeatable application installations, as well as very clever in-place application upgrades.*
We will be needing Helm 3 to install Jetstack/Cert-Manager and the NGINX Ingress controller. Follow the official installation guide. I highly recommend Windows users to install Helm 3 with Chocolatey, as to prevent some first time installation problems.

We will be needing Helm 3 to install Jetstack/Cert-Manager and the NGINX Ingress controller. Follow the official [installation guide](https://helm.sh/docs/intro/install/). I highly recommend Windows users to install Helm 3 with [Chocolatey](https://chocolatey.org/install), as to prevent some first time installation problems.

### Manage Helm 3

Helm 3 has a variety of pre-created charts available in the official stable charts repository. Add the stable repository to your Helm 3 installation.
```bash
$ helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

> Also adding:

```bash
$ helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

Now update the list of charts you just added to your Helm 3 installation.

```bash
$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "ingress-nginx" chart repository
...Successfully got an update from the "jetstack" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈
```

## Step 5: Configure additional Azure resources

When we created the AKS cluster, Azure automatically created an additional resource group where it attaches specific infrastructural resources like virtual machine scale sets, virtual networks and managed disks. This extra resource group is also known as the node resource group. When you delete the AKS cluster, this node resource group is deleted as well. The resource group follows the following name convention: **MC_&lt;resourceGroupName&gt;\_&lt;clusterName&gt;\_&lt;Region&gt;.**

### Create a static IP address

Let’s find out what the exact name of the node resource group is.
```bash
$ az aks show --resource-group djangoAKS --name djangoaks-cluster --query nodeResourceGroup -o tsv
MC_djangoAKS_djangoaks-cluster_westeurope
```

Now create the public IP:
```bash
$ az network public-ip create \
--resource-group MC_djangoAKS_djangoaks-cluster_westeurope \
--name djangoAKSPublicIP \
--sku Standard \
--allocation-method static \
--query publicIp.ipAdress \
-o tsv
```

Now let’s find out what the actual IP address is. The static IPv4 address should be visible under ipAddress.

```bash
$ az network public-ip show \
--resource-group MC_djangoAKS_djangoaks-cluster_westeurope \
--name djangoAKSPublicIP



...
"ipAddress": "51.105.159.17",
...
"name": "djangoAKSPublicIP",
...
```

### Connect AKS with ACR with Azure AD

As we host some of our application container images in ACR, it is essential that the AKS cluster can communicate with the container registry. When we configure the communication between the two services, Active Directory will handle the authentication on the background.

```bash
$ az aks update -n djangoaks-cluster -g djangoAKS --attach-acr djangoaksregistrybb
```

## Step 6: Create the NGINX Ingress controller

It is time to start using the magic from Helm 3. We can instantly deploy an nginx-ingress chart that is already available in de stable Helm repository!

Here I'm trying something I found in Patrick's deploy_codalab.sh and sort of [here](https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx/3.7.0#configuration):

```bash
$ helm install --namespace djaks nginx-ingress nginx-stable/nginx-ingress --set controller.replicaCount=2 --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"="django-aks-ingress" --set controller.service.loadBalancerIP="51.105.159.17"
```
another version from this [tutorial](https://github.com/HoussemDellai/kubernetes-ingress-tls-ssl-https):

```bash
$ helm install app-ingress ingress-nginx/ingress-nginx \
     --namespace ingress \
     --create-namespace \
     --set controller.replicaCount=2 \
     --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
     --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

optionally:

```bash
$ helm install app-ingress ingress-nginx/ingress-nginx \
     --namespace ingress \
     --create-namespace \
     --set controller.replicaCount=2 \
     --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
     --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
     --set controller.service.loadBalancerIP="51.105.159.17" \
     --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"="django-aks-ingress"
```

This avoids the WARNING I show below...

Below is original instructions from post I'm copying:

* Make sure that you replace the IP address in the command below under ```--set controller.service.loadBalancerTP``` with the static IP address you created in the previous step.
* Specify your preferable DNS name label under the ```--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"``` Azure will create a Fully Qualified Domain Name (FQDN) with the provided DNS prefix.
* Note that we install the Ingress controller in the earlier created namespace ```djaks```.
```bash
$ helm install nginx-ingress stable/nginx-ingress \
  --namespace djaks \
  --set controller.replicaCount=2 \
  --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
  --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
  --set controller.service.loadBalancerIP="51.105.159.17" \
  --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"="django-aks-ingress"
```
> Note if that above doesn't work, here it is flattened:
```bash
$ helm install nginx-ingress stable/nginx-ingress   --namespace djaks  --set controller.replicaCount=2  --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux  --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux  --set controller.service.loadBalancerIP="51.105.159.17"  --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"="django-aks-ingress"
```

> Temp: Output from my run of the above command...meant to be deleted at some point, but I'm doing the tutorial as we speak.
```
WARNING: This chart is deprecated
NAME: nginx-ingress
LAST DEPLOYED: Mon Oct 12 09:27:49 2020
NAMESPACE: djaks
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
*******************************************************************************************************
* DEPRECATED, please use https://github.com/kubernetes/ingress-nginx/tree/master/charts/ingress-nginx *
*******************************************************************************************************


The nginx-ingress controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace djaks get services -o wide -w nginx-ingress-controller'

An example Ingress that makes use of the controller:

  apiVersion: extensions/v1beta1
  kind: Ingress
  metadata:
    annotations:
      kubernetes.io/ingress.class: nginx
    name: example
    namespace: foo
  spec:
    rules:
      - host: www.example.com
        http:
          paths:
            - backend:
                serviceName: exampleService
                servicePort: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
        - hosts:
            - www.example.com
          secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
```

You can monitor the progress to see when the EXTERNAL-IP is bound to the ```nginx-ingress-controller```. Use CTRL-C to stop the monitoring.

update:
```bash
$ kubectl --namespace djaks get services -o wide -w nginx-ingress-ingress-nginx-controller

```

old:
```bash
$ kubectl --namespace djaks get services -o wide -w nginx-ingress-nginx-ingress
```

> **FDQN** stands for fully qualified domain name.

Azure has created a FQDN when we created the Ingress controller, we can access our service via this domain as well. Let’s find out what the domain name is.

```bash
$ az network public-ip list \
  --resource-group MC_djangoAKS_djangoaks-cluster_westeurope \
  --query "[?name=='djangoAKSPublicIP'].[dnsSettings.fqdn]" -o tsv
```

Awesome! Our service is available via ```django-aks-ingress.westeurope.cloudapp.azure.com``` as well!

## Step 7: Manage Jetstack/Cert-Manager

This was not in original tutorial but I will add a cert-manager namespace for the cert-manager pods:

```bash
$ kubectl create namespace cert-manager
```

### Install Jetstack/Cert-Manager

> very useful [resource-HoussemDellai](https://github.com/HoussemDellai/kubernetes-ingress-tls-ssl-https)

> His [video](https://www.youtube.com/watch?v=M7t0cpQyzKA)

In step 4 we configured Helm 3 and added their stable chart repository, in this repository resides a cert-manager chart as well. But this version is depreciated, therefore use the official chart repository of jetstack.io.
```bash
$ helm repo add jetstack https://charts.jetstack.io
```

Update the local Helm chart repository cache to fetch any updates.

> I was told I was up to date so...

```bash
$ helm repo update
"jetstack" already exists with the same configuration, skipping
```

We have to apply the [Custom Resource Definitions](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) (CRDs) to the cluster as part of the Helm 3 release.

Note that it is important that you have the earlier defined Kubernetes version 1.16.8+ running on your cluster. Also don’t apply the CRDs manually to your cluster, they can have issues with custom namespace names.
Install Jetstack/Cert-Manager.
```bash
$ helm install \
  cert-manager jetstack/cert-manager \
  --namespace djaks \
  --version v1.0.2 \
  --set installCRDs=true

$ helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.0.0 \
  --set installCRDs=true

$ helm install \
  cert-manager jetstack/cert-manager \
  --namespace djaks \
  --set installCRDs=true
```

You can verify that the installation was successful by checking the namespace for running pods. You should see **three** cert-manager pods running!
```bash
$ kubectl get pods --namespace djaks
```

### Create an ACME ClusterIssuer

ACME stands for Automated Certificate Management Environment. As we want to issue the certificates for the scope of the cluster, let’s use a ClusterIssuer resource. You can find more on Issuers [here](https://cert-manager.io/docs/concepts/issuer/). Let’s checkout the ClusterIssuer manifest file.

```bash
# k8s/cluster-issuer.yaml
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  # name: letsencrypt-staging
  name: letsencrypt-prod
spec:
  acme:
    email: user@example.com
    # server: https://acme-staging-v02.api.letsencrypt.org/directory
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      # name: letsencrypt-staging
      name: letsencrypt-prod
    solvers:
    - http01:
        ingress:
          class: nginx
```

Create the ClusterIssuer in your cluster:

```bash
$ kubectl apply -f cluster-issuer.yaml --namespace djaks
```

> Experimental command:

```bash
$ kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.2/cert-manager.yaml
```

```bash
kubectl delete secret {NAME OF THE SECRET NAMED ON THE CERTIFICATE HERE}
```

...

Next I listed the ingresses with,

```bash
$ kubectl --namespace djaks get ing
NAME                     HOSTS             q                                 ADDRESS                 PORTS     AGE
ingress-resource-rules   django-aks-ingress.westeurope.cloudapp.azure.com   10.240.0.4,10.240.0.5   80, 443   9m48s

```

and certificates with


```bash
$ kubectl get all --namespace cert-manager

```

```bash
$ kubectl --namespace djaks describe certificate tls-secret
```

```bash
$ kubectl --namespace djaks describe certificaterequest tls-secret-skgpp
```

```bash
kubectl --namespace djaks describe clusterissuer letsencrypt-prod
```

```bash
helm --namespace ingress uninstall nginx-ingress
```

....



## Step 8: Deploy the Django application

### Apply the Deployment manifest

> Note, be sure to change ```image:``` in yours...

```bash
# k8s/webapp-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: django-web-app
  labels:
    app: django-web
spec:
  replicas: 2
  selector:
    matchLabels:
      app: django-web
  template:
    metadata:
      labels:
        app: django-web
    spec:
      containers:
        - name: django-web-container
          imagePullPolicy: Always
          image: djangoaksregistrybb.azurecr.io/django-aks:v1.0.0
          ports:
          - containerPort: 8010
          env:
            - name: HOST_IP
              valueFrom:
                fieldRef:
                  fieldPath: status.hostIP
      imagePullSecrets:
        - name: acr-secret
```

Apply the Deployment manifest to the cluster.
```bash
$ kubectl apply -f webapp-deployment.yaml --namespace djaks
```

You can verify that the django-web-app Pods are running correctly by checking them out:

```bash
kubectl get pods --namespace djaks
```

If something went wrong you can look at the events that happened in the Pod by asking kubectl to describe the Pod. You need to specify a specific Pod name of the Pods you listed in the previous command.
```bash
$ kubectl describe pod django-web-app-#########-##### --namespace djaks
```

### Apply the Service manifest

```bash
# k8s/webapp-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: webapp
spec:
  type: ClusterIP
  ports:
  - port: 80
    targetPort: 8010
  selector:
    app: django-web
```

```bash
$ kubectl apply -f webapp-service.yaml --namespace djaks
```

The service is exposed on an internal IP with the ClusterIP ServiceSpec. This means that the service is only accessible from within the cluster, the Ingress controller will be publicly exposed and route the traffic to your application as we will see in a moment.

You can verify now that the webapp service is available.
```bash
$ kubectl get svc --namespace djaks
```

## Step 9: Set up the Ingress routing

### Define the Ingress rules
Let’s have a look at the Ingress routing file.

```bash
# k8s/ingress-routing.yaml

apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ingress-resource-rules
  annotations:
    kubernetes.io/ingress.class: nginx
    # cert-manager.io/cluster-issuer: letsencrypt-staging
    cert-manager.io/cluster-issuer: letsencrypt-prod
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  tls:
  - hosts:
    - django-aks-ingress.westeurope.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: django-aks-ingress.westeurope.cloudapp.azure.com
    http:
      paths:
      - backend:
          serviceName: webapp
          servicePort: 80
        path: /(.*)
```

Apply the Ingress to your cluster.
```bash
$ kubectl apply -f ingress-routing.yaml --namespace djaks
```

### Configure the Django allowed host

Go to the ```settings.py``` file and add your FQDN domain to the ```ALLOWED_HOSTS``` list.

```bash
# app/app/settings.py

...

ALLOWED_HOSTS = ['django-aks-ingress.westeurope.cloudapp.azure.com']

...
```

Now re-build the Docker container image of your new application version locally.
```bash
$ docker build -f Dockerfile -t django-aks:v1.0.1 .
```

Tag the image for your remote ACR registry.
```bash
$ docker tag django-aks:v1.0.1 djangoaksregistrybb.azurecr.io/django-aks:v1.0.1
```

Push the image to the ACR registry.
```bash
$ docker push djangoaksregistrybb.azurecr.io/django-aks:v1.0.1
```

If you get an authentication error, login to the ACR registry and try again.
```bash
$ az acr login --name djangoaksregistrybb
```

Now we have to let the cluster know that we want to update our django-web-app Deployment.
```bash
$ kubectl set image deployment django-web-app django-web-container=djangoaksregistrybb.azurecr.io/django-aks:v1.0.1 --namespace djaks
```

## Step 10: Clean up resources
If you want to get rid of the created resources, you can clean them up. I will present two clean up options!

### Keep the Azure resources, wipe the cluster namespace
When you want to keep the Azure resources, then you are good to go by deleting the namespace in which we created the cluster resources.

```bash
$ kubectl delete namespace djaks
```

### Delete all resources
When you want to get back to a clean slate and make sure the paid Azure resources are removed as well, delete the Azure resource group we created in the beginning.

```bash
$ az group delete --name djangoAKS
```

That’s it, we are done!