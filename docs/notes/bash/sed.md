# sed

## Find and Replate

```bash
$ sed -i -e "" file.txt
```

Flags:
```bash
-i[SUFFIX], --in-place[=SUFFIX]

              edit files in place (makes backup if SUFFIX supplied)
```

```bash
-e script, --expression=script

              add the script to the commands to be executed
```

Ex:
```bash
bbearce@pop-os:~$ cat deleteme.txt 
abc

abc

abc
bbearce@pop-os:~$ sed -i -e "s/abc/XYZ/g" deleteme.txt
bbearce@pop-os:~$ cat deleteme.txt 
XYZ

XYZ

XYZ
```

## Find Line
> [Source](https://www.baeldung.com/linux/read-specific-line-from-file)

```bash
$ sed -n '5{p;q}' input.txt
```

```bash
* -n, --quiet, --silent
              suppress automatic printing of pattern space

* “-n ‘5p’” means print only the fifth line

* The sed has provided a ‘q‘ command that allows to “quit” further processing. We can put the ‘q‘ command in the two one-liners
```
