# Map

map() allows us to apply a function to a list of items

```python
# Simple Example
>>> list(map(float, ['1.0', '2.0']))
[1.0, 2.0]

# Turn this:
items = [1, 2, 3, 4, 5]
squared = []
for i in items:
    squared.append(i**2)
    
# Into this with a lambda function:
items = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, items))
```