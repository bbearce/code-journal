# Installing Singularity

> Source: [sylabs.io](https://sylabs.io/guides/3.6/admin-guide/installation.html)

## Terminal Install:

### Pre-requisites:

On Ubuntu or Debian install the following dependencies:

```bash
sudo apt-get update && sudo apt-get install -y \
    build-essential \
    uuid-dev \
    libgpgme-dev \
    squashfs-tools \
    libseccomp-dev \
    wget \
    pkg-config \
    git \
    cryptsetup-bin
```

Install Go:

```bash
export VERSION=1.5.12 OS=linux ARCH=amd64 && \
    wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz && \
    sudo tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz && \
    rm go$VERSION.$OS-$ARCH.tar.gz
```

```bash
echo 'export GOPATH=${HOME}/go' >> ~/.bashrc && \
    echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc && \
    source ~/.bashrc
```

### Install Singularity:

```bash
export VERSION=3.6.3 && # adjust this as necessary \
    wget https://github.com/sylabs/singularity/releases/download/v${VERSION}/singularity-${VERSION}.tar.gz && \
    tar -xzf singularity-${VERSION}.tar.gz && \
    cd singularity
```
then check out code from git:
```bash
git clone https://github.com/sylabs/singularity.git && \
    cd singularity && \
    git checkout v3.6.3
```

Now compile Singularity:
```bash
./mconfig && \
    make -C ./builddir && \
    sudo make -C ./builddir install
```