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

## Apply Function

```python

x = pd.DataFrame({'A':[1,1,1,1,2,2,2,2], 'B':[0,0,0,0,0,1,0,1], 'C':['a' for i in range(8)]})
y = pd.DataFrame({'A':[1,1,1,1,1,2,2,2,2,2], 'B':[1,0,1,0,1,0,1,0,1,1], 'C':['a' for i in range(10)]})
z = pd.DataFrame({'A':[1,1,1,1,2,2,2,2], 'B':[2,3,6,9,1,1,6,7], 'C':['a' for i in range(8)]})

def some_agg_function(df_group):
    # It's important for the values to be sorted to make this easier
    df_group = df_group.sort_values('B')
    nrows = len(df_group)
    if nrows % 2 == 0:
    middle_index = nrows / 2 - 1; # for 0 based inded purposes; chooses 2 out of (1,[2],3,4)
    else:
    middle_index = math.floor(nrows / 2)
    # pdb.set_trace()
    return df_group.iloc[int(middle_index),:]
print(x); print(x[['A','B']].groupby('A').apply(some_agg_function))
print(y);print(y[['A','B']].groupby('A').apply(some_agg_function))
print(z);print(z[['A','B']].groupby('A').apply(some_agg_function))
```