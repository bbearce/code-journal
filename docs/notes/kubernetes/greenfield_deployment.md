# Greenfield Deployment

> If I've learned one thing it is that just because this works (if it does) it will not necessarily work in a month. Probably best to go to this link (below) and do the tutorial fresh from the docs each time... :(

> This is the official (take with grain of salt the size of hail) Azure tutorial including many extra things from more simple tutorials. Azure seems to have the most mature and complex kubernetes setup.

[Source](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/master/docs/setup/install-new.md)

The instructions below assume Application Gateway Ingress Controller (AGIC) will be installed in an environment with no pre-existing components.

## Create an identity

1. Create AD Service Principal:
```bash
az ad sp create-for-rbac --skip-assignment -o json > auth.json
appId=$(jq -r ".appId" auth.json)
password=$(jq -r ".password" auth.json)
```

which makes this auth.json:
```bash
{
  "appId": "3c3a801c-5ad1-468b-b7e7-787363800352",
  "displayName": "azure-cli-2020-10-23-23-08-43",
  "name": "http://azure-cli-2020-10-23-23-08-43",
  "password": "FNhe_Ln_nCiIiSRVv_lAm7bD-miRxhNpVD",
  "tenant": "e72101ce-ef5d-49e3-baec-01191775dcc7"
}
```

2. Get object ID:

This is the new Service Principal id.
```bash
objectId=$(az ad sp show --id $appId --query "objectId" -o tsv)
```

```bash
$ echo $objectId
dd3b3bcb-9b0c-4a64-8316-0f0150d39e0d
```

3. Create ```parameters.json``` for use in the ARM (Azure Resource Manager) template.

```bash
cat <<EOF > parameters.json
{
  "aksServicePrincipalAppId": { "value": "$appId" },
  "aksServicePrincipalClientSecret": { "value": "$password" },
  "aksServicePrincipalObjectId": { "value": "$objectId" },
  "aksEnableRBAC": { "value": false }
}
EOF
```
mine became:
```bash
{
  "aksServicePrincipalAppId": { "value": "3c3a801c-5ad1-468b-b7e7-787363800352" },
  "aksServicePrincipalClientSecret": { "value": "FNhe_Ln_nCiIiSRVv_lAm7bD-miRxhNpVD" },
  "aksServicePrincipalObjectId": { "value": "dd3b3bcb-9b0c-4a64-8316-0f0150d39e0d" },
  "aksEnableRBAC": { "value": false }
}
```

To deploy an RBAC (Role Based Authentication Control) Id cluster, set the ```aksEnabledRBAC``` field to ```true```.

## Deploy Components

The next few steps will add the following list of components to your Azure subscription:

* [Azure Kubernetes Service](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes)  
* [Application Gateway](https://docs.microsoft.com/en-us/azure/application-gateway/overview) v2  
* [Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) with 2 [subnets](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview)  
* [Public IP Address](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-public-ip-address)  
* [Managed Identity](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview), which will be used by [AAD Pod Identity](https://github.com/Azure/aad-pod-identity/blob/master/README.md)  

1. Download ARM template into template.json:

```bash
wget https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/deploy/azuredeploy.json -O template.json
```

> Note: The template had version 1.15 for kubernetes. I changed to 1.19 after running ```az aks get-versions --location westus```

2. Deploy the ARM template with ```az``` and use flags and their values to set certain values:

```bash
resourceGroupName="flask_demo"
location="eastus"
deploymentName="ingress-appgw"
az group create -n $resourceGroupName -l $location
az group deployment create -g $resourceGroupName -n $deploymentName --template-file template.json --parameters parameters.json
```

