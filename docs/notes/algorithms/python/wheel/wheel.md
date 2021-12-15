# Make a Spinning Wheel

To start we need picture like states. We can make these with strings.

> Import what you need
```python
import os
import sys
import time
import math
```

```python
state_1 = """
                    -|-
                     |
      /              |              \\
                     |
                     |
                     |
                     |
                     |
|                    |                    |
                     |
                     |
                     |
                     |
     \\              |             /
                     |
                     |
                    -|-
"""
```
```python
state_2 = """
              
                           /
                          /
                         /
                        /
                       /
                      /
                     /
                    /
                   /
                  /
                 /
                /
               /
              /

""" 
```
```python
state_3 = """



     

                                                            
                              ---------------
               ---------------
---------------







"""
```
```python
state_4 = """
                    -

                                         
      /                            \\
                     
                     
                     
                     
                     
----------------------------------------
                     
                     
                     
                     
     \\                           /
                     
                     
                     
                     -
"""
```
```python
state_5 = """






---------------
               ---------------
                              ---------------







"""
```
```python
state_6 = """

             \\
              \\
               \\
                \\
                 \\
                  \\
                   \\
                    \\
                     \\
                      \\
                       \\
                        \\
                         \\
                          \\

""" 
```

Now define a generator to access the wheel states infintely:
```python
def spinning_cursor():
    while True:
        for cursor in [state_1,state_2,state_3,state_4,state_5,state_6]:
            yield cursor

```

Next wheel size and set placeholders to keep track of the *elapsed time* and *velocity*:
```python
radius = 1 # ft

spinner = spinning_cursor()
elapsed_time = 0
velocity = 0
```

Choose the speed in terms of rotations per second:
```python
rotations_per_second = 3
rotations_desired = 2
```

"DJ spin that shit!"
- KS -

```python
for _ in range(6*rotations_desired): 
    # there are 6 states and we need all of them per rotation
    state = next(spinner) # call the next state
    if state == state_1 and elapsed_time != 0:
        # Once we've done a full rotation we should have enough info to calculate the velocity
        velocity = 2*math.pi*radius / elapsed_time # ft/s 
    
    # In 1 second we do 6 states * rotations_per_second.
    # We can divide 1 second into these partitions to find out 
    # how much time has passed between states.
    elapsed_time += 1.0 / (6*rotations_per_second) # seconds


    sys.stdout.write(state) # see the state
    sys.stdout.flush() # clear screen
    time.sleep(1.0/(6*rotations_per_second)) # control the rate of execution
    os.system('clear') # clear the screen of any states
    
os.system('clear') # clear the screen of any states 

print("velocity: {} in miles/hour".format(velocity/5280*60*60))
# velocity is currently in ft/s.

# 5280 ft in a mile 
# 60 seconds in a min
# 60 min in an hour

```

<video width="400" controls>
  <source src="/notes/algorithms/python/wheel/wheel.mp4" type="video/mp4">
  Your browser does not support HTML5 video.
</video>