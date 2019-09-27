# Sqlite

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

## .schema

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

```bash
sqlite> .mode csv all_images 
sqlite> .import <path_to_csv> all_images
```