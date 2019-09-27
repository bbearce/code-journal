# Stairs

**How many ways can you traverse n stairs if you can only take 1 or 2 steps.**

This is a cool little chunck of code illustrating and intersting way recurtsion solves a tough problem.
```
_
 |_
   |_
     |...
```

how many ways can you traverse n stairs if you can
only take 1 or 2 steps.

Example: n = 1
(1)
answer = 1

Example: n = 2
(1,1), (2)
answer = 2

Example: n = 3
(1,1,1), (1,2), (2,1)
answer = 3

Example: n = 4
(1,1,1,1), (1,1,2), (1,2,1), (2,1,1), (2,2)
answer = 5

Example: n = 5
(1,1,1,1,1), (1,1,1,2), (1,1,2,1), (1,2,1,1), (2,1,1,1), (1,2,2), (2,1,2), (2,2,1)
answer = 8

Pattern:

n|stairs_combo(n)
-|-----
1|1
2|2
3|3
4|5
5|8

What do we notice?

Answer Pattern:

n|stairs_combo(n)
-|-------------------------------------------------
1|1 = pattern doesn't hold
2|2 = pattern doesn't hold
3|3 = stairs_combo(2) + stairs_combo(1) = 2 + 1 = 3
4|5 = stairs_combo(3) + stairs_combo(2) = 3 + 2 = 5
5|8 = stairs_combo(4) + stairs_combo(3) = 5 + 3 = 8

We can use recursion on this since the answer depends on answers before it. 
For the first two values n can have (1,2) we will setup base cases to kill 
the recursive stack.

Here is the answer:
```python
def stairs_combo(n=0):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    elif n == 2:
        return 2
    else:
        return stairs_combo(n-1) + stairs_combo(n-2)

print(stairs_combo())  # 0
print(stairs_combo(1)) # 1
print(stairs_combo(2)) # 2
print(stairs_combo(3)) # 3
print(stairs_combo(4)) # 5
print(stairs_combo(5)) # 8
```
The other solution is to figure out the combinations for real! ;)

