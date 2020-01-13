# Basic bash commands

## Help from the ```man``` ;)

This small section is to show you that help is on the way with ```man```. Type this prefacing any other command in bash and you can get help on it:
```bash
$ man ls
NAME
       ls - list directory contents
.
.
.
```


## Where are you?

Find your location with ```pwd```:
```bash
bbearce@bbearce-XPS-15-9560:~$ pwd
/home/bbearce

```

Look around with ```ls```:
```bash
bbearce@bbearce-XPS-15-9560:~$ ls
check-config.sh                gems          Slicer-4.10.2-linux-amd64
Desktop                        Music         snap
docker_practice                pgAdmin4      src
Documents                      Pictures      Templates
Downloads                      Public        Videos
Dropbox (Partners HealthCare)  R             wget-log
examples.desktop               seaborn-data


```

### -l

With ```-l``` you can see things listed out vertically
```bash
bbearce@bbearce-XPS-15-9560:~$ ls -l
total 2268
-rw-rw-r--  1 bbearce bbearce   10314 May 20 09:40 check-config.sh
drwxr-xr-x  2 bbearce bbearce    4096 Sep 18 11:15 Desktop
drwxrwxr-x  2 bbearce bbearce    4096 Sep  5 18:11 docker_practice
drwxr-xr-x 14 bbearce bbearce    4096 Sep 18 13:37 Documents
drwxr-xr-x  4 bbearce bbearce    4096 Sep 19 14:44 Downloads
drwx------  7 bbearce bbearce    4096 Sep 18 11:22 Dropbox (Partners HealthCare)
-rw-r--r--  1 bbearce bbearce    8980 May 16 16:13 examples.desktop
drwxrwxr-x  9 bbearce bbearce    4096 Jul 15 13:53 gems
drwxr-xr-x  2 bbearce bbearce    4096 May 16 16:49 Music
drwxrwxr-x  3 bbearce bbearce    4096 Aug  8 09:10 pgAdmin4
drwxr-xr-x  2 bbearce bbearce    4096 Jun 17 15:07 Pictures
drwxr-xr-x  2 bbearce bbearce    4096 May 16 16:49 Public
drwxrwxr-x  3 bbearce bbearce    4096 May 17 09:02 R
drwxrwxr-x  2 bbearce bbearce    4096 Jul 19 17:36 seaborn-data
drwxrwxr-x  8 bbearce bbearce    4096 Jun  4 13:06 Slicer-4.10.2-linux-amd64
drwxr-xr-x  8 bbearce bbearce    4096 Jul 25 16:43 snap
drwxrwxr-x  3 bbearce bbearce    4096 May 23 16:12 src
drwxr-xr-x  2 bbearce bbearce    4096 May 16 16:49 Templates
drwxr-xr-x  2 bbearce bbearce    4096 Aug 30 13:55 Videos
-rw-rw-r--  1 bbearce bbearce 2220414 Aug 16 17:05 wget-log

```

The columns are *using ls -lai*:
```
+--------------+------------------+-----------------+-------+-------+------+-------+-----+-------+-----------+
| index number | file permissions | number of links | owner | group | size | month | day | time  | filename  |
+--------------+------------------+-----------------+-------+-------+------+-------+-----+-------+-----------+
|       933442 | -rwxrw-r--       |              10 | root  | root  | 2048 | Jan   |  13 | 07:11 | afile.exe |
+--------------+------------------+-----------------+-------+-------+------+-------+-----+-------+-----------+
```

### -a

With ```-a``` you show hidden files:

> Hidden files are denoted with a ```.<file_name>```

```bash
bbearce@bbearce-XPS-15-9560:~$ ls -a
.                              gems                .pyenv
..                             .gitconfig          .pylint.d
.azure                         .gksu.lock          .python_history
.azure-shell                   .gnome              R
.bash_history                  .gnupg              .Rhistory
.bash_logout                   .hplip              .rstudio-desktop
.bashrc                        .ICEauthority       seaborn-data
.bundle                        .ipython            Slicer-4.10.2-linux-amd64
.cache                         .java               snap
check-config.sh                .jupyter            .sqlite_history
.compiz                        .kde                src
.config                        .lesshst            .ssh
.DataGrip2019.1                .local              .sudo_as_admin_successful
Desktop                        .mozilla            .systemtap
.dmrc                          Music               Templates
.docker                        .nano               Videos
docker_practice                .node_repl_history  .viminfo
Documents                      .pgadmin            .vscode
Downloads                      pgAdmin4            .wget-hsts
.dropbox                       Pictures            wget-log

```

### -la

Using both ```-la```:
```bash
bbearce@bbearce-XPS-15-9560:~$ ls -la
total 2564
drwxrwxrwx 50 bbearce bbearce    4096 Sep 19 22:01 .
drwxr-xr-x  3 root    root       4096 May 29 15:05 ..
drwxrwxr-x  6 bbearce bbearce    4096 May 23 16:51 .azure
drwxrwxr-x  3 bbearce bbearce    4096 May 23 16:51 .azure-shell
-rw-------  1 bbearce bbearce   56352 Sep 19 18:05 .bash_history
-rw-r--r--  1 bbearce bbearce     220 May 16 16:13 .bash_logout
-rw-r--r--  1 bbearce bbearce    3865 Jun 20 16:55 .bashrc
drwxrwxr-x  4 bbearce bbearce    4096 Jul 10 14:28 .bundle
drwx------ 48 bbearce bbearce    4096 Sep  9 13:31 .cache
-rw-rw-r--  1 bbearce bbearce   10314 May 20 09:40 check-config.sh
drwx------  3 bbearce bbearce    4096 May 16 16:50 .compiz
drwx------ 43 bbearce bbearce    4096 Sep 12 11:10 .config
drwxrwxr-x  4 bbearce bbearce    4096 Jul 25 16:43 .DataGrip2019.1
drwxr-xr-x  2 bbearce bbearce    4096 Sep 18 11:15 Desktop
-rw-r--r--  1 bbearce bbearce      25 May 16 16:49 .dmrc
drwxrwx---  3 bbearce bbearce    4096 Sep  5 17:35 .docker
drwxrwxr-x  2 bbearce bbearce    4096 Sep  5 18:11 docker_practice
drwxr-xr-x 14 bbearce bbearce    4096 Sep 18 13:37 Documents
drwxr-xr-x  4 bbearce bbearce    4096 Sep 19 14:44 Downloads
drwx------  7 bbearce bbearce    4096 Sep 19 22:01 .dropbox
drwxrwxr-x  3 bbearce bbearce    4096 Sep 18 03:24 .dropbox-dist
drwx------  7 bbearce bbearce    4096 Sep 18 11:22 Dropbox (Partners HealthCare)
-rw-r--r--  1 bbearce bbearce    8980 May 16 16:13 examples.desktop
drwx------  2 bbearce bbearce    4096 Sep 17 06:50 .gconf
drwxrwxr-x  4 bbearce bbearce    4096 Jun 20 16:33 .gem
drwxrwxr-x  9 bbearce bbearce    4096 Jul 15 13:53 gems
-rw-rw-r--  1 bbearce bbearce      57 May 21 17:11 .gitconfig
-rw-r-----  1 bbearce bbearce       0 May 31 12:23 .gksu.lock
drwx------  3 bbearce bbearce    4096 May 17 08:25 .gnome
drwx------  3 bbearce bbearce    4096 Sep 19 22:01 .gnupg
drwxr-xr-x  2 bbearce bbearce    4096 Jun 28 10:21 .hplip
-rw-------  1 bbearce bbearce   27762 Sep 19 22:01 .ICEauthority
drwxr-xr-x  5 bbearce bbearce    4096 Jun 16 18:30 .ipython
drwxrwxr-x  4 bbearce bbearce    4096 Jul 25 16:43 .java
drwxrwxr-x  3 bbearce bbearce    4096 Jun 16 22:35 .jupyter
drwx------  3 bbearce bbearce    4096 Jun  9 21:43 .kde
-rw-------  1 bbearce bbearce      99 Sep  5 19:01 .lesshst
drwx------  6 bbearce bbearce    4096 Jun 16 18:30 .local
drwx------  5 bbearce bbearce    4096 May 17 08:13 .mozilla
drwxr-xr-x  2 bbearce bbearce    4096 May 16 16:49 Music
drwxrwxr-x  2 bbearce bbearce    4096 May 22 15:23 .nano
-rw-rw-r--  1 bbearce bbearce      55 Aug  6 09:48 .node_repl_history

```




### --block_size=SIZE

> SIZE units  are  K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000).


```bash
bbearce@bbearce-XPS-15-9560:~$ ls -la --block-size=M
total 3M
drwxrwxrwx 50 bbearce bbearce 1M Sep 20 09:32 .
drwxr-xr-x  3 root    root    1M May 29 15:05 ..
drwxrwxr-x  6 bbearce bbearce 1M May 23 16:51 .azure
drwxrwxr-x  3 bbearce bbearce 1M May 23 16:51 .azure-shell
-rw-------  1 bbearce bbearce 1M Sep 19 18:05 .bash_history
-rw-r--r--  1 bbearce bbearce 1M May 16 16:13 .bash_logout
...
```

## sudo, su

> Notes courtesy of [maketecheasier.com](https://www.maketecheasier.com/differences-between-su-sudo-su-sudo-s-sudo-i/)

Using ```sudo``` you can execute code as superuser. It stands for *super user do*.

```bash
$ man sudo
NAME
     sudo, sudoedit — execute a command as another user
.
.
.
```

```bash
$ man su

NAME
       su - change user ID or become superuser
.
.
.

```

### $ su

The ```su``` command substitutes the current user in use by the system in the shell. This will tell the system to switch (and essentially log out of) the current user to the one specified.. ```su``` is best used when a user wants direct access to the root account on the system. It doesn’t go through sudo or anything like that. It is [disabled](https://askubuntu.com/questions/446570/why-does-su-fail-with-authentication-error) by default on Ubuntu. It is recommended to use ```sudo -i``` in this case. As the note above mentions, this drops you at ```/root```.

### $ sudo su

This command is essentially the same as just running ```su``` in the shell. Instead of telling the system to “switch users” directly, you’re telling it to run the “su” command as root. When ```sudo su``` is run, “.profile,” “.bashrc” and “/etc/profile” will be started, much like running ```su``` (or ```su root```). This is because if any command is run with sudo in front of it, it’s a command that is given root privileges.

Though there isn’t very much difference from “su,” ```sudo su``` is still a very useful command for one important reason: When a user is running “su” to gain root access on a system, they must know the root password. The way root is given with ```sudo su``` is by requesting the current user’s password. This makes it possible to gain root without the root password which increases security.

### $ sudo -i

Using ```sudo -i``` is virtually the same as the ```sudo su``` command. Users can gain root by “sudo” and not by switching to the root user. Much like ```sudo su```, the ```-i``` flag allows a user to get a root environment without having to know the root account password. sudo ```-i``` is also very similar to using ```sudo su``` in that it’ll read all of the environmental files (.profile, etc.) and set the environment inside the shell with it.

Where it differs from “sudo su” is that ```sudo``` -i is a much **cleaner** way of gaining root and a root environment without directly interacting with the root user. How? With ```sudo su``` you’re using more than one root ```setuid``` commands. This fact makes it much more challenging to figure out what environmental variables will be kept and which ones will be changed (when swamping to the root environment). This is not true with ```sudo -i```, and it is because of this most people view it as the preferred method to gain root without logging in directly.

### $ sudo -s

The ```-s``` switch for “sudo” command reads the $SHELL variable of the current user executing commands. This command works as if the user is running ```sudo /bin/bash```. ```sudo -s``` is a “non-login” style shell. This means that unlike a command like ```sudo -i``` or ```sudo su```, the system will not read any environmental files. This means that when a user tells the shell to run ```sudo -s```, it gains root but will not change the user or the user environment. Your home will not be the root home, etc.

This command is best used when the user doesn’t want to touch root at all and just wants a root shell for easy command execution. Other commands talked about above gain root access, but touch root environmental files, and allow users more fuller access to root (which can be a security issue).

#### Summary and demo of ```pwd``` when running these commands:

```bash
bbearce@bbearce-XPS-15-9560:~$ sudo su
root@bbearce-XPS-15-9560:/home/bbearce# pwd
/home/bbearce
root@bbearce-XPS-15-9560:/home/bbearce# exit
exit

bbearce@bbearce-XPS-15-9560:~$ sudo -i
root@bbearce-XPS-15-9560:~# pwd
/root
root@bbearce-XPS-15-9560:~# exit
logout

bbearce@bbearce-XPS-15-9560:~$ sudo -s
root@bbearce-XPS-15-9560:~# pwd
/home/bbearce
root@bbearce-XPS-15-9560:~# exit
exit

```

> btw: ```exit``` is for leaving a current logged in session.


## bash or sh or dash?

> Notes courtesy of [diffzi.com](https://diffzi.com/bash-vs-dash/)

Bash (```bash```) is one of many available (yet the most commonly used) Unix shells. Bash stands for "**B**ourne **A**gain **SH**ell", and is a replacement/improvement of the original Bourne shell (```sh```).

### What is Bash?
Bash is the Bourne-Again shell. Bash is an excellent full-featured shell appropriate for interactive use; indeed, it is still the default login shell. Bash is designed for human beings and provides a superset of POSIX functionality.

### What is Dash?
Dash  is the  Debian  Almquist  Shell. Dash implements the Single Unix Spec, then it does not have to do more to meet that formal spec. Dash is for non-interactive script execution. Dash Only supports POSIX compliant features.

### Key Differences

>1. Bash is an excellent full-featured shell appropriate for interactive use; indeed, it is still the default login shell. However, it is rather large and slow to start up and operate by comparison with dash.
2. Dash implements the Single Unix Spec, then it does not have to do more to meet that formal spec. But some of the “bashisms” are convenient, would add little to the size of dash, and would make it far easier to use dash as an alternative.
3. A lot of shell scripts which contain the command set –k are not supported by dash but supported by bash.
4. Bash Supports the same scripting commands as Dash as well as its own additional commands, Dash Only supports POSIX compliant features.
5. Bash is designed for human beings and provides a superset of POSIX functionality, Dash is for non-interactive script execution.
6. Bash supports tab completion and Supports a command history.
7. Dash is only 100K compared to Bash’s 900K.
8. Dash is for Faster start-up and script execution as compared to Bash.


## Moving and Copying

Use the ```mv``` command to move things or rename them.  
Use the ```cp``` command to copy things.  
Use the ```rsync``` command to move things. Use the *-a* flag to archive and retain permissions and time stamps.  

## Check OS Version

```
$ cat /etc/os-release
```

## make files

> Courtesty of [stackoverflow](https://stackoverflow.com/questions/3915067/what-are-makefiles-make-install)

```make``` is part of the build system commonly used in unix type systems - [binutils](http://www.gnu.org/software/binutils/).

It looks at make files which hold configuration information and build targets.

Specifically:

* ./configure - this is a script that sets up the environment for the build  
* make - calls ```make``` with the default build target. Normally builds the app.  
* make install - calls ```make``` with the ```install``` build target. Normally installs the app.  

## sshfs

> Courtesy of [github](https://github.com/libfuse/sshfs)

### About
SSHFS allows you to mount a remote filesystem using SFTP. Most SSH servers support and enable this SFTP access by default, so SSHFS is very simple to use - there's nothing to do on the server-side.

### Development Status
SSHFS is shipped by all major Linux distributions and has been in production use across a wide range of systems for many years. However, at present SSHFS does not have any active, regular contributors, and there are a number of known issues (see the bugtracker). The current maintainer continues to apply pull requests and makes regular releases, but unfortunately has no capacity to do any development beyond addressing high-impact issues. When reporting bugs, please understand that unless you are including a pull request or are reporting a critical issue, you will probably not get a response.

### How to use
Once sshfs is installed (see next section) running it is very simple:

```bash
$ sshfs [user@]hostname:[directory] mountpoint
```

It is recommended to run SSHFS as regular user (not as root). For this to work the mountpoint must be owned by the user. If username is omitted SSHFS will use the local username. If the directory is omitted, SSHFS will mount the (remote) home directory. If you need to enter a password sshfs will ask for it (actually it just runs ssh which ask for the password if needed).

Also many ssh options can be specified (see the manual pages for sftp(1) and ssh_config(5)), including the remote port number (-oport=PORT)

To unmount the filesystem:

```bash
fusermount -u mountpoint
```

On BSD and macOS, to unmount the filesystem:

```bash
umount mountpoint
```

*If you get an error:*
```bash
fuse: mountpoint is not empty
fuse: if you are sure this is safe, use the 'nonempty' mount option
```
You might need to kill all sshfs processes and restart:
```bash
~$ killall sshfs
```
*Then restart your sshfs mounting procedure*

