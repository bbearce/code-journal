# Use with docker

> Spoiler, while using docker is possible, I think it's going to be harder in the long run. I believe there is nothing wrong with starting on docker on bryce and in fact I recommend that. Once on Carlsbad or Dotter, I believe that just using singularity is easier (than using docker on these machines).

\#I_Love_Docker

## Use on Martinos machines:

### Setup simlinks:
> Source [Martinos Docs](https://www.nmr.mgh.harvard.edu/martinos/userInfo/computer/docker.php)
```bash
ls -la ~
...

lrwxrwxrwx   1 bb927 bb927    33 Sep 29 12:48 .share -> /space/dotter/2/users/bb927/share
lrwxrwxrwx   1 bb927 bb927    39 Sep 29 12:44 .singularity -> /space/dotter/2/users/bb927/singularity

...
```

Once you have this setup, you can proceed, otherwise you are in for a world of hurt if you accidentally fill up your real home dir.

### Martinos gotchas:

#### Dotter for example:

##### Pure Singularity:

Your home directory and drive directories are actually simlinks and this will mess up singularity on dotter.

```bash
dotter[0]:bb927$ ls -la /homes/3
lrwxrwxrwx 1 root root 17 Sep 17  2016 /homes/3 -> /autofs/homes/003
dotter[0]:bb927$ ls -la /space/dotter
total 32
drwxr-xr-x   2 root root  4096 Jul  9 12:00 .
drwx--x--x 984 root root 24576 Sep 19 18:12 ..
lrwxrwxrwx   1 root root    24 Jul  9 12:00 1 -> /autofs/space/dotter_001
lrwxrwxrwx   1 root root    24 Jul  9 12:00 2 -> /autofs/space/dotter_002
lrwxrwxrwx   1 root root    24 Jul  9 12:00 3 -> /autofs/space/dotter_003
lrwxrwxrwx   1 root root    24 Jul  9 12:00 4 -> /autofs/space/dotter_004
lrwxrwxrwx   1 root root    24 Jul  9 12:00 5 -> /autofs/space/dotter_005
lrwxrwxrwx   1 root root    24 Jul  9 12:00 6 -> /autofs/space/dotter_006

```

Remember Singularity wants to auto ```--bind``` mount these 3 directories:

* /home/$USER  
* /tmp  
* $PWD  

I have a sanbox made from a regular ubuntu image (docker://ubuntu:latest) that was first pulled to ```ubuntu.simg``` and then the sandbox is called ```s_ubuntu```. Let's try to shell into it.

This is my current $PWD:
```bash
dotter[0]:bb927$ pwd
/space/dotter/2/users/bb927
dotter[0]:bb927$ ls
in  out  README.md  share  singularity  s_ubuntu  ubuntu.simg
```

After shelling in let's look at the mounts:
```bash
dotter[0]:bb927$ singularity shell s_ubuntu
Singularity> ls ~   
Desktop    Downloads  Pictures	Templates  matlab

Documents  Music      Public	Videos	   matlab_crash_dump.224407-1
Singularity> ls /tmp
hsperfdata_bb927
hsperfdata_sra24
idmap.sh.log
krb5ccmachine_PARTNERS.ORG
systemd-private-9eb3a577ab90415d9ff57c178714ca99-bolt.service-eEU8AX
systemd-private-9eb3a577ab90415d9ff57c178714ca99-chronyd.service-YED8Bf
systemd-private-9eb3a577ab90415d9ff57c178714ca99-colord.service-yBgBOp
systemd-private-9eb3a577ab90415d9ff57c178714ca99-cups.service-eaN0xa
systemd-private-9eb3a577ab90415d9ff57c178714ca99-rtkit-daemon.service-AtHDqM

Singularity> pwd
/space/dotter/2/users/bb927
Singularity> exit
exit
```
Now outside the container:
```bash
dotter[0]:bb927$ ls ~
Desktop    Downloads  matlab_crash_dump.224407-1  Pictures  Templates
Documents  matlab     Music                       Public    Videos
dotter[0]:bb927$ ls /tmp
hsperfdata_bb927
hsperfdata_sra24
idmap.sh.log
krb5ccmachine_PARTNERS.ORG
systemd-private-9eb3a577ab90415d9ff57c178714ca99-bolt.service-eEU8AX
systemd-private-9eb3a577ab90415d9ff57c178714ca99-chronyd.service-YED8Bf
systemd-private-9eb3a577ab90415d9ff57c178714ca99-colord.service-yBgBOp
systemd-private-9eb3a577ab90415d9ff57c178714ca99-cups.service-eaN0xa
systemd-private-9eb3a577ab90415d9ff57c178714ca99-rtkit-daemon.service-AtHDqM
dotter[0]:bb927$ pwd
/space/dotter/2/users/bb927
dotter[0]:bb927$ 
```

We see those three locations mounted perfectly! :)

Now let's shell in with ```--writable```

```bash
dotter[0]:bb927$ singularity shell --writable s_ubuntu
WARNING: Your current working directory is a symlink and may not be available in container, you should use real path with --writable when possible
WARNING: Skipping mount /etc/localtime [binds]: /etc/localtime doesn't exist in container
WARNING: By using --writable, Singularity can't create /homes destination automatically without overlay or underlay
FATAL:   container creation failed: mount /var/singularity/mnt/session/homes->/homes error: while mounting /var/singularity/mnt/session/homes: destination /homes doesn't exist in container
dotter[0]:bb927$ 

```

Ugh, what happend? Well the WARNINGS were sort of ignored but the FATAL error is because /homes doesn't exist in the container. We can work around this by creating it before hand:

```bash
dotter[0]:bb927$ mkdir s_ubuntu/homes
```

```bash
dotter[0]:bb927$ singularity shell --writable s_ubuntu
WARNING: Your current working directory is a symlink and may not be available in container, you should use real path with --writable when possible
WARNING: Skipping mount /etc/localtime [binds]: /etc/localtime doesn't exist in container
WARNING: Skipping mount /space/dotter/2/users/bb927 [cwd]: /autofs/space/dotter_002/users/bb927 doesn't exist in container
Singularity> ls /homes
3
Singularity> ls /homes/3
bb927
Singularity> ls /homes/3/bb927/
Desktop    Downloads  Pictures	Templates  matlab
Documents  Music      Public	Videos	   matlab_crash_dump.224407-1
Singularity> 
```

Now even though the warning says we might not be able to write to our home directory, you can. This is probably because dotter has an environment variable HOME=/homes/3/bb927 and that is also set inside the singularity container. Even though that is a simlink you can access it inside the container.

But we know that Help Desk has asked us not to use our home directories because they have limited storage, so let's create the drive mount location and get rid of another warning:

```bash
dotter[0]:bb927$ mkdir -p s_ubuntu/autofs/space/dotter_002/users/bb927
dotter[0]:bb927$ singularity shell --writable s_ubuntu
WARNING: Your current working directory is a symlink and may not be available in container, you should use real path with --writable when possible
WARNING: Skipping mount /etc/localtime [binds]: /etc/localtime doesn't exist in container
Singularity> 

```

So we know we can ignore the first WARNING because there isn't much we can do, and as far as the second one is concerned, you have the tools to make it disappear if you need it to. 

**Mounting, does it work?!?!?**

In my $PWD I have ```in```  and ```out``` folders:
```bash
dotter[0]:bb927$ ls
in  out  README.md  share  singularity  s_ubuntu  ubuntu.simg
dotter[0]:bb927$ ls in
in.txt
dotter[0]:bb927$ ls out
out.txt

```

Let's see if we can read and write to them:

> Note: Just from playing around I've noticed in ```--writable``` mode you need to have created the mount points ahead of time. It seems that when running the container, you will not need to do this as the mounts will be made on the fly as shown below:

```bash
dotter[0]:bb927$ singularity shell --bind /autofs/space/dotter_002/users/bb927/in:/mnt/in --bind /autofs/space/dotter_002/users/bb927/out:/mnt/out s_ubuntu
Singularity> ls /mnt
in  out
Singularity> ls /mnt/in
in.txt
Singularity> ls /mnt/out
out.txt

```

watch in ```--writable``` mode:

```bash
dotter[0]:bb927$ singularity shell --writable --bind /autofs/space/dotter_002/users/bb927/in:/mnt/in --bind /autofs/space/dotter_002/users/bb927/out:/mnt/out s_ubuntu/
WARNING: Your current working directory is a symlink and may not be available in container, you should use real path with --writable when possible
WARNING: Skipping mount /etc/localtime [binds]: /etc/localtime doesn't exist in container
WARNING: By using --writable, Singularity can't create /mnt/in destination automatically without overlay or underlay
FATAL:   container creation failed: mount /autofs/space/dotter_002/users/bb927/in->/mnt/in error: while mounting /autofs/space/dotter_002/users/bb927/in: destination /mnt/in doesn't exist in container

```

So create them:

```bash
dotter[0]:bb927$ mkdir -p s_ubuntu/mnt/in
dotter[0]:bb927$ mkdir -p s_ubuntu/mnt/out
```

and try again:

```bash
dotter[0]:bb927$ singularity shell --writable --bind /autofs/space/dotter_002/users/bb927/in:/mnt/in --bind /autofs/space/dotter_002/users/bb927/out:/mnt/out s_ubuntu/
WARNING: Your current working directory is a symlink and may not be available in container, you should use real path with --writable when possible
WARNING: Skipping mount /etc/localtime [binds]: /etc/localtime doesn't exist in container
Singularity> ls /mnt
in  out
Singularity> ls /mnt/in
in.txt
Singularity> ls /mnt/out
out.txt
```

Nice! Ok, now the 1st of two important questions; can we read/write?

```bash
Singularity> cd /mnt/in
Singularity> touch new_file.txt
Singularity> ls
in.txt	new_file.txt  out.txt
Singularity> pwd
/mnt/in
Singularity> exit
exit
dotter[0]:bb927$ ls in
in.txt  new_file.txt  out.txt
dotter[0]:bb927$ ls -la in
total 16
drwxrwsr-x 2 bb927 pimi 4096 Oct  3 20:58 .
drwxrwsr-x 7 bb927 pimi 4096 Oct  3 19:55 ..
-rw-rw-r-- 1 bb927 pimi   10 Oct  3 20:56 in.txt
-rw-r--r-- 1 bb927 pimi    0 Oct  3 20:58 new_file.txt
-rw-rw-r-- 1 bb927 pimi   11 Oct  3 20:55 out.txt

```

Nice! Ok so now can we access GPUs?

Let's pull pytorch:
```bash
dotter[0]:bb927$ singularity pull docker://pytorch/pytorch:latest
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
Getting image source signatures
Copying blob 23884877105a done
Copying blob bc38caa0f5b9 done
Copying blob 2910811b6c42 done
Copying blob 36505266dcc6 done
Copying blob 3472d01858ba done
Copying blob 4a98b57681ff done
Copying blob f3b419d1e6d5 done
Copying config c35e69071c done
Writing manifest to image destination
Storing signatures
2020/10/03 21:02:21  info unpack layer: sha256:23884877105a7ff84a910895cd044061a4561385ff6c36480ee080b76ec0e771
2020/10/03 21:02:23  info unpack layer: sha256:bc38caa0f5b94141276220daaf428892096e4afd24b05668cd188311e00a635f
2020/10/03 21:02:23  info unpack layer: sha256:2910811b6c4227c2f42aaea9a3dd5f53b1d469f67e2cf7e601f631b119b61ff7
2020/10/03 21:02:23  info unpack layer: sha256:36505266dcc64eeb1010bd2112e6f73981e1a8246e4f6d4e287763b57f101b0b
2020/10/03 21:02:23  info unpack layer: sha256:3472d01858ba9ce89d2a493bb3964eaf26a63500a3b47ad0a4c72d5e5fe40e11
2020/10/03 21:02:23  info unpack layer: sha256:4a98b57681ffdd1fcba82e9aeb95171dbe0837980deb82464e4893f4449c4459
2020/10/03 21:03:08  info unpack layer: sha256:f3b419d1e6d54391719bc37b641b2c133915f207103d8dd28c776607e5d477d8
INFO:    Creating SIF file...
INFO:    Build complete: pytorch_latest.sif
dotter[0]:bb927$ 

```

Alright deep breath:

```bash
dotter[0]:bb927$ singularity shell pytorch_latest.sif 
Singularity> python
Python 3.7.7 (default, May  7 2020, 21:25:33) 
[GCC 7.3.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.cuda.is_available()
False
>>> 
```

NOOOOOooooooooooo...Ok there must be a flag:

```bash
dotter[0]:bb927$ singularity shell --nv pytorch_latest.sif 
Singularity> python
Python 3.7.7 (default, May  7 2020, 21:25:33) 
[GCC 7.3.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.cuda.is_available()
True
```

*Swish*

## Should we use Docker???

Actually I think this could be a problem. See as root inside docker if you write to mounts it shows up ownded by either root or 10000..:1000..., some weird user of numbers...Anyways you won't be able to manipulate that data once you log out of the container as you are not them! It seems singularity is the best way to go mainly because you are the same user inside and outside the singularity...

> Note I haven't seen this per se, but Kevin has and DeepNeuro made files with a cron job that had those user characteristics...

> Update: tried it and actually you can't write even though you are root:

```bash
dotter[0]:bb927$ docker run -it --rm -v /space/dotter/2/users/bb927/in:/mnt/in pytorch/pytorch:latest bash
root@7b3a11b77784:/workspace# ls /mnt/in
in.txt  new_file.txt  out.txt
root@7b3a11b77784:/workspace# touch /mnt/in/example_of_what_not_to_do.txt
touch: cannot touch '/mnt/in/example_of_what_not_to_do.txt': Permission denied

```

So if you know how to add users in your Dockerfile, you could in theory make the user inside the image your Carlsbad or Dotter user, but that's a lot of work. I hope that using singularity just works smoothly...Stay tuned for how to use SHUB, Singularities Dockerhub...