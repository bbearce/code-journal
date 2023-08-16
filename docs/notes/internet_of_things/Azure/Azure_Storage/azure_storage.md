# Overview of Azure Storage

## Blob Container
### Code
```bash
# https://learn.microsoft.com/en-us/azure/storage/blobs/blobfuse2-how-to-deploy?tabs=Ubuntu

# How to install BlobFuse2
## Option 1: Install BlobFuse2 from the Microsoft software repositories for Linux
### To check your version of Linux, run the following command:
cat /etc/*-release

### Install libfuse-dev
sudo wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install libfuse-dev # ubuntu 18.04
# sudo apt-get install libfuse3-dev fuse3 # didn't work; this is for ubuntu > 18.04

### Install BlobFuse2
sudo apt-get install blobfuse2


# How to configure BlobFuse2
## Create Blobfuse Config File(s)
CONFIG=/home/azureuser/blobfuse-config.yaml
touch $CONFIG
vim $CONFIG

## Create an empty directory to mount the blob container
CONTAINER_DIR=/mnt/dlsparseviewctchallenge/bundles
sudo mkdir -p $CONTAINER_DIR
# echo $CONTAINER_DIR

sudo blobfuse2 mount $CONTAINER_DIR --config-file=$CONFIG

ls $CONTAINER_DIR
```

### Config
[Sample](https://github.com/Azure/azure-storage-fuse/blob/main/sampleStreamingConfig.yaml)
```bash
# Refer ./setup/baseConfig.yaml for full set of config parameters

allow-other: true

logging:
  type: syslog
  level: log_debug

components:
  - libfuse
  - stream
  - attr_cache
  - azstorage

libfuse:
  attribute-expiration-sec: 120
  entry-expiration-sec: 120
  negative-entry-expiration-sec: 240

stream:
  block-size-mb: 8
  blocks-per-file: 3
  cache-size-mb: 1024

attr_cache:
  timeout-sec: 7200

azstorage:
  type: block
  account-name: mystorageaccount
  account-key: mystoragekey
  endpoint: https://mystorageaccount-CHANGEME.blob.core.windows.net
  mode: key
  container: mycontainer
 
```




## File Share
```bash
sudo apt update -y;
sudo apt remove azure-cli -y && sudo apt autoremove -y
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
# Microsoft Account
az login --use-device-code

RESOURCE_GROUP_NAME=""
STORAGE_ACCOUNT_NAME=""

# This command assumes you have logged in with az login
HTTP_ENDPOINT=$(az storage account show \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $STORAGE_ACCOUNT_NAME \
    --query "primaryEndpoints.file" --output tsv | tr -d '"')
SMBPATH=$(echo $HTTP_ENDPOINT | cut -c7-${#HTTP_ENDPOINT})
FILE_HOST=$(echo $SMBPATH | tr -d "/")

# Test
nc -zvw3 $FILE_HOST 445


FILE_SHARE_NAME=""

MNT_ROOT="/mnt"
MNT_PATH="$MNT_ROOT/$STORAGE_ACCOUNT_NAME/$FILE_SHARE_NAME"

sudo mkdir -p $MNT_PATH

# This command assumes you have logged in with az login
HTTP_ENDPOINT=$(az storage account show \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $STORAGE_ACCOUNT_NAME \
    --query "primaryEndpoints.file" --output tsv | tr -d '"')
SMB_PATH=$(echo $HTTP_ENDPOINT | cut -c7-${#HTTP_ENDPOINT})$FILE_SHARE_NAME

STORAGE_ACCOUNT_KEY=$(az storage account keys list \
    --resource-group $RESOURCE_GROUP_NAME \
    --account-name $STORAGE_ACCOUNT_NAME \
    --query "[0].value" --output tsv | tr -d '"')

sudo mount -t cifs $SMB_PATH $MNT_PATH -o username=$STORAGE_ACCOUNT_NAME,password=$STORAGE_ACCOUNT_KEY,uid=$(id -u),gid=$(id -g),serverino,nosharesock,actimeo=30,mfsymlinks

ls $MNT_PATH
```