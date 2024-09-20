# Closures

```bash
pyenv activate venv3.10.4
```

Notice below when pdb.set_trace() is called, var will not be available.
```python
import pdb

def create_closure(var):
    def closure():
        pdb.set_trace()
        print(f"test")
    return closure

# Create a closure
my_closure = create_closure("Hello, World!")


# Print the source code
print(my_closure())
```

Notice below when pdb.set_trace() is called, var will be available.
```python
import pdb

def create_closure(var):
    def closure():
        pdb.set_trace()
        print(f"test {var}")
    return closure

# Create a closure
my_closure = create_closure("Hello, World!")


# Print the source code
print(my_closure())
```

The reason is that the variable `var` is not defined in the local scope of the closure. When pdb.set_trace() is called, it will not be able to find the variable `var`.