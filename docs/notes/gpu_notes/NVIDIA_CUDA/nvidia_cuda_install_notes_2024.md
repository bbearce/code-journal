
# Install cuda drivers (cuda-toolkit)
[cuda-toolkit](https://developer.nvidia.com/cuda-downloads)

```bash
wget https://developer.download.nvidia.com/compute/cuda/12.4.1/local_installers/cuda_12.4.1_550.54.15_linux.run

sudo apt-get update

sudo sh cuda_12.4.1_550.54.15_linux.run
```

# install nvidia-docker-toolkit
[nvidia-docker-toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

```bash
sudo apt-get install -y nvidia-container-toolkit

curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg   && 

curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list |     sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' |     sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

# nvidia sample image
[nvidia docker images](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/cuda/tags)

```bash
IMAGE=python:3.9
IMAGE=nvcr.io/nvidia/cuda:12.6.0-cudnn-devel-ubuntu24.04

# works
docker run \
  -it \
  --rm \
  --gpus all \
  --runtime=nvidia \
  --privileged \
  -v /var/run/docker.sock:/var/run/docker.sock \
  $IMAGE nvidia-smi

# doesn't work
docker run \
  -it \
  --rm \
  --gpus all \
  --runtime=nvidia \
  -v /var/run/docker.sock:/var/run/docker.sock \
  $IMAGE nvidia-smi
```

output:
```
azureuser@gpu-worker:~$ IMAGE=python:3.9
azureuser@gpu-worker:~$ 
# works
docker run \
  -it \
  --rm \
  --gpus all \
  --runtime=nvidia \
  --privileged \
  -v /var/run/docker.sock:/var/run/docker.sock \
  $IMAGE nvidia-smi
Unable to find image 'python:3.9' locally
3.9: Pulling from library/python
903681d87777: Already exists 
3cbbe86a28c2: Already exists 
6ed93aa58a52: Already exists 
787c78da4383: Already exists 
74e6988984f8: Already exists 
8f13d3184846: Already exists 
a28b5664f47e: Already exists 
777e0be36342: Already exists 
Digest: sha256:8e0fd8e1bd3caa6ea74d1c408ad5a854c6b8f0e32c2ef9886d27d4d1bb9caeac
Status: Downloaded newer image for python:3.9
Fri Aug 23 15:22:19 2024       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.54.15              Driver Version: 550.54.15      CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  Tesla V100-PCIE-16GB           Off |   00000001:00:00.0 Off |                    0 |
| N/A   34C    P0             38W /  250W |       0MiB /  16384MiB |      1%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                        
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+

azureuser@gpu-worker:~$ IMAGE=nvcr.io/nvidia/cuda:12.6.0-cudnn-devel-ubuntu24.04
azureuser@gpu-worker:~$ 
# works
docker run \
  -it \
  --rm \
  --gpus all \
  --runtime=nvidia \
  --privileged \
  -v /var/run/docker.sock:/var/run/docker.sock \
  $IMAGE nvidia-smi

==========
== CUDA ==
==========

CUDA Version 12.6.0

Container image Copyright (c) 2016-2023, NVIDIA CORPORATION & AFFILIATES. All rights reserved.

This container image and its contents are governed by the NVIDIA Deep Learning Container License.
By pulling and using the container, you accept the terms and conditions of this license:
https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license

A copy of this license is made available in this container at /NGC-DL-CONTAINER-LICENSE for your convenience.

Fri Aug 23 15:23:46 2024       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.54.15              Driver Version: 550.54.15      CUDA Version: 12.6     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  Tesla V100-PCIE-16GB           Off |   00000001:00:00.0 Off |                    0 |
| N/A   35C    P0             38W /  250W |       0MiB /  16384MiB |      1%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                        
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+
azureuser@gpu-worker:~$

azureuser@gpu-worker:~$ 
# doesn't work
docker run \
  -it \
  --rm \
  --gpus all \
  --runtime=nvidia \
  -v /var/run/docker.sock:/var/run/docker.sock \
  $IMAGE nvidia-smi

==========
== CUDA ==
==========

CUDA Version 12.6.0

Container image Copyright (c) 2016-2023, NVIDIA CORPORATION & AFFILIATES. All rights reserved.

This container image and its contents are governed by the NVIDIA Deep Learning Container License.
By pulling and using the container, you accept the terms and conditions of this license:
https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license

A copy of this license is made available in this container at /NGC-DL-CONTAINER-LICENSE for your convenience.

Failed to initialize NVML: Unknown Error
azureuser@gpu-worker:~$ 
```
