# Counter

Counter is a dict subclass for counting hashable objects:

```python
>>> c = Counter() # a new, empty counter
>>> c
Counter()
>>> c = Counter('gallahad') # a new counter from an iterable
>>> c
Counter({'a': 3, 'l': 2, 'g': 1, 'h': 1, 'd': 1}) # a new counter from a mapping
>>> c = Counter({'red': 4, 'blue': 2}) # a new counter from a mapping
>>> c
Counter({'red': 4, 'blue': 2}) 
>>> c = Counter(cats=4, dogs=8) # a new counter from keyword args  
>>> c
Counter({'dogs': 8, 'cats': 4}) 
>>> Counter([1,2,2,3,3,3,4,4,4,4])
Counter({4: 4, 3: 3, 2: 2, 1: 1})
```

## Delete objects as shown below:

```python
# Delete records as shown below:
>>> c = Counter(['eggs', 'ham'])
>>> c
Counter({'eggs': 1, 'ham': 1})
>>> del c['ham']   
>>> c
Counter({'eggs': 1})
```

Counter objects support three methods beyond those available for all dictionaries:

## elements()

Return an iterator over elements repeating each as many times as its count. Elements are returned in arbitrary order. If an element’s count is less than one, elements() will ignore it.

```python
>>> c = Counter(a=4, b=2, c=0, d=-2)
>>> list(c.elements())

['a', 'a', 'a', 'a', 'b', 'b']
```

## most_common([n])

Return a list of the n most common elements and their counts from the most common to the least. If n is omitted or None, most_common() returns all elements in the counter. Elements with equal counts are ordered arbitrarily:

```python
>>> Counter('abracadabra').most_common(3)
[('a', 5), ('r', 2), ('b', 2)]
```

## subtract([iterable-or-mapping])

Elements are subtracted from an iterable or from another mapping (or counter). Like dict.update() but subtracts counts instead of replacing them. Both inputs and outputs may be zero or negative.

```python
>>> c = Counter(a=4, b=2, c=0, d=-2)
>>> d = Counter(a=1, b=2, c=3, d=4)
>>> c.subtract(d)
>>> c
Counter({'a': 3, 'b': 0, 'c': -3, 'd': -6})
```