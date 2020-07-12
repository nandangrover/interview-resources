
# SQL Questions and Explanations

### 1. What is the difference between primary key and foreign key ?

|Primary key | Foreign key|
|------------|------------|
|Primary key uniquely identifies a record in a relational database|Foreign key refers to the field in the table which is primary key of other table|
|A table can contain only one primary key|A table can have multiple foreign keys|
|Does not allow null value|Allow null value|

### 2. What is the difference between primary key and unique key ?

|Primary key| Unique key|
|-----------|-----------|
|Used to serve as a unique identifier for each row in a table.|Uniquely determines a row which isn’t primary key.|
|Does not allow null value|Allow null value|
|A table can contain only one primary key|A table can have multiple unique keys|

### 3. SQL query to find third highest salary in company
The most simple way that should work in any database is to do following:

```sql
SELECT * FROM `employee` ORDER BY `salary` DESC LIMIT 1 OFFSET 2;
```

Which orders employees by salary and then tells db to return a single result (1 in LIMIT) counting from third row in result set (2 in OFFSET). It may be OFFSET 3 if your DB counts result rows from 1 and not from 0.

This example should work in MySQL and PostgreSQL.
