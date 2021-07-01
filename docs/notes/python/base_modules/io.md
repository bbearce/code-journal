# IO

## Reading \ Writing
```python
with open('test.txt', "w") as file:
    file.write("Bam")
```

> Source: [w3schools](https://www.w3schools.com/python/python_file_write.asp)
```python
f = open("demofile2.txt", "a")
f.write("Now the file has more content!")
f.close()

#open and read the file after the appending:
f = open("demofile2.txt", "r")
print(f.read())
```

```python
f = open("demofile3.txt", "w")
f.write("Woops! I have deleted the content!")
f.close()

#open and read the file after the appending:
f = open("demofile3.txt", "r")
print(f.read())
```

> Source: [geeksforgeeks](https://www.geeksforgeeks.org/python-os-mkdir-method/)

## Create folder
```python
import os 
os.mkdir("./to_here")
```

## Move File
> Source: [w3schools](https://www.w3schools.com/python/python_file_write.asp)
> 
```python
import shutil

with open('from_here.txt', "w") as file:
    file.write("from here")

src="./from_here.txt"
dest="./to_here/"
shutil.move(src, dest)
```

## Delete A File
```python
import os
os.remove("test.txt")
```

## Delete A Folder
```python
import shutil, os

#if empty
os.rmdir("from_here")

#if not empty
os.rmdir("from_here") # Error if it has from_here.txt inside

# So use
shutil.rmtree("to_here")
> [shutil.rmtree](https://docs.python.org/3/library/shutil.html#shutil.rmtree)
```