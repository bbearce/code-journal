# Pass By Reference

Python will pass values between objects as references to their memory position. Addresses not values are passed...except for primitives.. An example:
```python
def f(x,l=[]):
    for i in range(x):
        l.append(i*i)
    print(l) 

f(2) # [0, 1]
f(3,[3,2,1]) # [3, 2, 1, 0, 1, 4]
Here python treats the variable l as a fresh list because a fresh list or more accurately, it's location in memory was passed. See python created [3, 2, 1] separately and passed it's location to the function. Watch what happends when we call the function again with no list.

f(3) # [0, 1, 0, 1, 4]
why not [0,1,4]?. When the function was first called we created the default list "l=[]", and according to the function's directions, by default this function refers to that list's memory position. Check out the following examples of pythons memory usage for this example:

l_mem = []

l = l_mem           # the first call
for i in range(2):
    l.append(i*i)

print(l)            # [0, 1]

l = [3,2,1]         # the second call
for i in range(3):
    l.append(i*i)

print(l)            # [3, 2, 1, 0, 1, 4]

l = l_mem           # the third call
for i in range(3):
    l.append(i*i)

print(l)            # [0, 1, 0, 1, 4]
```