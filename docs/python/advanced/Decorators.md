# Decorators

**@Add_This_Functionality_To_Any_Function!**

## Simple Example:
```python
def my_decorator(original_function):
    def new_function(*args,**kwargs):
        print("you did it!")
        of = original_function(*args,**kwargs)
        return of
    return new_function

@my_decorator
def my_func1(stuff):
    print("do things")

@my_decorator
def my_func2(stuff):
    print("do things")

@my_decorator
def my_func3(stuff):
    print("do things")


my_func1(1);my_func2(1);my_func3(1);
```

## Tutorial:
We need to discuss global variables and function closure


### [1] functions:
```python
def foo():
    return 1;
```

### [2] scope
```python
a_string = 'this is a string'
def foo():
    print(locals())
foo() # {}
print(globals()) # {..., 'a_string': 'this is a string'}
```

### [3] varible resolution rules

Local variables with the same name as global ones don't modify the global one.

### [4] variable lifetime
```python
def foo():
    x = 1;

foo() # NameError: name 'x' is not defined
```

### [5] function arguments and parameters
```python
def foo(x):
    print(locals())
foo(1) # {'x': 1}

def foo(x, y=0): # remember if no default it's mandatory
    return x - y
foo()
# Traceback (most recent call last):
#   File "", line 1, in 
# TypeError: foo() missing 1 required positional argument: 'x'
```

### [6] Nested Functions
```python
def outer():
    x = 1
    def inner():
        print(x)
    inner()
outer() # 1
```

### [7] Functions are first class objectsin python
```python
# all objects in Python inherit from a common baseclass
issubclass(int, object) # True

def foo():
    pass
foo.__class__ # <type 'function'>
issubclass(foo.__class__, object) # True

#..so what?
def add(x,y):
    return x + y
def sub(x,y):
    return x - y
def apply(func, x, y):
    return func(x, y)

apply(add, 2, 1) # 3
apply(sub, 2, 1) # 1

#.. closure lead in

def outer():
    def inner():
        print('this is inner')

    return inner # the function not what it returns

foo = outer()
foo # .inner at 0x10be011e0>
foo() # 'this is inner'
```

### [8] Closures
```python
def outer():
    x = 1
    def inner():
        print(x)
    return inner # the function not what it returns
foo = outer()
foo.__closure__ # (,)

# aside: cell objects are used to store the free variables of a closure
# without closures x would have not existed as after the call to outer x is gone based on variable life time.
# inner functions defined in non-global scope remember what their enclosing namespaces looked like at definition time.
foo() # 1

# let's tweak it
def outer(x):
    def inner():
        print(x)
    return inner 

print1 = outer(1)
print2 = outer(2)

print1() # 1
print2() # 2
```

### [9] Decorators
```python
def outer(some_func):
    def inner():
        print('before some_func')
        ret = some_func() # 1
        return ret + 1
    return inner

def foo():
    return 1

decorated = outer(foo)
decorated() # we've added functionality to foo()!

# what if we did this
foo = outer(foo)
foo()
# before some_func
# 2

# Let's write a more useful decorator
class Coordinate(object):
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def __repr__(self):
            return "Coord: " + str(self.__dict__)

def add(a, b):
    return Coordinate(a.x + b.x, a.y + b.y)
def sub(a, b):
    return Coordinate(a.x - b.x, a.y - b.y)

one = Coordinate(100, 200)
two = Coordinate(300, 200)
add(one, two) # Coord: {'y': 400, 'x': 400}

# add this instance
three = Coordinate(-100, -100)

def wrapper(func):
    def checker(a, b): # 1
        if a.x < 0 or a.y < 0:
            a = Coordinate(a.x if a.x > 0 else 0, a.y if a.y > 0 else 0)
        if b.x < 0 or b.y < 0:
            b = Coordinate(b.x if b.x > 0 else 0, b.y if b.y > 0 else 0)
        ret = func(a, b)
        if ret.x < 0 or ret.y < 0:
            ret = Coordinate(ret.x if ret.x > 0 else 0, ret.y if ret.y > 0 else 0)
        return ret
    return checker

add = wrapper(add)
sub = wrapper(sub)

sub(one, two)
add(one, three)
```

### [10] the @ symbol
```python
# so instead of wrapper(add)\wrapper(sub), use @wrapper
@wrapper
def add(a, b):
    return Coordinate(a.x + b.x, a.y + b.y)

@wrapper
def sub(a, b):
    return Coordinate(a.x - b.x, a.y - b.y)
```

### [11] \*args and \*\*kwargs
```python
def one(*args):
    print(args)

one() # ()
one(1,2,3) # (1, 2, 3)

def two(x, y, *args):
    print(x, y, args)

two('a','b','c')

# Reminder
# l = (1,2,3)
# one(l[0], l[1], l[2]) # (1, 2, 3)
# one(*l) # (1, 2, 3)

def foo(**kwargs):
    print(kwargs)

foo()
foo(x=1, y=2)

```

### [12] More generic decorators
```python
def logger(func):
    def inner(*args, **kwargs):
        print('Arguments were: {}, {}'.format(args,kwargs))
        return func(*args, **kwargs)
    return inner

@logger
def foo1(x,y=1):
    return x * y

@logger
def foo2():
    return 2

foo1(5,4)
# Arguments were: (5, 4), {}
# 20
foo2()
# Arguments were: (), {}
# 2
```