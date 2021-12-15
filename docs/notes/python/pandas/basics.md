# Pandas

> Some Notes [pandas.pydata...](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)

## Display

```python
pd.set_option('display.max_columns', 50) # How many to show
pd.set_option('display.min_rows', 25) # How many to show
pd.set_option('display.max_rows', 25) # How many to show
pd.set_option('display.width', 1000) # How far across the screen
pd.set_option('display.max_colwidth', 1) # Column width in px
pd.set_option('display.max_colwidth', 40) # Column width in px
pd.set_option('expand_frame_repr', True) # allows for the representation of dataframes to stretch across pages, wrapped over the full column vs row-wise
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


## sqlite3 Intgration

> Source: [dataquest](https://www.dataquest.io/blog/python-pandas-databases/)

### Just sqlite3 library

In order to work with a SQLite database from Python, we first have to connect to it. We can do that using the connect function, which returns a Connection object:

```python
import sqlite3
conn = sqlite3.connect("flights.db")
```
Once we have a Connection object, we can then create a Cursor object. Cursors allow us to execute SQL queries against a database:

```python
cur = conn.cursor()
```

Once we have a Cursor object, we can use it to execute a query against the database with the aptly named execute method. The below code will fetch the first 5 rows from the airlines table:

```python
cur.execute("select * from airlines limit 5;")
```

You may have noticed that we didn’t assign the result of the above query to a variable. This is because we need to run another command to actually fetch the results. We can use the fetchall method to fetch all of the results of a query:

```python
results = cur.fetchall()
print(results)
```

```python
[(0, '1', 'Private flight', '\\N', '-', None, None, None, 'Y'), (1, '2', '135 Airways', '\\N', None, 'GNL', 'GENERAL', 'United States', 'N'), (2, '3', '1Time Airline', '\\N', '1T', 'RNX', 'NEXTIME', 'South Africa', 'Y'), (3, '4', '2 Sqn No 1 Elementary Flying Training School', '\\N', None, 'WYT', None, 'United Kingdom', 'N'), (4, '5', '213 Flight Unit', '\\N', None, 'TFU', None, 'Russia', 'N')]
```

As you can see, the results are formatted as a list of tuples. Each tuple corresponds to a row in the database that we accessed. Dealing with data this way is fairly painful. We’d need to manually add column heads, and manually parse the data. Luckily, the pandas library has an easier way, which we’ll look at in the next section.

Before we move on, it’s good practice to close Connection objects and Cursor objects that are open. This prevents the SQLite database from being locked. When a SQLite database is locked, you may be unable to update the database, and may get errors. We can close the Cursor and the Connection like this:

```python
cur.close()
conn.close()
```

### Add Pandas

```python
import pandas as pd
import sqlite3
conn = sqlite3.connect("flights.db")
df = pd.read_sql_query("select * from airlines limit 5;", conn)
```

#### Inserting rows with Python

To insert a row, we need to write an INSERT query. The below code will add a new row to the airlines table. We specify 9 values to insert, one for each column in airlines. This will add a new row to the table.

```python
cur = conn.cursor()
cur.execute("insert into airlines values (6048, 19846, 'Test flight', '', '', null, null, null, 'Y')")
conn.commit()
```

You want to avoid doing this! Inserting values with Python string formatting makes your program vulnerable to SQL Injection attacks. Luckily, sqlite3 has a straightforward way to inject dynamic values without relying on string formatting:

```python
cur = conn.cursor()
values = ('Test Flight', 'Y')
cur.execute("insert into airlines values (6049, 19847, ?, '', '', null, null, null, ?)", values)
conn.commit()
```

#### Updating rows

We can modify rows in a SQLite table using the execute method:

```python
cur = conn.cursor()
values = ('USA', 19847)
cur.execute("update airlines set country=? where id=?", values)
conn.commit()
```

#### Deleting rows

Finally, we can delete the rows in a database using the execute method:

```python
cur = conn.cursor()values = (19847, )
cur.execute("delete from airlines where id=?", values)conn.commit()
```

#### Creating tables with pandas

The pandas package gives us a much faster way to create tables. We just have to create a DataFrame first, then export it to a SQL table. First, we’ll create a DataFrame:

```python
from datetime import datetime
df = pd.DataFrame(
[[1, datetime(2016, 9, 29, 0, 0) ,
datetime(2016, 9, 29, 12, 0), 'T1', 1]],
columns=["id", "departure", "arrival", "number", "route_id"])
Then, we’ll be able to call the to_sql method to convert df to a table in a database. We set the keep_exists parameter to replace to delete and replace any existing tables named daily_flights:

df.to_sql("daily_flights", conn, if_exists="replace")
```





