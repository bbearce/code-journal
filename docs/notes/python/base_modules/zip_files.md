# Working With Zip Files

> Source: [medium.com](https://medium.com/dev-bits/ultimate-guide-for-working-with-i-o-streams-and-zip-archives-in-python-3-6f3cf96dca50)

A file recognized by Python can store three types of data:

* Text (string)
* Binary (bytes)
* Raw data

Python considers an object falling in the above three categories as a *“file-like object.”* They are also called streams from where data can be read from or written. The data stored in streams are called buffers of that stream. The first two, i.e. Text and Binary streams, are buffered I/O streams, and raw type is unbuffered. In this article, we are only interested in *buffered* streams.

Python’s io package provides two classes:

1. StringIO: for storing UTF-8 string buffers
2. BytesIO: for storing binary buffers

## Text Streams

```python
from io import StringIO

text_stream = StringIO()
text_stream.write("I am a text buffer") # prints 18
print(text_stream.getvalue()) # Prints 'I am a text buffer' to console
text_stream.close()
```

> One should be aware that, in Python, a file-like object can be used in any I/O operation. The classic example is the *print* statement.

Python’s print statement takes a keyword argument called file that decides which stream to write the given message/objects. Its value is a “file-like object.” See the definition of print,

```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

Let us modify our program to change the destination of print to our custom text stream.

```python
from io import StringIO

text_stream = StringIO()
text_stream.write("I am a text buffer")

print(" in python", file=text_stream) # Doesn't print to console, instead writes to stream
print(text_stream.getvalue()) # Prints 'I am a text buffer in python' to console
text_stream.close()
```

Text streams are only useful in operating on UTF-8 buffers(XML, JSON, CSV). There are many other cases where we have to represent binary buffers(ZIP, PDF, Custom Extensions) in program memory. Binary streams come to the rescue.

## Binary Streams

A binary stream stores and operates on binary data(bytes). It has the same methods as StringIO like getvalue, read, write.


```python
from io import BytesIO, StringIO

binary_stream =  BytesIO(b'I am a byte string \x01')

print(binary_stream.getvalue()) # Prints b'I am a byte string \x01'

try:
  text_stream = StringIO(binary_stream.getvalue())
except TypeError:
  print('Sorry, text stream cannot store bytes') # Prints 'Sorry, text stream cannot store bytes'
```

One can store any binary data coming from a PDF or ZIP file into a custom binary stream like the preceding one.

## Understanding the Python `zipfile` API

A zip file is a binary file. Contents of a zip file are compressed using an algorithm and paths are preserved. All open-source zip tools do the same thing, understand the binary representation, process it. It is a no-brainer that one should use BytesIO while working with zip files. Python provides a package to work with zip archives called ```zipfile```.

The ```zipfile``` package has two main classes:

1. ```ZipFile``` : to represent a zip archive in memory.
2. ```ZipInfo``` : to represent a member of a zip file.

A ```ZipFile``` is an exact representation of a zip archive. It means you can load a .zip file directly into that class object or dump a ```ZipFile``` object to a new archive. Every ```ZipFile``` has a list of members. Those members are ZipInfo objects.

```bash
mkdir -p config/app
mkdir -p config/docker
touch config/app/app-config.json
touch config/docker/docker-compose.yaml
touch config/root-config.json
zip -r config.zip config
```

```bash
(venv) bbearce@pop-os:~$ python3 -m zipfile -l config.zip
File Name                                             Modified             Size
config/                                        2020-12-11 15:32:24            0
config/root-config.json                        2020-12-11 15:32:24            0
config/docker/                                 2020-12-11 15:32:16            0
config/docker/docker-compose.yaml              2020-12-11 15:32:16            0
config/app/                                    2020-12-11 15:32:06            0
config/app/app-config.json
```

using python interactively:

```python
from zipfile import ZipFile

with ZipFile('config.zip') as zip_archive:
    for item in zip_archive.filelist:
        print(item)
    print(f'\nThere are {len(zip_archive.filelist)} ZipInfo objects present in archive')
```

out:
```python
<ZipInfo filename='config/' filemode='drwxrwxr-x' external_attr=0x10>
<ZipInfo filename='config/root-config.json' filemode='-rw-rw-r--' file_size=0>
<ZipInfo filename='config/docker/' filemode='drwxrwxr-x' external_attr=0x10>
<ZipInfo filename='config/docker/docker-compose.yaml' filemode='-rw-rw-r--' file_size=0>
<ZipInfo filename='config/app/' filemode='drwxrwxr-x' external_attr=0x10>
<ZipInfo filename='config/app/app-config.json' filemode='-rw-rw-r--' file_size=0>

There are 6 ZipInfo objects present in archive
```

### Use case #1: Create zip archive with files

We can create a zip file with the given name by opening a new ZipFile object with write mode ‘w’ or exclusive create mode ‘x’.

Next, we can add files/paths to the zip archive. There are two approaches to do that:

#### v1: Add a file as a file-like object

```python
from zipfile import ZipFile
from io import BytesIO

def create_zip_v1():
    """
    returns: zip archive
    """
    archive = BytesIO()
    with ZipFile(archive, 'w') as zip_archive:
        # Create three files on zip archive
        with zip_archive.open('docker/docker-compose.yaml', 'w') as file1:
            file1.write(b'compose-file-content...')
        with zip_archive.open('app/app-config.json', 'w') as file2:
            file2.write(b'app-config-content...')
        with zip_archive.open('root-config.json', 'w') as file3:
            file3.write(b'root-config-content...')
    return archive

archive = create_zip_v1()
# Flush archive stream to a file on disk
with open('config.zip', 'wb') as f:
    f.write(archive.getbuffer())

archive.close()
```

#### v2: Add a file as a ZipInfo object

This approach composes files as objects and gives more flexibility to add meta information on file.


```python
from zipfile import ZipFile, ZipInfo
from io import BytesIO

def create_zip_v2():
    """
    returns: zip archive
    """
    archive = BytesIO()
    with ZipFile(archive, 'w') as zip_archive:
        # Create three files on zip archive
        file1 = ZipInfo('docker/docker-compose.yaml')
        zip_archive.writestr(file1, b'compose-file-content...')
        file2 = ZipInfo('app/app-config.json')
        zip_archive.writestr(file2, b'app-config-content...')
        file3 = ZipInfo('root-config.json')
        zip_archive.writestr(file3, b'root-config-content...')
    return archive

archive = create_zip_v2()

# Flush archive stream to a file on disk
with open('config.zip', 'wb') as f:
    f.write(archive.getbuffer())

archive.close()
```

### Use case #2: Read a file from zip archive

```python
from zipfile import ZipFile

docker_compose_config = None

with ZipFile('config.zip') as zip_archive:
    docker_compose_config = zip_archive.read('docker/docker-compose.yaml')

print(docker_compose_config)
```

### Use case #3: Update or Insert a file in zip arch

This use case is the most tricky part of the zipping business in Python. On the first look, it might look simple. Let us try attempting a few solutions.

#### Attempt #1: Don't use as it erases everything but update

```python
from zipfile import ZipFile

with ZipFile('config.zip', 'w') as zip_archive:
    zip_archive.writestr(
        'docker/docker-compose.yaml', # File to replace
        b'docker-compose-file-content-new'   # Data
    )
```

If we run the preceding script, it replaces the file in archive config.zip, but, as zipfile is opened in write mode ‘w,’ the other files/paths in archive can vanish.

> They do, I checked. Don’t use ‘w’ mode, when you update/replace a single file in a zip archive, or your data is gone for good.

#### Attempt #2: Doesn't erase old data upon update so there is extra data

```python
from zipfile import ZipFile

with ZipFile('config.zip', 'a') as zip_archive:
    zip_archive.writestr(
        'docker/docker-compose.yaml', # File to replace
        b'docker-compose-file-content-new'   # Data
    )
```

```bash
$ python -m zipfile -l config.zip
File Name                                             Modified             Size
docker/docker-compose.yaml                     1980-01-01 00:00:00           23
app/app-config.json                            1980-01-01 00:00:00           21
root-config.json                               1980-01-01 00:00:00           22
docker/docker-compose.yaml                     2020-12-11 16:41:14           31
```

So docker/docker-compose.yaml had appeared twice in the ZipInfo list but only once in extraction. For every update, the zip archive size grows and grows in the magnitude of the updated file size. If you ignore the Python warning, at some point the junk in the archive may occupy more space than actual files.

The two attempts until now couldn’t achieve an acceptable solution. Now comes third, which is a clean and elegant way.

#### Attempt #3: Works!

You have to loop through all objects and edit the one you want and if it's not the file you are looking for then copy old contents. 

> Old contents are not untouched and in fact are deleted if you edit any part of the zip!

```python
from zipfile import ZipFile, ZipInfo
from io import BytesIO


def update_or_insert(path, data):
    """
    Param: path -> file in archive
    Param: data -> data to be updated
    
    Returns a new zip file with the updated content
    for the given path
    """
    new_zip = BytesIO()
    print('here?')
    with ZipFile('config.zip', 'r') as old_archive:
        with ZipFile(new_zip, 'w') as new_archive:
            for item in old_archive.filelist:
                # If you spot an existing file, create a new object
                if item.filename == path:
                    zip_inf = ZipInfo(path)
                    new_archive.writestr(zip_inf, data)
                else:
                    # Copy other contents as it is
                    new_archive.writestr(item, old_archive.read(item.filename))
    return new_zip

new_zip = update_or_insert(
    'docker/docker-compose.yaml',
    b'docker-compose-file-content-new'
)

# Flush new zip to disk
with open('config.zip', 'wb') as f:
    f.write(new_zip.getbuffer())

new_zip.close()
```

#### Use case #4: Remove an existing file from zip archive

The cleanest way again is to copy contents from the old archive to the new archive and skip the ```ZipInfo``` objects that match the given path.


```python
from zipfile import ZipFile, ZipInfo
from io import BytesIO


def delete(path):
    """
    Param: path -> file in archive
    Returns a new zip file after deleting path
    """
    new_zip = BytesIO()
    with ZipFile('config.zip', 'r') as old_archive:
        with ZipFile(new_zip, 'w') as new_archive:
            for item in old_archive.filelist:
                if item.filename != path:
                    # Copy everything other than path to be inserted
                    new_archive.writestr(item, old_archive.read(item.filename))
    return new_zip

new_zip = delete('docker/docker-compose.yaml')

# Flush new zip to disk
with open('config.zip', 'wb') as f:
    f.write(new_zip.getbuffer())

new_zip.close()
```