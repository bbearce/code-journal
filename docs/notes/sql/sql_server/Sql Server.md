# SQL Server

Testing the addition of a new language

```sql
SELECT TOP 10000 sum(colC) FROM DB_Server.dbo.DB_Table
WHERE colA = 'some value'
GROUP BY colB 

```