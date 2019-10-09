# Installs

## Installing

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
```bash
$ ./configure
$ make
$ make test
$ sudo make altinstall
```