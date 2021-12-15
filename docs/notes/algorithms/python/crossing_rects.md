# Crossing Rects

**Find the total overlapping area without double counting.**

**Problem:**

Imagine a grid with rectangles on it.

Find the total overlapping area that does not repeat. In other words if a grid square is overlapped by multiple rectangles, it will only contribute 1 unit of area to the total overlapping area.

Example 1:
```
__________________________________> (x)   
|_| | | A (0,0), (1,1)    
|___| |   B (0,0), (2,2)     
|_____|     C (0,0), (3,3)     
|        
|       
\/

(y)

The answer is: 4
```

Example 2:
```
Key: *(x,y), +(w, l)
* - Top Left Corner
+ - Width and Length

A (0,0), (9,3)
_________________________________> (x)   
|      __|____ B (6,1), (8,4)
|     |  |   _| 
|_____|__|__|_| C (13,2), (1,3)
|     |     | | 
|     |_____|_|
\/

(y)
```
The answer is: 9

Here's the anser:


```python
def overlapping_area(rects=[ [(0,0),(1,1)],[(0,0),(2,2)] ]):
    """
    This function takes a list of lists where each inner
    list is a rectangle. Each list will contain two tuples representing
    the top left corner of each rectangle and it's x-width and y-length.
    
    # Example:
    rect = [(x,y),(w,l)]

    """

    def get_coordinates(rect=[(0,0), (0,0)]):
        """
        Takes a list of tuples defining the top left corner 
        coordinates of each square and the x-width, y-length
        and return all coordinates of rectangle. 

        Below is the easy to see to setup the for loop, 
        that way seeingwhat the comprehension is doing
        is much easier. This generates all x,y coordinates of
        a rect.

        coor = []
        for i in range(rect[1][0]):
            for j in range(rect[1][1]):
                coor.append((rect[0][0]+i,rect[0][1]+j))
        """

        # List comprehension (list comprehensions are ordered backwards):
        coor = [(rect[0][0]+i,rect[0][1]+j) for j in range(rect[1][1]) for i in range(rect[1][0])]
        return coor

    # Get all rects coordinates
    rects_coors = [get_coordinates(r) for r in rects]

    area = 0
    # Number of rectangles
    n = len(rects)
    seen = set()
    while n > 1:
        # Find overlap
        coors_in_common = set(rects_coors[n-1]) & set(rects_coors[n-2])
        # Subtract out any previous overlap squares
        new_coors = coors_in_common - seen 
        # Add fresh squares to area
        area += len(new_coors)
        # Mark seen area
        seen = set(rects_coors[n-1]) & set(rects_coors[n-2])
        n -= 1

    print("area of overlap is: {}".format(area))

# Default
overlapping_area()
>>> area of overlap is: 1

# Trial 1
rects = [[(0,0), (1,1)],
         [(0,0), (2,2)],
         [(0,0), (3,3)]] 

overlapping_area(rects)
>>> area of overlap is: 1

# Trial 2
rects = [[(0,0), (9,3)], 
         [(6,1), (8,4)],
         [(13,2),(1,3)]] 

overlapping_area(rects)
>>> area of overlap is: 9
```