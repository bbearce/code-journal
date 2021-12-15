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