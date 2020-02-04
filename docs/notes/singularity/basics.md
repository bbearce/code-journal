# Singularity

> Source [Sylabs Inc.](https://www.youtube.com/watch?v=SJHizTjwyFk)

## Build from Recipe

### The Recipe File

Bootstrap let's us know that we are using a container from dockerhub to build off of. This should be similar to Dockerfile's from docker. Most of this should be self explainatory.
```bash
Bootstrap: docker
From: ubuntu:18.04

%help
    This is where you can add some useful info.

%labels
    Creator Ben

%environment
    export MY_VAR='~~~~~some environment variable~~~~~'

%files
    test.py   /

%post
    apt-get -qq -y update
    apt-get -qq -y install python > /dev/null

%runscript
    python /test.py


```

Sections in recipe file:
* help: Give some help 
* labels: Impart meta-data into container
* environment: Environment variables
* files: Copy files into container 
* post: Run commands once container is created
* runscript: A command to run by default 

### Build 

Use this command to build the recipe file into a ```*.simg```

```bash
sudo singularity build ubuntu.simg singularity.recipe
```

Here the "ubuntu.simg singularity.recipe" part of the command is "<image-name>.simg <recipe-file-name>.recipe"


### Help

```bash
bbearce@bryce:~/singularity$ singularity help ubuntu.simg


    This is where you can add some useful info.
```

### Inspect

Look at meta-data with ```inspect```. Notice the ```%labels``` sections details shows up under ```"CREATOR": "Ben"```

```bash
bbearce@bryce:~/singularity$ singularity inspect ubuntu.simg
{
    "org.label-schema.usage.singularity.deffile.bootstrap": "docker",
    "org.label-schema.usage.singularity.deffile": "singularity.recipe",
    "org.label-schema.usage": "/.singularity.d/runscript.help",
    "org.label-schema.schema-version": "1.0",
    "CREATOR": "Ben",
    "org.label-schema.usage.singularity.deffile.from": "ubuntu:18.04",
    "org.label-schema.build-date": "Tue,_04_Feb_2020_15:36:00_-0500",
    "org.label-schema.usage.singularity.runscript.help": "/.singularity.d/runscript.help",
    "org.label-schema.usage.singularity.version": "2.5.2-dist",
    "org.label-schema.build-size": "135MB"
}
```

### Shell

Get inside container with ```shell```. I got confused when I tried this because my machine didn't give the same results as the tutorial. Let's proceed.

```bash
bbearce@bryce:~/singularity$ singularity shell ubuntu.simg 
Singularity: Invoking an interactive shell within container...

bash: warning: setlocale: LC_ALL: cannot change locale (en_US.UTF-8)
Singularity ubuntu.simg:~/singularity>
```

Looks good right? Let's try an ```ls``` to see what's inside.

```bash
Singularity ubuntu.simg:~/singularity> ls
Singularity.recipe  test.py  ubuntu.simg
Singularity ubuntu.simg:~/singularity>
```

Hmm... I don't see the *test.py* file I was expecting. I also see my newly created image too. However the prompt is different and I can access my environment variable:

```bash
Singularity ubuntu.simg:~/singularity> echo $MY_VAR
~~~~~some environment variable~~~~~
```

So what is going on? Turns out by default the users *home* directory is auto mounted to the same location in the container. If you change directories to */* you can see we are not on the computer anymore. We can see *test.py* 4th from the bottom!

> /home and /root are both bound by default

```bash
Singularity ubuntu.simg:/> ls -l 
total 1737
drwxr-xr-x   2 root root    1146 Jan 12 16:10 bin
drwxr-xr-x   2 root root       3 Apr 24  2018 boot
drwxr-xr-x  17 root root    4640 Jan 24 11:16 dev
lrwxrwxrwx   1 root root      36 Jan 28  2019 environment -> .singularity.d/env/90-environment.sh
drwxr-xr-x   1 root root      60 Feb  4 17:14 etc
drwxr-xr-x   1 root root      60 Feb  4 17:14 home
drwxr-xr-x   8 root root     117 May 23  2017 lib
drwxr-xr-x   2 root root      43 Jan 12 16:10 lib64
drwxr-xr-x   2 root root       3 Jan 12 16:09 media
drwxr-xr-x   2 root root       3 Jan 12 16:09 mnt
drwxr-xr-x   2 root root       3 Jan 12 16:09 opt
dr-xr-xr-x 825 root root       0 Jan 24 11:02 proc
drwx------   2 root root      46 Feb  4 17:00 root
drwxr-xr-x   5 root root      67 Feb  4 17:00 run
drwxr-xr-x   2 root root    1123 Feb  4 17:00 sbin
lrwxrwxrwx   1 root root      24 Jan 28  2019 singularity -> .singularity.d/runscript
drwxr-xr-x   2 root root       3 Jan 12 16:09 srv
dr-xr-xr-x  13 root root       0 Jan 24 15:49 sys
-rw-rw-r--   1 root root      70 Feb  4 17:00 test.py
drwxrwxrwt  22 root root 1773568 Feb  4 17:17 tmp
drwxr-xr-x  10 root root     150 Jan 12 16:09 usr
drwxr-xr-x  11 root root     160 Jan 12 16:10 var
```


### Sandbox

We can build a container without a recipe file (interactively).

> You should always record what you are doing in a recipe file so that you can remember the steps needed to create the file. We will write in *Singularity.recipe*.

## Build from ubuntu again but this time directly from bash command (not recipe file).

```bash
bbearce@bryce:~/singularity$ sudo singularity build --sandbox ubuntu_s docker://ubuntu
Docker image path: index.docker.io/library/ubuntu:latest
Cache folder set to /root/.singularity/docker
Importing: base Singularity environment
Exploding layer: sha256:5c939e3a4d1097af8d3292ad3a41d3caa846f6333b91f2dd22b972bc2d19c5b5.tar.gz
Exploding layer: sha256:c63719cdbe7ae254b453dba06fb446f583b503f2a2c15becc83f8c5bc7a705e0.tar.gz
Exploding layer: sha256:19a861ea6baff71b05cd577478984c3e62cf0177bf74468d0aca551f5fcb891c.tar.gz
Exploding layer: sha256:651c9d2d6c4f37c56a221259e033e7e2353b698139c2ff950623ca28d64a9837.tar.gz
Exploding layer: sha256:c6a9ef4b9995d615851d7786fbc2fe72f72321bee1a87d66919b881a0336525a.tar.gz
Singularity container built: ubuntu_s
Cleaning up...
bbearce@bryce:~/singularity$ ls
Singularity.recipe  test.py  ubuntu_s
```

So far the recipe file has this:

"Singularity.recipe"
```bash
Bootstrap: docker
From: ubuntu:18.04
```

Now let's try to run our *test.py* file in the container:

```bash
bbearce@bryce:~/singularity$ singularity exec ubuntu_s python test.py
/.singularity.d/actions/exec: 9: exec: python: not found
```

We need to add python to the container... let's do thatin the sandbox.

> Since singularity doesn't allow root permissions, you will have to use sudo to do this. Setting up singularity containers will often require root permissions, but to run them you will not

```bash
sudo singularity shell --writable ubuntu_s
```

> We need --writable to tell singularity that when we exit the container we want our changes to stay...otherwise they will be deleted.

```bash
Singularity ubuntu_s:~> apt-get -y install python
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package python
```

Ugh...we don't have python. So let's update the package list.

```bash
Singularity ubuntu_s:~> apt-get -y update        
Get:1 http://archive.ubuntu.com/ubuntu bionic InRelease [242 kB]
Get:2 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
Get:3 http://archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Get:4 http://security.ubuntu.com/ubuntu bionic-security/restricted amd64 Packages [26.7 kB]
...
...
...
```

Now let's install python

```bash
Singularity ubuntu_s:~> apt-get -y install python
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
...
...
...
```

Can we now run our file?

```bash
bbearce@bryce:~/singularity$ singularity exec ubuntu_s python test.py
the singularity has happened!
```

> Remember we haven't added test.py to the container so the only reason we can do what we just did was because */home/<user>/singularity* was auto mapped inside the container, and that's where *test.py* is.

Now let's record this in our recipe file.

"Singularity.recipe"
```bash
Bootstrap: docker
From: ubuntu:18.04

%post
    apt-get -y update
    apt-get -y install python

%files
    test.py  /

%runscript
    python /test.py

```

The final step is to build another container named *ubuntu.simg* from the recipe file to show that we can now do it from that.

```bash
bbearce@bryce:~/singularity$ sudo singularity build ubuntu.simg Singularity.recipe 
Using container recipe deffile: Singularity.recipe
Sanitizing environment
Adding base Singularity environment to container
Docker image path: index.docker.io/library/ubuntu:18.04
Cache folder set to /root/.singularity/docker
Exploding layer: sha256:5c939e3a4d1097af8d3292ad3a41d3caa846f6333b91f2dd22b972bc2d19c5b5.tar.gz
Exploding layer: sha256:c63719cdbe7ae254b453dba06fb446f583b503f2a2c15becc83f8c5bc7a705e0.tar.gz
Exploding layer: sha256:19a861ea6baff71b05cd577478984c3e62cf0177bf74468d0aca551f5fcb891c.tar.gz
Exploding layer: sha256:651c9d2d6c4f37c56a221259e033e7e2353b698139c2ff950623ca28d64a9837.tar.gz
Exploding layer: sha256:c6a9ef4b9995d615851d7786fbc2fe72f72321bee1a87d66919b881a0336525a.tar.gz
User defined %runscript found! Taking priority.
Adding files to container
Copying 'test.py' to '/'
Running post scriptlet
+ apt-get -y update
...
...
...
Reading package lists... Done
+ apt-get -y install python
...
...
...
Processing triggers for libc-bin (2.27-3ubuntu1) ...
Adding runscript
Finalizing Singularity container
Calculating final size for metadata...
Skipping checks
Building Singularity image...
Singularity container built: ubuntu.simg
Cleaning up...
```

Now we should be able to run the container directly:

```bash
bbearce@bryce:~/singularity$ ls
Singularity.recipe  test.py  ubuntu_s  ubuntu.simg

bbearce@bryce:~/singularity$ singularity run ubuntu.simg 
the singularity has happened!
```