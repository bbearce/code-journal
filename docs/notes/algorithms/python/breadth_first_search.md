# Breadth First Search

Solve a maze!

In the breadth first search we start by imagining a grid.

```python
grid = ["..........",
        "...#...##.",
        "..##...#..",
        ".....###..",
        "......*..."]
```

Also define a key.
```python
wall, clear, goal = "#", ".", "*"
```
The constraints are that you need to provide a function that takes a grid and a starting point and finds the shortest path to the goal. Provide the starting point as a tuple ```(0,0)```.

Here is a basic example:

```python
import collections, pdb


def bfs(grid, start):
    queue = collections.deque([[start]])
    seen = set([start])
    while queue:
        # pdb.set_trace()
        path = queue.popleft()
        x, y = path[-1]
        if grid[y][x] == goal:
            return path
        for x2, y2 in ((x+1,y), (x-1,y), (x,y+1), (x,y-1)):
            if 0 <= x2 < width and 0 <= y2 < height and grid[y2][x2] != wall and (x2, y2) not in seen:
                queue.append(path + [(x2, y2)])
                seen.add((x2, y2))
        # pdb.set_trace(); # For when you get stuck

```
I have another version without the collections package to see a base python implementation.

```python
lot = [[1,1,1,1,1,1,1],
       [1,1,0,1,1,1,1],
       [0,1,1,0,0,0,1],
       [0,0,1,1,1,1,9],]

def bfs(grid, start):
    # queue=[start]
    width = len(lot[0])
    height = len(lot)
    queue = [[start]]
    seen = set([start])
    loop_limit = 3000
    count=0
    while queue and count < loop_limit:
        path = queue.pop(0)
        x, y = path[-1]
        if lot[y][x] == 9:
            return (path,len(path))
        for x2, y2 in ((x+1,y),(x-1,y),(x,y+1),(x,y-1)):
            if 0 <= x2 < width and 0 <= y2 < height and lot[y][x] != 0 and (x2,y2) not in seen:
                queue.append(path + [(x2,y2)])
                seen.add((x2,y2))
        count=count+1
        #print(count)
        #print(path)
    return ["we didn't get to finish",path]

shortest_path = bfs(lot, (0,0))
print(shortest_path)
```


This prints out the following:

```
([(0, 0), (1, 0), (2, 0), (3, 0), (4, 0), (5, 0), (6, 0), (6, 1), (6, 2), (6, 3)], 10)
```

A list that contains the path traversed and the the number of points in the traversed path.

Finally there is a graph based approach:

```python
# Python3 Program to print BFS traversal 
# from a given source vertex. BFS(int s) 
# traverses vertices reachable from s. 
from collections import defaultdict 
import pdb
# This class represents a directed graph 
# using adjacency list representation 
class Graph: 
  
    # Constructor 
    def __init__(self): 
  
        # default dictionary to store graph 
        self.graph = defaultdict(list) 
  
    # function to add an edge to graph 
    def addEdge(self,u,v): 
        self.graph[u].append(v) 
  
    # Function to print a BFS of graph 
    def BFS(self, s): 
        
        # Mark all the vertices as not visited 
        visited = [False] * (len(self.graph)) 
  
        # Create a queue for BFS 
        queue = [] 
  
        # Mark the source node as  
        # visited and enqueue it 
        queue.append(s) 
        visited[s] = True
  
        while queue: 
            pdb.set_trace()
            # Dequeue a vertex from  
            # queue and print it 
            s = queue.pop(0) 
            print (s, end = " ") 
  
            # Get all adjacent vertices of the 
            # dequeued vertex s. If a adjacent 
            # has not been visited, then mark it 
            # visited and enqueue it 
            for i in self.graph[s]: 
                if visited[i] == False: 
                    queue.append(i) 
                    visited[i] = True
  
# Driver code 
  
# Create a graph given in 
# the above diagram 
g = Graph() 
g.addEdge(0, 1) 
g.addEdge(0, 2) 
g.addEdge(1, 2) 
g.addEdge(2, 0) 
g.addEdge(2, 3) 
g.addEdge(3, 3) 
  
print ("Following is Breadth First Traversal"
                  " (starting from vertex 2)") 
g.BFS(2) 
  
# This code is contributed by Neelam Yadav 
```
