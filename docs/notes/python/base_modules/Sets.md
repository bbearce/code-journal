# Sets

Sets In Python

A Set is an unordered collection data type that is iterable, mutable, and has no duplicate elements. Python’s set class represents the mathematical notion of a set. The major advantage of using a set, as opposed to a list, is that it has a highly optimized method for checking whether a specific element is contained in the set. This is based on a data structure known as a hash table.

Methods:
## add()

1. add(x) Method: Adds the item x to set if it is not already present in the set.
```python
>>> s3.add(9)
>>> s3
{9, 3}
2. union(s) Method: Returns a union of two set.Using the ‘|’ operator between 2 sets is the same as writing set1.union(set2)
```

## union()
```python
>>> s1.union(s2)
{1, 2, 3, 4, 5}

>>> s1 | s2
{1, 2, 3, 4, 5}
3. intersect(s) Method: Returns an intersection of two sets.The ‘&’ operator comes can also be used in this case.
```

## intersection()
```python
>>> s1.intersection(s2)
{3}

>>> s1 & s2
{3}
```

## difference
4. difference(s) Method: Returns a set containing all the elements of invoking set but not of the second set. We can use ‘-‘ operator here.
```python
>>> s1.difference(s2)
{1, 2}

>>> s1 - s2
{1, 2}
5. clear() Method: Empties the whole set.

>>> s1.clear()
>>> s2.clear()

>>> s1
set()
>>> s2
set()
```

## discard()
6. discard() Method: The discard() method takes a single element x and removes it from the set (if present).
```python
>>> s1
{1, 2, 3}
>>> s1.discard(1)
>>> s1
{2, 3}
```

## issubset()
7. issubset() Method: The issubset() method returns True if all elements of a set are present in another set (passed as an argument). If not, it returns False.
```python
>>> s1
{1, 2, 3}
>>> s1.issubset({1,2,3,4})
True
Operators for Sets
Sets and frozen sets support the following operators:

>>> k = 1

>>> key in s1      # containment check
True
>>> key not in s1  # non-containment check
False
>>> s1 == s2       # s1 is equivalent to s2
False
>>> s1 != s2       # s1 is not equivalent to s2
True
>>> s1 <= s2       # s1 is subset of s2
False
>>> s1 < s2        # s1 is proper subset of s2
False
>>> s1 >= s2       # s1 is superset of s2
False
>>> s1 > s2        # s1 is proper superset of s2
False
>>> s1 | s2        # the union of s1 and s2
{1, 2, 3, 4, 5}
>>> s1 & s2        # the intersection of s1 and s2
{3}
>>> s1 - s2        # the set of elements in s1 but not s2
{1, 2}
>>> s1 ^ s2        # the set of elements in precisely one of s1 or s2
{1, 2, 4, 5}
```