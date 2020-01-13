# Default Dict

Initialize dictionary values with a data type.

Returns a new dictionary-like object. defaultdict is a subclass of the built-in dict class.

The first argument provides the initial value for the default_factory attribute; it defaults to ```None```. All remaining arguments are treated the same as if they were passed to the dict constructor, including keyword arguments.

```python
from collections import defaultdict

# Try to append all numbers in a list to their descriptions
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
d = {}
for k, v in s:
    d[k].append(v)

# You can't because there is no default data type for the values of the keys being created.
>>> Traceback (most recent call last):
  File "", line 1, in 
  File "", line 6, in 
KeyError: 'yellow'

# Use a default dict to accomplish this:
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
d = defaultdict(list)
for k, v in s:
    d[k].append(v)

>>> d
defaultdict(, {'yellow': [1, 3], 'blue': [2, 4], 'red': [1]})

# You can accomplish this with the base dictionary like this:
>>> d = {}
>>> d.setdefault('yellow', [])
[]
>>> d
{'yellow': []}
```

or

```python 
>>> d = {}
>>> d.setdefault('yellow', list)

>>> d
{'yellow': }

# ...and our previous example:
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
d = {}
for k, v in s:
    d.setdefault(k, []).append(v)

>>> d
{'yellow': [1, 3], 'blue': [2, 4], 'red': [1]}
```