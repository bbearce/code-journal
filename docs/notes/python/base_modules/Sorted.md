# Sorted

Sort Arrays

Want to sort something, **sorted()** is a great start.

**Syntax:** sorted(iterable, key, reverse)

**Parameters:** sorted takes three parameters from which two are optional.

 - Iterable: sequence (list, tuple, string) or collection (dictionary, set, frozenset) or any other iterator that needs to be sorted  
 - Key(optional) : A function that would server as a key or a basis of sort comparison  
 - Reverse(optional) : If set true, then the iterable would be sorted in reverse (descending) order, by default it is set as false  

**Note:** A list also has sort() method which performs the same way as sorted(). Only difference being, sort() method doesn't return any value and changes the original list itself.

```python
x = [2,44,3,87,5]
print(x) # [2, 44, 3, 87, 5]
print(sorted(x)) #[2, 3, 5, 44, 87]
print(sorted(x, reverse=True)) # [87, 44, 5, 3, 2]
```

Custom Sorting using the key parameter:
sorted() function has an optional parameter called ‘key’ which takes a function as its value. This key function transforms each element before sorting, it takes the value and returns 1 value which is then used within sort instead of the original value.

For example, if we pass a list of strings in sorted(), it gets sorted alphabetically . But if we specify key = len, i.e. give len function as key, then the strings would be passed to len, and the value it returns, i.e. the length of strings will be sorted. Which means that the strings would be sorted based on their lengths instead

```python
# sort by your own criteria
L = ["cccc", "b", "dd", "aaa"] 
print("Normal sort :", sorted(L))
print("Sort with len :", sorted(L, key = len)) 
```