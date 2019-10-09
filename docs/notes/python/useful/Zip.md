# Zip

Combine iterables and return them as tuple sets.

The zip() function returns an iterator of tuples based on the iterable object.

 - If no parameters are passed, zip() returns an empty iterator  
 - If a single iterable is passed, zip() returns an iterator of 1-tuples. Meaning, the number of elements in each tuple is 1  
 - If multiple iterables are passed, ith tuple contains ith iterable values from all iterables. Suppose, two iterables are passed; one iterable containing 3 and other containing 5 elements, then the returned iterator will have 3 tuples  

```python
>>> zip()
<zip object at 0x102c9be48>

# length 0
>>> list(zip())
[]

# length 1
>>> list(zip([1,2,3]))
[(1,), (2,), (3,)]

# same length iterables
>>> x, y = [1,2,3], [4,5,6]
>>> list(zip(x,y))
[(1, 4), (2, 5), (3, 6)]

# different length iterables
>>> x, y = [1,2,3], [4,5,6,7]
>>> list(zip(x,y))
[(1, 4), (2, 5), (3, 6)]
```

iterables - can be built-in iterables (like: list, set, tuple, string, dict...), or user-defined iterables (object that has __iter__ method).

```python
x, y, z = [1,2,3], [4,5,6], {'a':4,'b':5,'c':6}

# dictionaries use keys by default
results_default = set(zip(x,y,z))

results_2 = set(zip(x,y,z.keys()))
results_3 = set(zip(x,y,z.values()))

print(results_default) # {(3, 6, 'c'), (1, 4, 'a'), (2, 5, 'b')}
print(results_2) # {(3, 6, 'c'), (1, 4, 'a'), (2, 5, 'b')}
print(results_3) # {(1, 4, 4), (3, 6, 6), (2, 5, 5)}
```

View the zip contents with a list or set or tuple data type:

```python
>>> list(zip(x,y))
[(1, 4), (2, 5), (3, 6)]
>>> set(zip(x,y))
{(2, 5), (3, 6), (1, 4)}
>>> tuple(zip(x,y))
((1, 4), (2, 5), (3, 6))
```

Unzipping is possible too:
```python
> x
[1, 2, 3]
>>> y
[4, 5, 6, 7]
>>> list(zip(x,y))
[(1, 4), (2, 5), (3, 6)]

# Use * to unzip a zip object. You have to call zip() as a wrapper for *zip().
>>> list(zip(*zip(x,y)))
[(1, 2, 3), (4, 5, 6)]
>>> zip(*zip(x,y))
<zip object at 0x102ccb888>

# The zip object is automatically unpacked into a nd b.
>>> a,b = zip(*zip(x,y))
>>> a
(1, 2, 3)
>>> b
(4, 5, 6)
>>> 
```

Watch what happens when you try to view a zip object with a dictionary:
```python
>>> dict(zip([1,2],['a','b']))
{1: 'a', 2: 'b'}
```
We can create dictionaries from individual unassociated key, values lists!