# Classes

# Basic Class Definition
Intro:  
 - Class: Blueprint  
 - Object - Instance  

```python
class Shark:
    # Basic Method definition
    def swim(self):
        print("The shark is swimming.")

    def be_awesome(self):
        print("The shark is being awesome.")
```

Notice the use of self to reference an instance specifically...the one calling the method.

Implementing

```python
sammy = Shark()
sammy.swim() # >>> The shark is swimming.
sammy.be_awesome() # >>> The shark is being awesome.
```

Now let's dicuss __init__.
```python
class Shark:
    def __init__(self):
        print("This is the constructor method.")

>>> Shark()

This is the constructor method.
<__main__.Shark object at 0x10348d470>
```

Finally, we can set the name of the Shark object sammy as equal to "Sammy" by passing it as a parameter of the Shark class:

```python
class Shark:
    def __init__(self, name):
        self.name = name

    def swim(self):
        print(self.name + " is swimming.")

    def be_awesome(self):
        print(self.name + " is being awesome.")


def main():
    # Set name of Shark object
    sammy = Shark("Sammy")
    sammy.swim()
    sammy.be_awesome()

if __name__ == "__main__":
    main()
```

run...

```python
$ python shark.py
Sammy is swimming.
Sammy is being awesome.
```

# Inheritance

**super() and inheritance.**

In this tutorial, you’ll learn about the following:

 - The concept of inheritance in Python  
 - Multiple inheritance in Python  
 - How the super() function works  
 - How the super() function in single inheritance works  
 - How the super() function in multiple inheritance works  


Let's start with a simple example:

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

    def perimeter(self):
        return 2 * self.length + 2 * self.width

class Square:
    def __init__(self, length):
        self.length = length

    def area(self):
        return self.length * self.length

    def perimeter(self):
        return 4 * self.length

>>> square = Square(4)
>>> square.area()
16
>>> rectangle = Rectangle(2,4)
>>> rectangle.area()
8
```

Here no references to inheritance are being made.

**super() in Single Inheritance**

super() gives you access to methods in a superclass from the subclass that inherits from it. super() alone returns a temporary object of the superclass that then allows you to call that superclass’s methods.

By using inheritance, you can reduce the amount of code you write while simultaneously reflecting the real-world relationship between rectangles and squares:

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

    def perimeter(self):
        return 2 * self.length + 2 * self.width

# Here we declare that the Square class inherits from the Rectangle class
class Square(Rectangle):
    def __init__(self, length):
        super().__init__(length, length)

>>> square = Square(4)
>>> square.area()
16
```

What Can super() Do for You?
Like in other object-oriented languages, it allows you to call methods of the superclass in your subclass. The primary use case of this is to extend the functionality of the inherited method.

In the example below, you will create a class Cube that inherits from Square and extends the functionality of .area() (inherited from the Rectangle class through Square) to calculate the surface area and volume of a Cube instance:

```python
class Square(Rectangle):
    def __init__(self, length):
        super().__init__(length, length)

class Cube(Square):
    def surface_area(self):
        face_area = super().area()
        return face_area * 6

    def volume(self):
        face_area = super().area()
        return face_area * self.length

>>> cube = Cube(3)
>>> cube.surface_area()
54
>>> cube.volume()
27
```

Here you have implemented two methods for the Cube class: .surface_area() and .volume(). Both of these calculations rely on calculating the area of a single face, so rather than reimplementing the area calculation, you use super() to extend the area calculation.

Also notice that the Cube class definition does not have an .__init__(). Because Cube inherits from Square and .__init__() doesn’t really do anything differently for Cube than it already does for Square, you can skip defining it, and the .__init__() of the superclass (Square) will be called automatically.

**A super() Deep Dive**

While the examples above (and below) call super() without any parameters, super() can also take two parameters: the first is the subclass, and the second parameter is an object that is an instance of that subclass.

First, let’s see two examples showing what manipulating the first variable can do, using the classes already shown:

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

    def perimeter(self):
        return 2 * self.length + 2 * self.width

class Square(Rectangle):
    def __init__(self, length):
        super(Square, self).__init__(length, length)
```

In Python 3, the super(Square, self) call is equivalent to the parameterless super() call. The first parameter refers to the subclass Square, while the second parameter refers to a Square object which, in this case, is self. You can call super() with other classes as well:
```python
class Cube(Square):
    def surface_area(self):
        face_area = super(Square, self).area()
        return face_area * 6

    def volume(self):
        face_area = super(Square, self).area()
        return face_area * self.length
```

In this example, you are setting Square as the subclass argument to super(), instead of Cube. This causes super() to start searching for a matching method (in this case, .area()) at one level above Square in the instance hierarchy, in this case Rectangle.

In this specific example, the behavior doesn’t change. But imagine that Square also implemented an .area() function that you wanted to make sure Cube did not use. Calling super() in this way allows you to do that.

**What about the second parameter?**

Remember, this is an object that is an instance of the class used as the first parameter. For an example, isinstance(Cube, Square) must return True.

By including an instantiated object, super() returns a bound method: a method that is bound to the object, which gives the method the object’s context such as any instance attributes. If this parameter is not included, the method returned is just a function, unassociated with an object’s context.

For more information about bound methods, unbound methods, and functions, read the Python documentation on its [descriptor system](https://docs.python.org/3.7/howto/descriptor.html).

# Multiple Inheritance and Composition

**super() in Multiple Inheritance**

Now that you’ve worked through an overview and some examples of super() and single inheritance, you will be introduced to an overview and some examples that will demonstrate how multiple inheritance works and how ```super()``` enables that functionality.

**Multiple Inheritance Overview**

There is another use case in which ```super()``` really shines, and this one isn’t as common as the single inheritance scenario. In addition to single inheritance, Python supports multiple inheritance, in which a subclass can inherit from multiple superclasses that don’t necessarily inherit from each other (also known as sibling classes).

```bash
Superclass 1              Superclass 2
     |                         |
     |                         |
     |                         |
     |                         |
     |                         |
     ------>  Subclass <-------
```

Let's get reacquainted with our base code:

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

    def perimeter(self):
        return 2 * self.length + 2 * self.width

class Square(Rectangle):
    def __init__(self, length):
        super(Square, self).__init__(length, length)
```

Now let's proceed...

To better illustrate multiple inheritance in action, here is some code for you to try out, showing how you can build a right pyramid (a pyramid with a square base) out of a Triangle and a Square:

```python
class Triangle:
    def __init__(self, base, height):
        self.base = base
        self.height = height

    def area(self):
        return 0.5 * self.base * self.height

class RightPyramid(Triangle, Square):
    def __init__(self, base, slant_height):
        self.base = base
        self.slant_height = slant_height

    def area(self):
        base_area = super().area()
        perimeter = super().perimeter()
        return 0.5 * perimeter * self.slant_height + base_area
```

This example declares a Triangle class and a RightPyramid class that inherits from both Square and Triangle. You’ll see another .area() method that uses super() just like in single inheritance, with the aim of it reaching the .perimeter() and .area() methods defined all the way up in the Rectangle class.

The problem, though, is that both superclasses (Triangle and Square) define a .area(). Take a second and think about what might happen when you call .area() on RightPyramid, and then try calling it like below:

```python
>> pyramid = RightPyramid(2, 4)
>> pyramid.area()
Traceback (most recent call last):
  File "shapes.py", line 63, in 
    print(pyramid.area())
  File "shapes.py", line 47, in area
    base_area = super().area()
  File "shapes.py", line 38, in area
    return 0.5 * self.base * self.height
AttributeError: 'RightPyramid' object has no attribute 'height'
```

Did you guess that Python will try to call Triangle.area()? This is because of something called the method resolution order.

**Method Resolution Order**

The method resolution order (or MRO) tells Python how to search for inherited methods. This comes in handy when you’re using super() because the MRO tells you exactly where Python will look for a method you’re calling with super() and in what order.

Every class has an .__mro__ attribute that allows us to inspect the order, so let’s do that:

```python
>>> RightPyramid.__mro__
(<class '__main__.RightPyramid'>, <class '__main__.Triangle'>, 
 <class '__main__.Square'>, <class '__main__.Rectangle'>, 
 <class 'object'>)
```

This tells us that methods will be searched first in Rightpyramid, then in Triangle, then in Square, then Rectangle, and then, if nothing is found, in object, from which all classes originate.

The problem here is that the interpreter is searching for .area() in Triangle before Square and Rectangle, and upon finding .area() in Triangle, Python calls it instead of the one you want. Because Triangle.area() expects there to be a .height and a .base attribute, Python throws an AttributeError.

The problem here is that the interpreter is searching for .area() in Triangle before Square and Rectangle, and upon finding .area() in Triangle, Python calls it instead of the one you want. Because Triangle.area() expects there to be a .height and a .base attribute, Python throws an AttributeError.

Luckily, you have some control over how the MRO is constructed. Just by changing the signature of the RightPyramid class, you can search in the order you want, and the methods will resolve correctly:

```python
class RightPyramid(Square, Triangle):
    def __init__(self, base, slant_height):
        self.base = base
        self.slant_height = slant_height
        super().__init__(self.base)

    def area(self):
        base_area = super().area()
        perimeter = super().perimeter()
        return 0.5 * perimeter * self.slant_height + base_area
```

Notice that RightPyramid initializes partially with the .__init__() from the Square class. This allows .area() to use the .length on the object, as is designed.

Now, you can build a pyramid, inspect the MRO, and calculate the surface area:

```python
>>> pyramid = RightPyramid(2, 4)
>>> RightPyramid.__mro__
(<class '__main__.RightPyramid'>, <class '__main__.Square'>, 
<class '__main__.Rectangle'>, <class '__main__.Triangle'>, 
<class 'object'>)
>>> pyramid.area()
20.0
```

You see that the MRO is now what you’d expect, and you can inspect the area of the pyramid as well, thanks to .area() and .perimeter().

There’s still a problem here, though. For the sake of simplicity, I did a few things wrong in this example: the first, and arguably most importantly, was that I had two separate classes with the same method name and signature.

This causes issues with method resolution, because the first instance of .area() that is encountered in the MRO list will be called.

When you’re using super() with multiple inheritance, it’s imperative to design your classes to cooperate. Part of this is ensuring that your methods are unique so that they get resolved in the MRO, by making sure method signatures are unique—whether by using method names or method parameters.

In this case, to avoid a complete overhaul of your code, you can rename the Triangle class’s .area() method to .tri_area(). This way, the area methods can continue using class properties rather than taking external parameters:

```python
class Triangle:
    def __init__(self, base, height):
        self.base = base
        self.height = height
        super().__init__()

    def tri_area(self):
        return 0.5 * self.base * self.height
```

Let’s also go ahead and use this in the RightPyramid class:

```python
class RightPyramid(Square, Triangle):
    def __init__(self, base, slant_height):
        self.base = base
        self.slant_height = slant_height
        super().__init__(self.base)

    def area(self):
        base_area = super().area()
        perimeter = super().perimeter()
        return 0.5 * perimeter * self.slant_height + base_area

    def area_2(self):
        base_area = super().area()
        triangle_area = super().tri_area()
        return triangle_area * 4 + base_area
```

The next issue here is that the code doesn’t have a delegated Triangle object like it does for a Square object, so calling .area_2() will give us an AttributeError since .base and .height don’t have any values.

You need to do two things to fix this:

1. All methods that are called with super() need to have a call to their superclass’s version of that method. This means that you will need to add super().__init__() to the .__init__() methods of Triangle and Rectangle.  

2. Redesign all the .__init__() calls to take a keyword dictionary. See the complete code below.  

```python
class Rectangle:
    def __init__(self, length, width, **kwargs):
        self.length = length
        self.width = width
        super().__init__(**kwargs)

    def area(self):
        return self.length * self.width

    def perimeter(self):
        return 2 * self.length + 2 * self.width

# Here we declare that the Square class inherits from 
# the Rectangle class
class Square(Rectangle):
    def __init__(self, length, **kwargs):
        super().__init__(length=length, width=length, **kwargs)

class Cube(Square):
    def surface_area(self):
        face_area = super().area()
        return face_area * 6

    def volume(self):
        face_area = super().area()
        return face_area * self.length

class Triangle:
    def __init__(self, base, height, **kwargs):
        self.base = base
        self.height = height
        super().__init__(**kwargs)

    def tri_area(self):
        return 0.5 * self.base * self.height

class RightPyramid(Square, Triangle):
    def __init__(self, base, slant_height, **kwargs):
        self.base = base
        self.slant_height = slant_height
        kwargs["height"] = slant_height
        kwargs["length"] = base
        super().__init__(base=base, **kwargs)

    def area(self):
        base_area = super().area()
        perimeter = super().perimeter()
        return 0.5 * perimeter * self.slant_height + base_area

    def area_2(self):
        base_area = super().area()
        triangle_area = super().tri_area()
        return triangle_area * 4 + base_area
```

There are a number of important differences in this code:

- kwargs is modified in some places (such as RightPyramid.__init__()): This will allow users of these objects to instantiate them only with the arguments that make sense for that particular object.  

- Setting up named arguments before **kwargs: You can see this in RightPyramid.__init__(). This has the neat effect of popping that key right out of the **kwargs dictionary, so that by the time that it ends up at the end of the MRO in the object class, **kwargs is empty.  

Now, when you use these updated classes, you have this:

```python
>>> pyramid = RightPyramid(base=2, slant_height=4)
>>> pyramid.area()
20.0
>>> pyramid.area_2()
20.0
```

It works! You’ve used super() to successfully navigate a complicated class hierarchy while using both inheritance and composition to create new classes with minimal reimplementation.

Quiz!!! When Rectangle calls super() what are the values of kwargs and what class gets it's __init__ method called

Answer:

 - kwargs is still containing ```{'base': 2, 'height': 4}``` values as they haven't been removed from kwargs  

- ```super().__init__(**kwargs)``` calls Triangle's __init__, but Rectangle doesn't inherit from Triangle so why?  

Remember the MRO? Let's look at RightPyramid's MRO

```python
>>> RightPyramid.__mro__
(<class '__main__.RightPyramid'>, <class '__main__.Square'>, <class '__main__.Rectangle'>, <class '__main__.Triangle'>, <class 'object'>)
```

Even though Rectangle doesn't inherit from Triangle, Triangle is next in the list of classes to search for an __init__ method. You'll notice object is at the end of the list.

```python
super().__init__(**kwargs)
```

Multiple Inheritance Alternatives
As you can see, multiple inheritance can be useful but also lead to very complicated situations and code that is hard to read. It’s also rare to have objects that neatly inherit everything from more than multiple other objects.

If you see yourself beginning to use multiple inheritance and a complicated class hierarchy, it’s worth asking yourself if you can achieve code that is cleaner and easier to understand by using composition instead of inheritance.

With composition, you can add very specific functionality to your classes from a specialized, simple class called a mixin.

Since this article is focused on inheritance, I won’t go into too much detail on composition and how to wield it in Python, but here’s a short example using VolumeMixin to give specific functionality to our 3D objects—in this case, a volume calculation:

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

class Square(Rectangle):
    def __init__(self, length):
        super().__init__(length, length)

class VolumeMixin:
    def volume(self):
        return self.area() * self.height

class Cube(VolumeMixin, Square):
    def __init__(self, length):
        super().__init__(length)
        self.height = length

    def face_area(self):
        return super().area()

    def surface_area(self):
        return super().area() * 6
```

In this example, the code was reworked to include a mixin called VolumeMixin. The mixin is then used by Cube and gives Cube the ability to calculate its volume, which is shown below:

```python
>>> cube = Cube(2)
>>> cube.surface_area()
24
>>> cube.volume()
8
```

**A super() Recap**

In this tutorial, you learned how to supercharge your classes with super(). Your journey started with a review of single inheritance and then showed how to call superclass methods easily with super().

You then learned how multiple inheritance works in Python, and techniques to combine super() with multiple inheritance. You also learned about how Python resolves method calls using the method resolution order (MRO), as well as how to inspect and modify the MRO to ensure appropriate methods are called at appropriate times.

For more information about object-oriented programming in Python and using super(), check out these resources:

[Official super() documentation](https://docs.python.org/3/library/functions.html#super)  
[Python’s super() Considered Super by Raymond Hettinger](Python’s super() Considered Super by Raymond Hettinger)  
[Object-Oriented Programming in Python 3](Object-Oriented Programming in Python 3)  