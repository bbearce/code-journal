# Installs

## Installing python3.6

>Source [realpython](https://realpython.com/installing-python/)

Ubuntu 17.10, Ubuntu 18.04 (and above) come with Python 3.6 by default. You should be able to invoke it with the command ```python3```.

Ubuntu 16.10 and 17.04 do not come with Python 3.6 by default, but it is in the Universe repository. You should be able to install it with the following commands:

```bash
$ sudo apt-get update
$ sudo apt-get install python3.6
```

You can then invoke it with the command ```python3.6```.

If you are using Ubuntu 14.04 or 16.04, Python 3.6 is not in the Universe repository, and you need to get it from a Personal Package Archive (PPA). For example, to install Python from the “deadsnakes” PPA, do the following:

```bash
$ sudo add-apt-repository ppa:deadsnakes/ppa
$ sudo apt-get update
$ sudo apt-get install python3.6
```

As above, invoke with the command ```python3.6```.

## Installing python3.7+

>Source [linuxize](https://linuxize.com/post/how-to-install-python-3-7-on-ubuntu-18-04/)

### Install with Apt

Installing Python 3.7 on Ubuntu with apt is a relatively straightforward process and will only take a few minutes:

Start by updating the packages list and installing the prerequisites:

```bash
sudo apt update
sudo apt install software-properties-common
```

Next, add the deadsnakes PPA to your sources list:

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
```

When prompted press Enter to continue:

```bash
Press [ENTER] to continue or Ctrl-c to cancel adding it.
```

Once the repository is enabled, install Python 3.7 with:

```bash
sudo apt install python3.7
```

At this point, Python 3.7 is installed on your Ubuntu system and ready to be used. You can verify it by typing:

```bash
python3.7 --version
Python 3.7.3
```

### Install from source
>Source [linuxize](https://linuxize.com/post/how-to-install-python-3-7-on-ubuntu-18-04/)

This first step may or may not be necessary but is probably a good idea. First, update the packages list and install the packages necessary to build Python source:

```bash
sudo apt update
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget
```

You an use ```wget``` to grab the latest like so:

```bash
cd /usr/src
sudo wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
```
You can also go to https://www.python.org/ and download it. This gave me a Python-3.7.4.tar.xz file. To unzip it use:
```bash
$ tar -xf Python-3.7.4.tar.xz 
$ ls -la
drwxr-xr-x 18 bbearce bbearce     4096 Jul  8 14:31 Python-3.7.4
-rw-rw-r--  1 bbearce bbearce 17131432 Sep  4 11:09 Python-3.7.4.tar.xz
```

*To see other unzipping techniques for different file types try [here](/bash/tar_files)*

Change into that new directory and use ```make``` to install Python:

```bash
$ cd Python-3.7.4
```

*Note: Notice the README.rst*

```bash
$ vi README.rst
```

...

Build Instructions
------------------

On Unix, Linux, BSD, macOS, and Cygwin::

    ./configure
    make
    make test
    sudo make install

This will install Python as ``python3``.

...

Installing multiple versions
----------------------------

On Unix and Mac systems if you intend to install multiple versions of Python
using the same installation prefix (``--prefix`` argument to the configure
script) you must take care that your primary python executable is not
overwritten by the installation of a different version.  All files and
directories installed using ``make altinstall`` contain the major and minor
version and can thus live side-by-side.  ``make install`` also creates
``${prefix}/bin/python3`` which refers to ``${prefix}/bin/pythonX.Y``.  If you
intend to install multiple versions using the same prefix you must decide which
version (if any) is your "primary" version.  Install that version using ``make
install``.  Install all other versions using ``make altinstall``.


To continue run ```make```:

> Note that 

```bash
$ ./configure
$ make
$ make test
$ sudo make altinstall
```