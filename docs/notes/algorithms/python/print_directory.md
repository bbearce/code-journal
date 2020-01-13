# Print Directory

**This function takes the name of a directory and prints out the paths files within that directory as well as any files contained in contained directories.**

This function is similar to os.walk. Please don't use os.walk in your answer. We are interested in your ability to work with nested structures.

```python
def print_directory_contents(sPath):
    import os                                       
    for sChild in os.listdir(sPath):
        # Everything gets turned into a new path
        # Then we see which can go deeper...               
        sChildPath = os.path.join(sPath,sChild)
        if os.path.isdir(sChildPath):
            # If we have a folder, recall this function
            print_directory_contents(sChildPath)
        else:
            # If we hit a base case then print as we have found a file
            print(sChildPath)
```