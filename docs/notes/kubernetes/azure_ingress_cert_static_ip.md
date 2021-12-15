# Azure Ingress\Cert for Static IP

## Create an ingress controller

```bash
myResourceGroup="flask_demo"
myAKSCluster="flaskAKSCluster"
az aks show --resource-group $myResourceGroup --name $myAKSCluster --query nodeResourceGroup -o tsv
```
Node Resource Group: *MC_flask_demo_flaskAKSCluster_eastus*

Next, create a public IP address with the static allocation method using the [az network public-ip create command](https://docs.microsoft.com/en-us/cli/azure/network/public-ip?view=azure-cli-latest#az_network_public_ip_create).

```bash
az network public-ip create --name
                            --resource-group
                            [--allocation-method {Dynamic, Static}]
                            [--dns-name]
                            [--idle-timeout]
                            [--ip-tags]
                            [--location]
                            [--public-ip-prefix]
                            [--reverse-fqdn]
                            [--sku {Basic, Standard}]
                            [--subscription]
                            [--tags]
                            [--version {IPv4, IPv6}]
                            [--zone {1, 2, 3}]
```

The following example creates a public IP address named myAKSPublicIP in the AKS cluster resource group obtained in the previous step:

```bash
az network public-ip create --resource-group MC_flask_demo_flaskAKSCluster_eastus --name myAKSPublicIP --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv
```

Public IP: *20.62.186.98*

> The above commands create an IP address that will be deleted if you delete your AKS cluster.

Now deploy the nginx-ingress chart with Helm.

You must pass two additional parameters to the Helm release so the ingress controller is made aware both of the static IP address of the load balancer to be allocated to the ingress controller service, and of the DNS name label being applied to the public IP address resource. For the HTTPS certificates to work correctly, a DNS name label is used to configure an FQDN for the ingress controller IP address.

1. Add the ```--set controller.service.loadBalancerIP``` parameter. Specify your own public IP address that was created in the previous step.  

2. Add the ```--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"``` parameter. Specify a DNS name label to be applied to the public IP address that was created in the previous step.  


> Tip: The following example creates a Kubernetes namespace for the ingress resources named ```ingress-basic```. Specify a namespace for your own environment as needed. If your AKS cluster is not RBAC enabled, add ```--set rbac.create=false``` to the Helm commands.

```bash
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Add the ingress-nginx repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# Use Helm to deploy an NGINX ingress controller
STATIC_IP="20.62.186.98"
helm install nginx-ingress ingress-nginx/ingress-nginx \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set controller.service.loadBalancerIP=$STATIC_IP \
    --set rbac.create=false \
    --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"="anything-will-do"
```

No ingress rules have been created yet, so the NGINX ingress controller's default 404 page is displayed if you browse to the public IP address. Ingress rules are configured in the following steps.

```bash
az network public-ip list --resource-group mc_flask_demo_flaskakscluster_eastus --query "[?name=='myAKSPublicIP'].[dnsSettings.fqdn]" -o tsv
```

*anything-will-do.eastus.cloudapp.azure.com*

## Install cert-manager


```bash
# Label the cert-manager namespace to disable resource validation
kubectl label namespace ingress-basic cert-manager.io/disable-validation=true

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install \
  cert-manager \
  --namespace "ingress-basic" \
  --version "v0.16.1" \
  --set installCRDs=true \
  --set rbac.create=false \
  --set nodeSelector."beta\.kubernetes\.io/os"=linux \
  jetstack/cert-manager
```

