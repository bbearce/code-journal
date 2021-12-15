# Sqlite

## create database

Syntax
Following is the basic syntax of sqlite3 command to create a database:
```bash
$sqlite3 DatabaseName.db
```

Always, database name should be unique within the RDBMS.

Example
If you want to create a new database <testDB.db>, then SQLITE3 statement would be as follows −
```bash
$sqlite3 testDB.db
SQLite version 3.7.15.2 2013-01-09 11:53:05
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
sqlite>
```

The above command will create a file **testDB.db** in the current directory. This file will be used as database by SQLite engine. If you have noticed while creating database, sqlite3 command will provide a sqlite> prompt after creating a database file successfully.

Once a database is created, you can verify it in the list of databases using the following SQLite **.databases** command.

```bash
sqlite>.databases
seq  name             file
---  ---------------  ----------------------
0    main             /home/sqlite/testDB.db
```

You will use SQLite ```.quit``` command to come out of the sqlite prompt as follows:
```bash
sqlite>.quit
$
```

### The ```.dump``` Command
You can use ```.dump``` dot command to export complete database in a text file using the following SQLite command at the command prompt.
```bash
$sqlite3 testDB.db .dump > testDB.sql
```

The above command will convert the entire contents of **testDB.db** database into SQLite statements and dump it into ASCII text file testDB.sql. You can perform restoration from the generated **testDB.sql** in a simple way as follows:
```bash
$sqlite3 testDB.db < testDB.sql
```

At this moment your database is empty, so you can try above two procedures once you have few tables and data in your database. For now, let's proceed to the next chapter.


## .tables

The following notes are from [sqlitetutorial.net](https://www.sqlitetutorial.net/sqlite-tutorial/sqlite-show-tables/)

List tables after connecting using ```.tables```
```bash
$ sqlite3 kaggle_rsna
SQLite version 3.11.0 2016-02-15 17:29:24
Enter ".help" for usage hints.
sqlite> .tables
all_images                         final_annotation_list            
all_images_corrected               remove_exams                     
annotations                        remove_series                    
annotations_corrected              study_and_instance_annotation_ids

```
The .tables command also can be used to show temporary tables. See the following example:

First, [create a new temporary](https://www.sqlitetutorial.net/sqlite-create-table/) table named temp_table1:

```bash
sqlite> CREATE TEMPORARY TABLE temp_table1( name TEXT );
```

Second, list all tables from the database:

```bash
sqlite> .tables
```

The following shows the output:

```bash
albums            employees         invoices          playlists
artists           genres            media_types       temp.temp_table1
customers         invoice_items     playlist_track    tracks
```

Because the schema of temporary tables is temp, the command showed the names of schema and table of the temporary table such as ```temp.temp_table1```.

If you want to show tables with the specific name, you can add a matching pattern:

```bash
.tables pattern
```

The command works the same as LIKE operator. The pattern must be surrounded by single quotation marks (').

For example, to find tables whose names start with the letter ‘a’, you use the following command:

```bash
sqlite> .table 'a%'
```

Here is the output:

```bash
albums   artists
```

To shows the tables whose name contains the string  ck, you use the %ck% pattern as shown in the following command:

```bash
sqlite> .tables '%ck%'
```

The output is as follows:

```bash
playlist_track  tracks
```

Showing tables using SQL statement:

Another way to list all tables in a database is to query them from the sqlite_master table.

```sql
SELECT 
    name
FROM 
    sqlite_master 
WHERE 
    type ='table' AND 
    name NOT LIKE 'sqlite_%';
```

Here is the output:

|name          |
|--------------|
|albums        |
|customers     |
|employees     |
|genres        |
|invoices      |
|invoice_items |
|media_types   |
|playlists     |
|playlist_track|
|tracks        |

## .schema / create table

View the schema of a particular table:

```bash
sqlite> .schema table
sqlite> .schema all_images
CREATE TABLE all_images(
  "InstanceID" TEXT,
  "SeriesID" TEXT,
  "StudyID" TEXT
);

```

## bulk insert

If the table doesn't exist, sqlite will try to create it and it's scheme assuming a header.

> .mode csv assumes a ',' separator.
```bash
sqlite> .mode csv <table_name> 
sqlite> .import <path_to_csv> <table_name>
```

> if you want to specify a different separator use this instead (I used a specific csv for this...):
```bash
sqlite> .separator ,
sqlite> .import <path_to_csv> <table_name>
sqlite> .schema
CREATE TABLE measurements(
  "seriesUID" TEXT,
  "instanceUID" TEXT,
  "length" TEXT,
  "start_x" TEXT,
  "start_y" TEXT,
  "end_x" TEXT,
  "end_y" TEXT,
  "annotator" TEXT
);
```
> I noticed that it was lazy about assigning data types.


## csv export

An example export from the ```rad``` table.

> Any query can be used as the basis for the export, including multi-line complex ones.

```bash
sqlite> .headers on
sqlite> .mode csv
sqlite> .output rad_data.csv
sqlite> select * from rad;
sqlite> 

```