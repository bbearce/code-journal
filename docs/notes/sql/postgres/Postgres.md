# Postgres

> Courtesy of [Stackoverflow](https://stackoverflow.com/questions/1471571/how-to-configure-postgresql-for-the-first-time 
)

## Installation

Whoops...I'm sure this is easy...I'll get to it eventually.

## First Time Use and Setup 

Here's what worked for postgresql-9.1 on Xubuntu 12.04.1 LTS.

Connect to the default database with user ```postgres```: 

```sudo -u postgres psql template1```

Set the password for user ```postgres```, then exit psql (Ctrl-D) : 

```sql
postgres=# ALTER USER postgres with encrypted password 'xxxxxxx';
```

Edit the *pg_hba.conf* file: 

```sudo vim /etc/postgresql/9.1/main/pg_hba.conf ```

And change ```peer``` to ```md5``` on the line concerning postgres : 
```
local      all     postgres     peer md5 
```
> Note: you need sudo or the file will appear blank 

Restart the database: 

```postgres=# sudo /etc/init.d/postgresql restart\``` 

(Here you can check it worked with ```psql -U postgres```.) 

Create a user having the same name as you (to find it, you can type ```whoami```) : 

```sql
postgres=# createuser -U postgres -d -e -E -l -P -r -s <my_name> 
```

The options tell postgresql to create a user that can login, create databases, create new roles, is a superuser, and will have an encrypted password. The really important ones are ```-P``` ```-E```, so that you're asked to type the password that will be encrypted, and ```-d``` so that you can do a ```createdb```. 

Beware of passwords : it will first ask you twice the new password (for the new user), repeated, and then once the postgres password (the one specified on step 2). 

Again, edit the *pg_hba.conf *file (see step 3 above), and change ```peer``` to ```md5``` on the line concerning "all" other users : 

```
local      all     all     peer md5 
```
Restart (like in step 4), and check that you can login without ```-U postgres```: 

```
psql template1 
```

> Note that if you do a mere psql, it will fail since it will try to connect you to a default database having the same name as you (ie. ```whoami```). ```template1``` is the admin database that is here from the start. 

Now ```createdb <dbname>``` should work. 


## Display all DBs: 

Use ```\list``` or ```\l``` to display databases.

```sql
postgres=# \list
                                    List of databases
      Name       |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------------+----------+----------+------------+------------+-----------------------
 codalab_website | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 postgres        | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0       | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
                 |          |          |            |            | postgres=CTc/postgres
 template1       | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
                 |          |          |            |            | postgres=CTc/postgres
(4 rows)

```
## Display Tables

Use ```\dt``` to list all public tables, and ```\dt *``` for all tables in the current DB you are connected to.  
```sql
postgres=# \dt

                      List of relations
 Schema |                Name                 | Type  | Owner 
--------+-------------------------------------+-------+-------
 public | account_emailaddress                | table | root
 public | account_emailconfirmation           | table | root
 public | auth_group                          | table | root
 public | auth_group_permissions              | table | root
 public | auth_permission                     | table | root
 public | authenz_cluser                      | table | root
 public | authenz_cluser_groups               | table | root
 public | authenz_cluser_user_permissions     | table | root
 public | captcha_captchastore                | table | root
 public | coopetitions_dislike                | table | root
 public | coopetitions_downloadrecord         | table | root
 public | coopetitions_like                   | table | root
 public | customizer_configuration            | table | root
 public | django_admin_log                    | table | root
...

codalab_website=# \dt *
                          List of relations
   Schema   |                Name                 | Type  |  Owner   
------------+-------------------------------------+-------+----------
 pg_catalog | pg_aggregate                        | table | postgres
 pg_catalog | pg_am                               | table | postgres
 pg_catalog | pg_amop                             | table | postgres
 pg_catalog | pg_amproc                           | table | postgres
 pg_catalog | pg_attrdef                          | table | postgres
 pg_catalog | pg_attribute                        | table | postgres
 pg_catalog | pg_auth_members                     | table | postgres
 pg_catalog | pg_authid                           | table | postgres
 pg_catalog | pg_cast                             | table | postgres
 pg_catalog | pg_class                            | table | postgres
...

```
## Display Connection Information

```sql
codalab_website-# \conninfo
You are connected to database "codalab_website" as user "root" via socket in "/var/run/postgresql" at port "5432".

```

> You will never see tables in other databases, these tables aren't visible. You have to connect to the correct database to see its tables (and other objects). 

 
## Switch DBs: 

To switch databases: 
```sql
postgres=# \connect database_name 
```

Selects all non-template DBs: 
```sql
postgres=# SELECT datname FROM pg_database 
WHERE datistemplate = false; 
```
 

Select all tables in current DB connection 
```sql
postgres=# SELECT table_schema,table_name 
FROM information_schema.tables 
ORDER BY table_schema,table_name; 
```

## Delete Database: 

```sql
postgres=# DROP DATABASE [ IF EXISTS ] name 
```
 

## Create Database: 

```sql
postgres=# CREATE DATABASE testdb; 
```

## Create User: 

### See all current users: 
```sql
postgres=# SELECT usename FROM pg_user; 
```

### Create New User:

Bash:
```bash
$ createuser name  
```
or  
Postgres:
```sql
postgres=# CREATE USER testuser; 
```

### Drop User:
Bash:
```bash
$ dropuser name 
```
or  
Postgres:
```sql
postgres=# DROP USER testuser; 
```
 
## Bulk Insert:
> https://stackoverflow.com/questions/12856377/the-correct-copy-command-to-load-postgresql-data-from-csv-file-that-has-single-q


Double single quotes (if standard_conforming_strings is on, see the docs)
```sql
COPY my_table FROM 'c:\downloads\file.csv' DELIMITERS ',' CSV QUOTE '''';
```

or use the non-standard PostgreSQL-specific escape string:
```sql
COPY my_table FROM 'c:\downloads\file.csv' DELIMITERS ',' CSV QUOTE E'\'';
```

If you have a header:
COPY my_table FROM 'c:\downloads\file.csv' WITH DELIMITER ',' CSV HEADER;

> use \copy in bash