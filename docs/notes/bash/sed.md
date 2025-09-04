# sed

## Search
> `grep` is likely better
```bash
$ sed -n "/pattern/p" file.txt

$ cat test.txt 
asdfasdf
fjfjfjfjfj
aaaaa

$ sed -n "/a/p" test.txt 
asdfasdf
aaaaa
```

## Find and Replate
> `g` is for global or replace all
```bash
$ sed -i -e "s/pattern_to_find/replace_string/" file.txt

$ sed -i -e "s/pattern_to_find/replace_string/g" file.txt
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
$ cat deleteme.txt 
abc

abc

abc
$ sed -i -e "s/abc/XYZ/g" deleteme.txt
$ cat deleteme.txt 
XYZ

XYZ

XYZ
```

## Find Line
> [Source](https://www.baeldung.com/linux/read-specific-line-from-file)

```bash
$ cat test.txt 
asdfasdf
fjfjfjfjfj
aaaaa

$ sed -n "1p" test.txt 
asdfasdf
$ sed -n "2p" test.txt 
fjfjfjfjfj
$ sed -n "1,2p" test.txt 
asdfasdf
fjfjfjfjfj
$ sed -n "1~2p" test.txt 
asdfasdf
aaaaa
$ sed -n "2{p;q}" test.txt 
fjfjfjfjfj
$ sed -n "1,2{p;q}" test.txt 
asdfasdf
```

```bash
* -n, --quiet, --silent
              suppress automatic printing of pattern space

* “-n ‘5p’” means print only the fifth line

* The sed has provided a ‘q‘ command that allows to “quit” further processing. We can put the ‘q‘ command in the two one-liners

* {p;q} is just to apply the p and the q command
```
