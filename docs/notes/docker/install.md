# Install

Docker has good documentation [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/).


## Uninstall old versions

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

## Install using the repository

### [1] Update the apt package index and install packages to allow apt to use a repository over HTTPS:

```bash
sudo apt-get update
```
and
```bash
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

### [2] Add Docker’s official GPG key:

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 ```854A  E2D8 8D81 803C 0EBF CD88```, by searching for the last 8 characters of the fingerprint.

```bash
$ sudo apt-key fingerprint 0EBFCD88
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

### [3] Use the following command to set up the *stable* repository.

```bash
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

## Install Docker Engine

[1] Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

[2] Verify that Docker Engine is installed correctly by running the hello-world image.

```bash
$ sudo docker run hello-world
```

[3] If you would like to use Docker as a non-root user, you should now consider adding your user to the “docker” group with something like:

```bash
$ sudo usermod -aG docker <your-user>
```

