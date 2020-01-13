# Useful Everyday Code

## Drop If Exists

```sql
DROP TABLE [IF EXISTS] [schema_name.]table_name;
```

## Select Into

*source [stackoverflow](https://stackoverflow.com/questions/2361921/select-into-statement-in-sqlite)*

```sql
CREATE TABLE equipments_backup AS SELECT * FROM equipments
```

## Ranks / Row_Number

> Courtest of [sqlitetutorial](https://www.sqlitetutorial.net/sqlite-window-functions/sqlite-rank/)

*Window function support was first added to SQLite with release version 3.25.0 (2018-09-15). The SQLite developers used the PostgreSQL window function documentation as their primary reference for how window functions ought to behave.*

The ```RANK()``` function is a window function that assigns a rank to each row in a query’s result set. The rank of a row is calculated by one plus the number of ranks that comes before it.

The following shows the syntax of the ```RANK()``` function:

```sql
RANK() OVER (
    PARTITION BY <expression1>[{,<expression2>...}]
    ORDER BY <expression1> [ASC|DESC], [{,<expression1>...}]
)
```

> Courtesty of [sqlite](https://www.sqlite.org/windowfunctions.html)

SQLite supports the following 11 built-in window functions:

* row_number()  

    The number of the row within the current partition. Rows are numbered starting from 1 in the order defined by the ORDER BY clause in the window definition, or in arbitrary order otherwise.

* rank()  

    The row_number() of the first peer in each group - the rank of the current row with gaps. If there is no ORDER BY clause, then all rows are considered peers and this function always returns 1.

* dense_rank()  

    The number of the current row's peer group within its partition - the rank of the current row without gaps. Partitions are numbered starting from 1 in the order defined by the ORDER BY clause in the window definition. If there is no ORDER BY clause, then all rows are considered peers and this function always returns 1.

* percent_rank()  

    Despite the name, this function always returns a value between 0.0 and 1.0 equal to (rank - 1)/(partition-rows - 1), where rank is the value returned by built-in window function rank() and partition-rows is the total number of rows in the partition. If the partition contains only one row, this function returns 0.0.

* cume_dist()  

    The cumulative distribution. Calculated as row-number/partition-rows, where row-number is the value returned by row_number() for the last peer in the group and partition-rows the number of rows in the partition.

* ntile(N)  

    Argument N is handled as an integer. This function divides the partition into N groups as evenly as possible and assigns an integer between 1 and N to each group, in the order defined by the ORDER BY clause, or in arbitrary order otherwise. If necessary, larger groups occur first. This function returns the integer value assigned to the group that the current row is a part of.