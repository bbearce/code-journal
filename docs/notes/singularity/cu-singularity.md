# Apptainer

## Build
> Singularity is now ```apptainer```. 

```bash
apptainer build ubuntu_latest.sif docker://ubuntu:latest
```

```bash
apptainer build pytorch_w_cuda.sif pytorch_based.recipe
```

### Recipe File
"pytorch_based.recipe"
```bash
Bootstrap: docker
From: pytorch/pytorch:latest

%help
    This is where you can add some useful info.

%labels
    Creator Ben

%environment
    export MY_VAR='~~~~~some environment variable~~~~~'

%files
    test.py /
    requirements.txt /

%post
    apt-get -qq -y update
    pip install -r requirements.txt
    

%runscript
    python /test.py
```



## Shell
```bash
apptainer shell <image name>
```

> Notice we can write/see to ```~/singularity``` and not ```/mnt```.
```bash
bearceb@ophlapps03:~/singularity$ ls
pytorch_based.recipe  pytorch_based.sif  README.md  requirements.txt  test.py  ubuntu_latest.sif

bearceb@ophlapps03:~/singularity$ apptainer shell ubuntu_latest.sif
Apptainer> ls
README.md  pytorch_based.recipe  pytorch_based.sif  requirements.txt  test.py  ubuntu_latest.sif

Apptainer> pwd
/home/bearceb/singularity

Apptainer> touch test.txt

Apptainer> touch /mnt/test.txt
touch: cannot touch '/mnt/test.txt': Read-only file system
```
Only mounted directories have write permissions (```$PWD``` is one automatically mounted). Shelling provides read-only access to images.

* By default most user-owned files and directories are available to any container that is run on Alpine and Blanca

* This includes files in:  
  - /home/$USER  
  - /projects/$USER  
  - /scratch/summit/$USER  
  - /rc_scratch/$USER)  


## Sandbox
```bash
apptainer build --sandbox <sandbox> <*.sif>
```

```bash
bbearce@pop-os:~/Desktop/singularity$ apptainer build --sandbox s_ubuntu ubuntu_latest.sif
INFO:    Starting build...
INFO:    Verifying bootstrap image ubuntu_latest.sif
INFO:    Creating sandbox directory...
INFO:    Build complete: s_ubuntu
bbearce@pop-os:~/Desktop/singularity$ ls
pytorch_based.recipe  s_ubuntu  ubuntu_latest.sif
README.md             test.py
requirements.txt      test.txt
```

You need the ```--writable``` flag to write in the sandbox. Natively it too is read-only.
```bash
bbearce@pop-os:~/Desktop/singularity$ apptainer shell s_ubuntu 
Apptainer> touch /mnt/test.txt
touch: cannot touch '/mnt/test.txt': Read-only file system
Apptainer> 
exit
bbearce@pop-os:~/Desktop/singularity$ apptainer shell --writable s_ubuntu 
WARNING: Skipping mount /etc/localtime [binds]: /etc/localtime doesn't exist in container
Apptainer> touch /mnt/test.txt
Apptainer> pip install <package>
```

To build back into an image:
```bash
bbearce@pop-os:~/Desktop/singularity$ apptainer build s_ubuntu_modified s_ubuntu
INFO:    Starting build...
INFO:    Creating SIF file...
INFO:    Build complete: s_ubuntu_modified
```


## Run
> Work on this
```bash
singularity run ubuntu_latest.sif
```

## Alpine
Picking up where Scott left off we want to run on Alpine.
> login: https://ondemand-rmacc.rc.colorado.edu/ 

```bash
[bbearce@xsede.org@login-ci1 ~]$ 
```

```bash
[bbearce@xsede.org@login-ci1 ~]$ curc-quota
------------------------------------------------------------------------
                                       Used         Avail    Quota Limit
------------------------------------------------------------------------
/home/bbearce@xsede.org                   0          2.0G           2.0G
/projects/bbearce@xsede.org               0          250G           250G
/scratch/alpine1                         0G         9537G          9537G

[bbearce@xsede.org@login-ci1 ~]$ cd /projects/bbearce@xsede.org
[bbearce@xsede.org@login-ci1 bbearce@xsede.org]$ ls
ondemand  README.mdwn
```

```bash
acompile # start session
```

### Set important environment variables
This is to now fill up your quota:
```bash
module load singularity/3.6.4
export TMP=/scratch/alpine/$USER
export TEMP=/scratch/alpine/$USER
export TMPDIR=/scratch/alpine/$USER
export TEMPDIR=/scratch/alpine/$USER
export ALPINE_SCRATCH=/gpfs/alpine1/scratch/$USER
export APPTAINER_TMPDIR=$ALPINE_SCRATCH/singularity/tmp
export APPTAINER_CACHEDIR=$ALPINE_SCRATCH/singularity/cache
mkdir -pv $APPTAINER_CACHEDIR $APPTAINER_TMPDIR
```

### Create From Recipe
```bash
[bbearce@xsede.org@c3cpu-a5-u34-3 singularity]$ cd /projects/bbearce@xsede.org/singularity

[bbearce@xsede.org@c3cpu-c15-u11-4 singularity]$ apptainer build pytorch_w_cuda.sif pytorch_based.recipe
INFO:    Environment variable SINGULARITY_TMPDIR is set, but APPTAINER_TMPDIR is preferred
INFO:    User not listed in /etc/subuid, trying root-mapped namespace
INFO:    Environment variable SINGULARITY_TMPDIR is set, but APPTAINER_TMPDIR is preferred
INFO:    The %post section will be run under fakeroot
INFO:    Environment variable SINGULARITY_CACHEDIR is set, but APPTAINER_CACHEDIR is preferred
INFO:    Starting build...
Getting image source signatures
Copying blob 99803d4b97f3 done  
Copying blob 4f4fb700ef54 done  
Copying blob 4ade0a4bc5d5 done  
Copying blob 2185b402c9ca done  
Copying blob 035a286326d6 [=========>----------------------------] 782.8MiB / 2.9GiB
```


## Sylabs Container Registry
List Remotes:
```bash
apptainer remote list
Cloud Services Endpoints
========================

NAME           URI                  ACTIVE  GLOBAL  EXCLUSIVE  INSECURE
DefaultRemote  cloud.apptainer.org  YES     YES     NO         NO

Keyservers
==========

URI                       GLOBAL  INSECURE  ORDER
https://keys.openpgp.org  YES     NO        1*

* Active cloud services keyserver
```

Sylabs was the default but we have to add retroactvely.
```bash
apptainer remote add --no-login SylabsCloud cloud.sycloud.io
```

Switch remote:
```bash
apptainer remote use SylabsCloud
```

Login:
```bash
apptainer remote login
Generate an access token at https://cloud.sycloud.io/auth/tokens, and paste it here.
Token entered will be hidden for security.
Access Token: 
INFO:    Access Token Verified!
INFO:    Token stored in /home/bearceb/.apptainer/remote.yaml
```

Push (```-U``` means unsigned. [Sign](https://cloud.sylabs.io/keystore) for more security):
```bash
apptainer push -U ubuntu_latest.sif library://bbearce/ubuntu/dockerhub:pushed
WARNING: Skipping container verification
28.4MiB / 28.4MiB [================================================================] 100 % 6.0 MiB/s 0s
```

Pull:
```bash
apptainer pull library://bbearce/ubuntu/dockerhub:latest

bearceb@ophlapps03:~/singularity$ ls
dockerhub_latest.sif
```


## Remote Builder (Haven't got working )
[Remote Builder](https://curc.readthedocs.io/en/latest/software/Containerizationon.html?highlight=singularity#singularity)

Build images remotely.

### Make a syslabs token
```bash
cd ~
mkdir .singularity; cd .singularity
echo “<your-token>” > ~/.singularity/sylabs-token
singularity build --remote <desired-image-name> <your-recipe>
```

```bash
singularity build --remote pytorch.sif pytorch_based.recipe
```



