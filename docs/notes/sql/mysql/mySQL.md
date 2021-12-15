# mySQL



## Basics

First, connect to your MySQL database using your MySQL client from your operating system command line:

```$ mysql -u root -p```

Next, after you're logged into your MySQL database, tell MySQL which database you want to use:

```sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| MySQL_DevDB        |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.01 sec)

mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              |
| db                        |
| event                     |
| func                      |
| general_log               |
| help_category             |
| help_keyword              |
| help_relation             |
| help_topic                |
| innodb_index_stats        |
| innodb_table_stats        |
| ndb_binlog_index          |
| plugin                    |
| proc                      |
| procs_priv                |
| proxies_priv              |
| servers                   |
| slave_master_info         |
| slave_relay_log_info      |
| slave_worker_info         |
| slow_log                  |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
| user                      |
+---------------------------+
28 rows in set (0.00 sec)

```


### Create Database

You can now work with the database. For example, the following commands demonstrate how to create a basic table named ```example```, and how to insert some data into it:

```sql
CREATE TABLE example ( id smallint unsigned not null auto_increment, name varchar(20) not null, constraint pk_example primary key (id) );
INSERT INTO example ( id, name ) VALUES ( null, 'Sample data' );
```

### Drop Database\Table

```sql
DROP DATABASE dbname;
DROP TABLE tablename;
```

Type ```\q``` to exit the mysql program.

### New User

> Courtesty of [a2hosting.com](https://www.a2hosting.com/kb/developer-corner/mysql/managing-mysql-databases-and-users-from-the-command-line)

To create a database user, type the following command. Replace ```username``` with the user you want to create, and replace ```password``` with the user's password:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password';
```

This command grants the user all permissions. However, you can grant specific permissions to maintain precise control over database access. For example, to explicitly grant the ```SELECT``` permission, you would use the following command:

```sql
GRANT SELECT ON *.* TO 'username'@'localhost'
```

To log in to MySQL as the user you just created, type the following command. Replace ```username``` with the name of the user you created in step 3:

```sql
mysql -u username -p
```

### Delete User

To view a list of all users, type the following command from the mysql> prompt:

```sql
SELECT user FROM mysql.user GROUP BY user;
```

To delete a specific user, type the following command from the mysql> prompt. Replace username with the name of the user that you want to delete:

```sql
DELETE FROM mysql.user WHERE user = 'username';
```

### Using SQL script files

Create a file named **example.sql** and open it in your preferred text edtior. Copy and paste the following text into the file:

```sql
CREATE DATABASE dbname;
USE dbname;
CREATE TABLE tablename ( id smallint unsigned not null auto_increment, name varchar(20) not null, constraint pk_example primary key (id) );
INSERT INTO tablename ( id, name ) VALUES ( null, 'Sample data' );
```

To process the SQL script, type the following command. Replace username with the name of the user you just created:

```mysql -u username -p < example.sql```


## MySQL Root Password Guide

> Courtest of [a2hosting](https://www.a2hosting.com/kb/developer-corner/mysql/reset-mysql-root-password)

To reset the root password for MySQL, follow these steps:

### Log in to your account using SSH.  

You must runUsing SQL script files the commands in the following steps as the ```root``` user. Therefore, you can either log in directly as the root user (which is not recommended for security reasons), or use the ```su``` or ```sudo``` commands to run the commands as the ```root``` user.  

### Stop the MySQL server using the appropriate command for your Linux distribution:  

* For Debian and Ubuntu, type: ```service mysql stop```  
* For CentOS and Fedora, type: ```service mysqld stop```  

### Restart the MySQL server with the **—skip-grant-tables** option. To do this, type the following command:  

```mysqld_safe --skip-grant-tables &```

Make sure you type the ampersand (```&```) at the end of the command. This runs the command in the background and allows you to type the commands in the following steps.

> Running MySQL with the **—skip-grant-tables** option enabled is highly insecure, and should only be done for a brief period while you reset the password. The steps below show you how to stop the mysqld_safe server instance safely and start the MySQL server securely after you have reset the root password.

### Log into MySQL using the following command:

```mysql```

At the ```mysql>``` prompt, reset the password. To do this, type the following command, replacing ```NEW-PASSWORD``` with the new root password:

```UPDATE mysql.user SET Password=PASSWORD('NEW-PASSWORD') WHERE User='root';```

At the mysql> prompt, type the following commands:

```sql
FLUSH PRIVILEGES;
exit;
```

Stop the MySQL server using the following command. You will be prompted to enter the new MySQL root password before the MySQL server shuts down:

```mysqladmin -u root -p shutdown```

Start the MySQL server normally. To do this, type the appropriate command for your Linux distribution:

* For Debian and Ubuntu, type:  ```service mysql start```  
* For CentOS and Fedora, type:  ```service mysqld start```  
