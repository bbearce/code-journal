# Nodes with Classes

**Build a parent/child relationship with classes**

Consider the following code, what will it output?

```python
class Node(object):
    def __init__(self,sName):
        self._lChildren = []
        self.sName = sName
    def __repr__(self):
        return "<Node '{}'>".format(self.sName)
    def append(self,*args,**kwargs):
        self._lChildren.append(*args,**kwargs)
    def print_all_1(self):
        print(self)
        for oChild in self._lChildren:
            oChild.print_all_1()
    def print_all_2(self):
        def gen(o):
            lAll = [o,]
            while lAll:
                oNext = lAll.pop(0)
                lAll.extend(oNext._lChildren)
                yield oNext
        for oNode in gen(self):
            print(oNode)

oRoot = Node("root")
oChild1 = Node("child1")
oChild2 = Node("child2")
oChild3 = Node("child3")
oChild4 = Node("child4")
oChild5 = Node("child5")
oChild6 = Node("child6")
oChild7 = Node("child7")
oChild8 = Node("child8")
oChild9 = Node("child9")
oChild10 = Node("child10")

oRoot.append(oChild1)
oRoot.append(oChild2)
oRoot.append(oChild3)
oChild1.append(oChild5)
oChild2.append(oChild6)
oChild4.append(oChild7)
oChild3.append(oChild8)
oChild3.append(oChild9)
oChild6.append(oChild10)

# specify output from here onwards

oRoot.print_all_1() 
oRoot.print_all_2()
```


Let's discuss what happens. Below is the map of the parents and children:

    
         oRoot()
___________|_____________
|          |             |
oRoot1() oRoot2()     oRoot3()
|          |         ____|____
oRoot5() oRoot6()    |        |
           |       oRoot8() oRoot9()
         oRoot10()

oRoot4()
   |
oRoot7()

The append method is taking the calling object and appending the passed object to an instance variable '_lChildren'. Once the above tree is setup, it's time to investigate the two print methods.

oRoot().print_all_1()
def print_all_1(self):
    print(self)
    for oChild in self._lChildren:
        oChild.print_all_1()
The first action is to print the starting root. print(self). Then we loop through the children and call the print_all_1() method for each child. It's important to note that oChild5() is called before oChild2(). This is because before oChild2() is called oChild1() called print_all1() on it's children. This has the affect of traveling down branches as opposed to across them.

>>> oRoot.print_all_1()
<Node 'root'>
<Node 'child1'>
<Node 'child5'>
<Node 'child2'>
<Node 'child6'>
<Node 'child10'>
<Node 'child3'>
<Node 'child8'>
<Node 'child9'>
oRoot().print_all_2()
def print_all_2(self):
    def gen(o):
        lAll = [o,]
        while lAll:
            oNext = lAll.pop(0)
            lAll.extend(oNext._lChildren)
            yield oNext
    for oNode in gen(self):
        print(oNode)
First loop in the for loop:
Do you see the generator? print_all_2() actually creates a generator function. First it creates a function called gen(o) and then creates lAll, a list for storing objects. This is an interesting use of a generator; since lAll is remembered between successive yields, it can theoretically traverse an infinite tree. So the first value to gen is self. In that for loop the first yield is hit and pauses the function after yeilding oRoot(). The for loop is looping through an iterator so the iterator is telling the for loop to keep going. So finish the first for loop and print the oRoot().

The second:
So the for loop picks up and rechecks the while condition, which is true as we added oRoot()'s direct children to lAll. Now we pop oChild1() off of lAll and add it's children to lAll. Here is what lAll looks like at this point: [oChild2(), oChild3(), oChild5()]. We yield and print oChild1() and check the while again. Its true so we grab the next element off lAll which is oChild2(). Append its children and yield it and continue horizontaly and left to right across the tree. Check out the final route taken:

>>> oRoot.print_all_2()
<Node 'root'>
<Node 'child1'>
<Node 'child2'>
<Node 'child3'>
<Node 'child5'>
<Node 'child6'>
<Node 'child8'>
<Node 'child9'>
<Node 'child10'>
