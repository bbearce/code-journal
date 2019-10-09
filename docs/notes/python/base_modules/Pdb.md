# Pdb

## Useage - [Docs](https://docs.python.org/3/library/pdb.html)

You can use 

```python
import pdb
pdb.set_trace()
```

or *New: as of 3.7* [breakpoint()](https://docs.python.org/3/library/functions.html#breakpoint)

```python
breakpoint()
```

to enter debug mode.

## Hidden gems

Use ```!``` to escape default pdb commands like ```n(ext), c(ont(inue)) and h(elp)```

```python
>>> c = 1
>>> breakpoint()
--Return--
> <stdin>(1)<module>()->None
(Pdb) !c
1
(Pdb) 
```