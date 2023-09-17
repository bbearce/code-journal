# Environments
You need to install [python](./Installs.md) first.

## Venv (virtualenv in python2)
[packaging.python.org](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/)

### Install pip
```bash
python3 -m pip install --user --upgrade pip
python3 -m pip --version
python3 -m pip install --user virtualenv
```

### Create Virtual Environment
```bash
python3 -m venv env
```

```bash
bbearce@pop-os:~/Desktop/venv_demo$ python3 -m venv env
bbearce@pop-os:~/Desktop/venv_demo$ ls
env
bbearce@pop-os:~/Desktop/venv_demo$ source env/bin/activate
(env) bbearce@pop-os:~/Desktop/venv_demo$ deactivate
bbearce@pop-os:~/Desktop/venv_demo$ . env/bin/activate # abreviated
(env) bbearce@pop-os:~/Desktop/venv_demo$
```

### Install Packages
```bash
python3 -m pip install pandas
python3 -m pip uninstall -y pandas
```

Short:
```bash
pip install pandas
pip uninstall -y pandas
```
```bash
(env) bbearce@pop-os:~/Desktop/venv_demo$ python3 -m pip install pandas
Looking in indexes: https://pypi.org/simple, https://packagecloud.io/github/git-lfs/pypi/simple
Collecting pandas
  Obtaining dependency information for pandas from https://files.pythonhosted.org/packages/d9/26/895a49ebddb4211f2d777150f38ef9e538deff6df7e179a3624c663efc98/pandas-2.1.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
  Downloading pandas-2.1.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (18 kB)
Collecting numpy>=1.23.2 (from pandas)
  Obtaining dependency information for numpy>=1.23.2 from https://files.pythonhosted.org/packages/c4/36/161e2f8110f8c49e59f6107bd6da4257d30aff9f06373d0471811f73dcc5/numpy-1.26.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
  Downloading numpy-1.26.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (58 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 58.5/58.5 kB 1.2 MB/s eta 0:00:00
Collecting python-dateutil>=2.8.2 (from pandas)
  Using cached python_dateutil-2.8.2-py2.py3-none-any.whl (247 kB)
Collecting pytz>=2020.1 (from pandas)
  Obtaining dependency information for pytz>=2020.1 from https://files.pythonhosted.org/packages/32/4d/aaf7eff5deb402fd9a24a1449a8119f00d74ae9c2efa79f8ef9994261fc2/pytz-2023.3.post1-py2.py3-none-any.whl.metadata
  Downloading pytz-2023.3.post1-py2.py3-none-any.whl.metadata (22 kB)
Collecting tzdata>=2022.1 (from pandas)
  Downloading tzdata-2023.3-py2.py3-none-any.whl (341 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 341.8/341.8 kB 8.4 MB/s eta 0:00:00
Collecting six>=1.5 (from python-dateutil>=2.8.2->pandas)
  Using cached six-1.16.0-py2.py3-none-any.whl (11 kB)
Downloading pandas-2.1.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (12.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 12.6/12.6 MB 46.8 MB/s eta 0:00:00
Downloading numpy-1.26.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.2 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 18.2/18.2 MB 34.0 MB/s eta 0:00:00
Downloading pytz-2023.3.post1-py2.py3-none-any.whl (502 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 502.5/502.5 kB 51.5 MB/s eta 0:00:00
Installing collected packages: pytz, tzdata, six, numpy, python-dateutil, pandas
Successfully installed numpy-1.26.0 pandas-2.1.0 python-dateutil-2.8.2 pytz-2

(env) bbearce@pop-os:~/Desktop/venv_demo$

(env) bbearce@pop-os:~/Desktop/venv_demo$ python3 -m pip uninstall -y pandas
Found existing installation: pandas 2.1.0
Uninstalling pandas-2.1.0:
  Successfully uninstalled pandas-2.1.0
```

> It's best practive to put virtual environments in ~/.virtualenvs. VSCode and other IDE's will look here by default for python virtual environments.
```bash
bbearce@pop-os:~/Desktop/venv_demo$ ls -la ~/.virtualenvs
total 16
drwxrwxr-x  4 bbearce bbearce 4096 Jul 22  2022 .
drwxr-xr-x 77 bbearce bbearce 4096 Sep 17 15:40 ..
drwxrwxr-x  6 bbearce bbearce 4096 Jul 22  2022 venv3.10.4
```

VSCode Example:
![venv3.10.](Environments/vscode-venv.jpg)



## Pyenv
[Official Docs](https://github.com/pyenv/pyenv)  
[realpython.com](https://realpython.com/intro-to-pyenv/#installing-pyenv) (they use the above to guide you)

If ```venv``` was manual, ```pyenv``` is automatic and steamlined to create many different environments and virtual environments.

When you use ```venv``` you are copying your main python installation into a virtual environment. You are locked into that version for virtual environements unless you manually install different python versions on your system.

With ```pyenv```, you do have to have these base python environments, but instead of installing where they normally go on your os, they are collected and managed by pyenv in ```~/.pyenv/versions```: 

```bash
bbearce@pop-os:~/Desktop/venv_demo$ ls -la ~/.pyenv
-rw-r--r--  1 bbearce bbearce      7 Sep 16 16:02 version
drwxr-xr-x  3 bbearce bbearce   4096 Sep 16 16:29 versions
...

bbearce@pop-os:~/Desktop/venv_demo$ ls ~/.pyenv/versions
3.11.5  venv3.11.5
```

Via special pyenv bash scripts, not python installations as with venv above, we can install multiple python versions (```3.11.5```) and virtual environments(```venv3.11.5```). They live in the same directory, though the virtual environments are sylinks to ./envs/ under each actual python version.

```bash
bbearce@pop-os:~/Desktop/venv_demo$ ls -la ~/.pyenv/versions/
drwxr-xr-x  7 bbearce bbearce 4096 Sep 16 16:29 3.11.5
lrwxrwxrwx  1 bbearce bbearce   52 Sep 16 16:29 venv3.11.5 -> /home/bbearce/.pyenv/versions/3.11.5/envs/venv3.11.5

bbearce@pop-os:~/Desktop/venv_demo$ ls -la ~/.pyenv/versions/3.11.5/envs/
drwxr-xr-x 5 bbearce bbearce 4096 Sep 16 16:29 venv3.11.5
```

So you can have *any* python version with *any* number of unique virtual environments.

### Create (install) Environment

See environments availabe:
```bash
pyenv install --list # all
pyenv install --list | grep " 3\.[678]" # get specific
  3.6.0
  3.6-dev
  3.6.1
  3.6.2
  3.6.3
  3.6.4
  3.6.5
  3.6.6
  3.6.7
  3.6.8
  3.7.0
  3.7-dev
  3.7.1
  3.7.2
  3.8-dev
```

```bash
pyenv install 3.10.4
# pyenv uninstall 3.10.4 # Uninstall
# rm -rf ~/.pyenv/versions/3.10.4 # Equivalent

```
```bash
bbearce@pop-os:~/Desktop/venv_demo$ pyenv install 3.10.4
Downloading Python-3.10.4.tar.xz...
-> https://www.python.org/ftp/python/3.10.4/Python-3.10.4.tar.xz
Installing Python-3.10.4...
...
Installing Python-3.10.4...
Installed Python-3.10.4 to /home/bbearce/.pyenv/versions/3.10.4
```

```bash
bbearce@pop-os:~/Desktop/venv_demo$ ls -la ~/.pyenv/versions
drwxr-xr-x  6 bbearce bbearce 4096 Sep 17 17:03 3.10.4
drwxr-xr-x  7 bbearce bbearce 4096 Sep 16 16:29 3.11.5
lrwxrwxrwx  1 bbearce bbearce   52 Sep 16 16:29 venv3.11.5 -> /home/bbearce/.pyenv/versions/3.11.5/envs/venv3.11.5
```

### Create Virtual Environment
> You need a pyenv plugin called [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv).

```bash
# Ubuntu based install...check for your os
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
```

Create:
```bash
bbearce@pop-os:~/Desktop/venv_demo$ pyenv virtualenv 3.10.4 venv3.10.4

bbearce@pop-os:~/Desktop/venv_demo$ ls -la ~/.pyenv/versions
drwxr-xr-x  6 bbearce bbearce 4096 Sep 17 17:03 3.10.4
drwxr-xr-x  7 bbearce bbearce 4096 Sep 16 16:29 3.11.5
lrwxrwxrwx  1 bbearce bbearce   52 Sep 16 16:29 venv3.11.5 -> /home/bbearce/.pyenv/versions/3.11.5/envs/venv3.11.5

bbearce@pop-os:~/Desktop/venv_demo$ pyenv virtualenv 3.10.4 venv3.10.4
bbearce@pop-os:~/Desktop/venv_demo$ ls -la ~/.pyenv/versions
drwxr-xr-x  7 bbearce bbearce 4096 Sep 17 17:19 3.10.4
drwxr-xr-x  7 bbearce bbearce 4096 Sep 16 16:29 3.11.5
lrwxrwxrwx  1 bbearce bbearce   52 Sep 17 17:19 venv3.10.4 -> /home/bbearce/.pyenv/versions/3.10.4/envs/venv3.10.4
lrwxrwxrwx  1 bbearce bbearce   52 Sep 16 16:29 venv3.11.5 -> /home/bbearce/.pyenv/versions/3.11.5/envs/venv3.11.5
```

### Using Environments
There are a variety of ways to specify when and which env\venv you want to use. Here are the most basic.

#### Global
By default you are using a global ```pyenv``` version specified by a file in ```~/.pyenv/version```.
```bash
bbearce@pop-os:~/Desktop/venv_demo$ cat ~/.pyenv/version
3.11.5
```

Set it with:
```bash
pyenv global <version>
```
```bash
bbearce@pop-os:~/Desktop/venv_demo$ pyenv version
3.11.5 (set by /home/bbearce/.pyenv/version)

bbearce@pop-os:~/Desktop/venv_demo$ pyenv versions
  system
  3.10.4
  3.10.4/envs/venv3.10.4
* 3.11.5 (set by /home/bbearce/.pyenv/version)
  3.11.5/envs/venv3.11.5
  venv3.10.4 --> /home/bbearce/.pyenv/versions/3.10.4/envs/venv3.10.4
  venv3.11.5 --> /home/bbearce/.pyenv/versions/3.11.5/envs/venv3.11.5
```

```bash
bbearce@pop-os:~/Desktop/venv_demo$ python
Python 3.11.5 (main, Sep 16 2023, 15:49:34) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```


#### Shell
Activates for quick use (essentially the same as ```venv```).
```bash
pyenv shell <version>
```
Regular env:
```bash
bbearce@pop-os:~/Desktop/venv_demo$ pyenv shell 3.11.5
bbearce@pop-os:~/Desktop/venv_demo$ pyenv version
3.11.5 (set by PYENV_VERSION environment variable)
```

Virtualenv:
```bash
pyenv activate <version>
pyenv deactivate
```
```bash
bbearce@pop-os:~/Desktop/venv_demo$ pyenv activate venv3.11.5
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ pyenv version
venv3.11.5 (set by PYENV_VERSION environment variable)
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ pyenv deactivate
bbearce@pop-os:~/Desktop/venv_demo$ 
```
You can use these with shell too.
```bash
bbearce@pop-os:~/Desktop/venv_demo$ pyenv shell venv3.11.5
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ pyenv version
venv3.11.5 (set by PYENV_VERSION environment variable)
```
However deactivate doesn't work:
```bash
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ pyenv deactivate
bbearce@pop-os:~/Desktop/venv_demo$ pyenv shell venv3.11.5
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ pyenv deactivate
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ deactivate
pyenv-virtualenv: deactivate must be sourced. Run 'source deactivate' instead of 'deactivate'
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ source deactivate
pyenv-virtualenv: deactivate 3.11.5/envs/venv3.11.5
# Need to use shell again to switch
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ pyenv shell 3.11.5
```


#### Local
This is set for a direcory or project and creates a ```.python-version``` inside the directory
```bash
pyenv local <version>
```
```bash
bbearce@pop-os:~/Desktop/venv_demo$ pyenv version
3.11.5 (set by /home/bbearce/.pyenv/version)
bbearce@pop-os:~/Desktop/venv_demo$ pyenv local venv3.11.5
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ ls -la
drwxrwxr-x 5 bbearce bbearce  4096 Sep 17 15:32 env
-rw-rw-r-- 1 bbearce bbearce    11 Sep 17 17:29 .python-version
-rw-rw-r-- 1 bbearce bbearce 22572 Sep 17 15:48 vscode-venv.jpg
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ cat .python-version 
venv3.11.5
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ pyenv version
venv3.11.5 (set by /home/bbearce/Desktop/venv_demo/.python-version)
(venv3.11.5) bbearce@pop-os:~/Desktop/venv_demo$ rm .python-version # deactivates and removes .python-version
bbearce@pop-os:~/Desktop/venv_demo$
```

## PDM
