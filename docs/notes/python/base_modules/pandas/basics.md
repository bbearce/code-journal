# Pandas

> Some Notes [pandas.pydata...](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)

## Display

```python
pd.set_option('display.max_columns', 50) # How many to show
pd.set_option('display.max_rows', 25) # How many to show
pd.set_option('display.width', 1000) # How far across the screen
pd.set_option('display.max_colwidth', 1) # Column width in px
pd.set_option('display.max_colwidth', 40) # Column width in px
```

## Making DataFrames

### From Dictionaries
```python
# Method 1: each key is a column with array datatype for values
>>> data = {'a':[1,4], 'b':[2,5], 'c':[3,6]}
>>> pandas.DataFrame(data)
   a  b  c
0  1  2  3
1  4  5  6

# Use 'columns' to specify order
>>> pandas.DataFrame(data, columns=['b','c','a'])
   b  c  a
0  2  3  1
1  5  6  4

# Method 2: Creating Dataframe from list of dicts
>>> data = [{'a': 1, 'b': 2, 'c': 3}, {'a': 10, 'b': 20, 'c': 30}]
>>> pandas.DataFrame(data) 
    a   b   c
0   1   2   3
1  10  20  30

```

### From lists
```python
>>> rows = [[1,2,3],[4,5,6]]
>>> pandas.DataFrame(rows, columns=['a','b','c'])
   a  b  c
0  1  2  3
1  4  5  6
```

### From excel
```python
import pandas as pd

cars = pd.read_excel(r'C:\Users\Ron\Desktop\Cars.xlsx')
df = pd.DataFrame(cars, columns = ['Brand', 'Price'])

```
