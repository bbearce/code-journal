# Directory and File Compare

>Source [stackoverflow](https://stackoverflow.com/questions/19251993/comparing-two-directories-with-subdirectories-to-find-any-changes)


### List out files appended to their paths:

```python
import os

def listfiles(path):
    files = []
    for dirName, subdirList, fileList in os.walk(path):
        dir = dirName.replace(path, '')
        for fname in fileList:
            files.append(os.path.join(dir, fname))
    return files

x = listfiles('D:\\xfiles')
y = listfiles('D:\\yfiles')
```

### You can use list comprehensions to compare, but it's slow compared to *sets*:

```python
q = [filename for filename in x if filename not in y]
```

### Use *sets* in python to make compares:

```python
files_only_in_x = set(x) - set(y) 
files_only_in_y = set(y) - set(x)
files_only_in_either = set(x) ^ set(y)
files_in_both = set(x) & set(y)
all_files = set(x) | set(y)
```