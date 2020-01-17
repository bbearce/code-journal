# SSH

## Useage

```bash
$ ssh <username>@<host-ip>
```

## Keys

> Courtesty of [stackexchange](https://unix.stackexchange.com/questions/337465/username-and-password-in-command-line-with-sshfs/381331)

**Generating a Key Pair the Proper way**

* On Local server

```bash
ssh-keygen -t rsa
```

* On remote Server

```bash
ssh root@remote_servers_ip "mkdir -p .ssh"
```

* Uploading Generated Public Keys to the Remote Server

```bash
cat ~/.ssh/id_rsa.pub | ssh root@remote_servers_ip "cat >> ~/.ssh/authorized_keys"
```

* Set Permissions on Remote server

```bash
ssh root@remote_servers_ip "chmod 700 ~/.ssh; chmod 640 ~/.ssh/authorized_keys"
```

* Login

```bash
ssh root@remote_servers_ip
```

* Enabling SSH Protocol v2

```bash
uncomment "Protocol 2" in /etc/ssh/sshd_config
```

* Enabling public key authorization in sshd

```bash
uncomment "PubkeyAuthentication yes" in /etc/ssh/sshd_config
```

* If StrictModes is set to yes in /etc/ssh/sshd_config then

```bash
restorecon -Rv ~/.ssh
```

> If you get "The program 'restorecon' is currently not installed. You can install it by typing"

```bash
restorecon -Rv ~/.ssh
```

...then run that