Install:

[az cli install](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
az login
```

Copy snapshot from one region to another:
```bash
SUBSCRIPTION_ID=a7ab1d40-7176-42a9-9156-0eb7fbe1ae3e
```

```bash
az disk create \
    --resource-group CHALLENGES \
    --name qtim-challenges-os-disk-backup-disk-new \
    --source qtim-challenges-os-disk-backup-eastus \
    --zone 1 \
    --location eastus

```

```bash
az disk create \
    --resource-group CHALLENGES \
    --name qtim-challenges-disk-eastus \
    --source /subscriptions/$SUBSCRIPTION_ID/resourceGroups/CHALLENGES/providers/Microsoft.Compute/disks/qtim-challenges_OsDisk_1_2739395aad8147d1954d583529de7dab \
    --location eastus


```

```bash
az snapshot show \
    --resource-group CHALLENGES \
    --name qtim-challenges-os-disk-backup-eastus

```

```bash
az snapshot create \
    --resource-group CHALLENGES \
    --source /subscriptions/$SUBSCRIPTION_ID/resourceGroups/CHALLENGES/providers/Microsoft.Compute/disks/qtim-challenges_OsDisk_1_2739395aad8147d1954d583529de7dab \
    --name qtim-challenges-os-disk-backup-eastus \
    --location eastus

```


```bash
az vm disk attach \
    --resource-group CHALLENGES \
    --vm-name qtim-challenges-new \
    --name qtim-challenges-os-disk-backup-disk
```

```bash
az snapshot create \
    --resource-group CHALLENGES \
    --source qtim-challenges-os-disk-backup-disk \
    --name qtim-challenges-os-disk-backup-disk-snapshot

```