# Zipping and Unzipping

## zip, unzip

> Courtesty of [geeksforgeeks.org](https://www.geeksforgeeks.org/zip-command-in-linux-with-examples/)

### zip
```bash
$ man zip
NAME
       zip - package and compress (archive) files

.
.
.
```

### zipping files
To zip files into a *.zip* file run this command:
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
something.txt  wheel.py

bbearce@bbearce-XPS-15-9560:~/Desktop$ zip example.zip wheel.py something.txt 
  adding: wheel.py (deflated 79%)
  adding: something.txt (stored 0%)

bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
example.zip  something.txt  wheel.py

```

### zipping folder contents
To zip eveything in a directory into zip file use ```./*```
> This is useful for pointing to ```<some directory>/*```
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
something.txt  wheel.py

bbearce@bbearce-XPS-15-9560:~/Desktop$ zip example.zip ./*
  adding: something.txt (stored 0%)
  adding: wheel.py (deflated 79%)

bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
example.zip  something.txt  wheel.py
```

Folders will be picked up but **not** their contents without ```-r```:
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
directory  something.txt  wheel.py

bbearce@bbearce-XPS-15-9560:~/Desktop$ ls directory/
test.txt

bbearce@bbearce-XPS-15-9560:~/Desktop$ zip example.zip ./*
  adding: directory/ (stored 0%)
  adding: something.txt (stored 0%)
  adding: wheel.py (deflated 79%)

bbearce@bbearce-XPS-15-9560:~/Desktop$ zip -r example.zip ./*
updating: directory/ (stored 0%)
updating: something.txt (stored 0%)
updating: wheel.py (deflated 79%)
  adding: directory/test.txt (stored 0%)
```

> Notice how zipping to *example.zip* again updates pre-existing files and adds new ones.
> Also flag ```-j``` can be used to make sure a folder's contents only get zipped. By default if you include a folder name, the folder is included in the zip file. Using ```-j``` will explicitly exclude the folder.

### unzip
```bash
$ man unzip
NAME
       unzip - list, test and extract compressed files in a ZIP archive

.
.
.
```

To unzip simply use the ```unzip``` command:
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
example.zip

bbearce@bbearce-XPS-15-9560:~/Desktop$ unzip example.zip 
Archive:  example.zip
   creating: directory/
 extracting: something.txt           
  inflating: wheel.py                
 extracting: directory/test.txt      

bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
directory  example.zip  something.txt  wheel.py
```

We can unzip to a specific directory with the ```-d``` flag:
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
example.zip

bbearce@bbearce-XPS-15-9560:~/Desktop$ unzip example.zip -d test
Archive:  example.zip
   creating: test/directory/
 extracting: test/something.txt      
  inflating: test/wheel.py           
 extracting: test/directory/test.txt

bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
example.zip  test

bbearce@bbearce-XPS-15-9560:~/Desktop$ ls test
directory  something.txt  wheel.py
```

If the directory already exists, we can dump the contents into it:
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
example.zip

bbearce@bbearce-XPS-15-9560:~/Desktop$ mkdir test
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
example.zip  test

bbearce@bbearce-XPS-15-9560:~/Desktop$ unzip example.zip -d test
Archive:  example.zip
   creating: test/directory/
 extracting: test/something.txt      
  inflating: test/wheel.py           
 extracting: test/directory/test.txt 

bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
example.zip  test

bbearce@bbearce-XPS-15-9560:~/Desktop$ ls test
directory  something.txt  wheel.py

```

## Tar Files

### .tar

The ```tar``` command is used to create tar archives by converting a group of files into an archive. It supports a vast range of compression programs such as ```gzip```, ```bzip2```, ```lzip```, ```lzma```, ```lzop```, ```xz``` and ```compress```. Tar was originally designed for creating archives to store files on magnetic tape which is why it has its name “**T**ape **AR**chive”.

#### Create a .tar
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
test.txt
bbearce@bbearce-XPS-15-9560:~/Desktop$ tar -cvf test.tar test.txt
test.txt
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
test.tar  test.txt
```

> Here ```-cvf``` means *c: create; v: verbose; f: use archive file or device (we can and did specify a filename to use)*. Keep in mind we didn't use the ```-z``` flag which would make a ```gzip```.


### .tar.gz

> ```.tar.gz``` files are just files that have been zipped with -z to make a gzip.

Let's redo the above example with ```-z```:
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
test.txt
bbearce@bbearce-XPS-15-9560:~/Desktop$ tar -cvzf test.tar test.txt
test.txt
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
test.tar  test.txt
```

> Note that the archive name ```test.tar``` is a ```.tar.gz``` but you have to specify that explicitly. So a canonical way of diong this is: ```tar -cvzf test.tar.gz test.txt```.
> Also the [-] is optional, so ```tar cvzf test.tar.gz test.txt``` would work too.

We can see that the .tar.gz is smaller with ```du```
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ du -BK ./*
20K     ./test.tar
4K      ./test.tar.gz
12K     ./test.txt

```


#### Unzip a .tar

We need the ```-x``` option:

```bash
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
test.tar
bbearce@bbearce-XPS-15-9560:~/Desktop$ tar -xvf test.tar
test.txt
bbearce@bbearce-XPS-15-9560:~/Desktop$ ls
test.tar  test.txt

```

When downloading 3DSlicer you get a \*.tar.gz file. This command installs it.

```bash
tar zxvf Slicer-4.8.1-linux-amd64.tar.gz
```

FYI: ([Link](https://stackoverflow.com/questions/21929223/what-does-zxvf-mean-in-tar-zxvf-filename))
