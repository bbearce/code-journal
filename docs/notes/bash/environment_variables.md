# Environment Variables

## $PATH

> Courtesy of [smallbusiness.chron.com](https://smallbusiness.chron.com/modify-path-ubuntu-52030.html)

Ubuntu Linux, as well as all other Linux distributions, uses the PATH variable to tell the operating system where to look for executable commands. Typically these commands are located in the /usr/sbin, usr/bin and /sbin, and /bin directories. Other command directories can be added to this list of directories by adding them to the PATH variable. You can choose to make a user-specified directory available for a single user or the entire system, depending on your company's requirements.

### Add to Path

Add a path to the ```PATH``` variable:

```bash
export PATH=$PATH:/my/custom/path
```

This appends your new path to the old path and overwrites PATH.

### Remove from Path

Execute ``` echo $PATH``` to see full path variable and copy it to clipboard. Then remove a specific path between colons ```...:<some path>:<path to be removed>:...``` and paste that into this command:

```bash
export PATH=<new full path minus the path you do not want>
```

> Note thatwe aren't appending to ```$PATH``` this time, but overwriting it.