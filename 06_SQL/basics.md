# SQL basics

## 📘 What is SQL?

**SQL (Structured Query Language)** is used to store, retrieve, update, and manipulate data in relational databases.

## Common SQL Commands

### DDL – Data Definition Language

> Deals with **structure/schema** of the database.

| Command     | Purpose                          |
|-------------|----------------------------------|
| `CREATE`    | Create a new table/database      |
| `ALTER`     | Modify an existing table         |
| `DROP`      | Delete a table/database          |
| `TRUNCATE`  | Remove all data, keep structure  |
| `RENAME`    | Rename table or column           |


### DML – Data Manipulation Language

> Deals with **actual data inside** the tables.

| Command      | Purpose                                |
|--------------|----------------------------------------|
| `SELECT`     | Query data from a table                |
| `INSERT`     | Add new records                        |
| `UPDATE`     | Modify existing records                |
| `DELETE`     | Remove records                         |
| `WHERE`      | Filter records                         |
| `GROUP BY`   | Group data for aggregation             |
| `ORDER BY`   | Sort results                           |
| `JOIN`       | Combine rows from multiple tables      |

---


## SELECT: Retrieve data from a table


### 1. Basic SELECT Syntax

```sql
SELECT column1, column2, ...
FROM table_name;
```
Note: Use * to select all columns.

### 2. SELECT with WHERE Clause

```sql
SELECT column1, column2
FROM table_name
WHERE condition;
```

### 3. Common Comparison Operators

|Operator|Meaning|Example|
|---|---|---|
|=|Equal to|age = 30|
|<> or !=|Not equal to|age != 25|
|>, <|Greater/Less|salary > 5000|
|>=, <=|Greater/Less or Equal|age >= 18|
|BETWEEN|Range|age BETWEEN 20 AND 30|
|IN|In a list|country IN ('US', 'UK')|
|LIKE|Pattern match|name LIKE 'A%'|
|IS NULL|Null check|email IS NULL|


### 4. DISTINCT: Remove Duplicates

```sql
SELECT DISTINCT department FROM employees;
```

### 5. LIMIT: Restrict the Number of Rows (MySQL/PostgreSQL)

```sql
SELECT * FROM products
LIMIT 10 OFFSET 5; # Retrieve 10 rows from row 6
```

## Aggregate Functions

- COUNT: Returns the number of rows that match a specified condition
- SUM(): Sums values
- AVG(): Calculates average
- MAX(): Finds max value
- MIN(): Finds min value


### COUNT(*) vs COUNT(column):

| Syntax        | Description                                   |
|---------------|-----------------------------------------------|
| `COUNT(*)`    | Counts **all rows**, including NULL values.   |
| `COUNT(column)` | Counts **non-NULL** values in the column.  |

```sql
SELECT COUNT(*) FROM employees;
-- Total number of rows in the employees table

SELECT COUNT(salary) FROM employees;
-- Counts rows where salary is NOT NULL
```

## INSERT: Insert rows of data

```sql
INSERT INTO table_name (column1, column2,...)
VALUES 
  (val1a, val2a,...),
  (val1b, val2b,...),
  (val1c, val2c,...);
```

We can insert data from one table into another using INSERT with SELECT.

```sql
INSERT INTO archive_employees (name, department)
SELECT name, department
FROM employees
WHERE status = 'Terminated';
```

## UPDATE: Modify existing records in a table

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**NOTE**: Always use a WHERE clause to avoid updating all rows.

## DELETE: Remove records from a table

```sql
DELETE FROM table_name
WHERE condition;
```

**NOTE**: Without WHERE, all rows will be deleted!

---

## **Data Definition Language (DDL)** commands

## CREATE TABLE: Define a **new table** and its columns

Syntax:

```sql
CREATE TABLE table_name (
  column1 datatype constraints,
  column2 datatype constraints,
  ...
);
```

Example:

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  age INT,
  hire_date DATE DEFAULT CURRENT_DATE
);
```

## ALTER TABLE: Modify an existing table: add, delete, or rename columns

### Add column

```sql
ALTER TABLE employees
ADD COLUMN salary DECIMAL(10, 2);
```

### Drop Column

```sql
ALTER TABLE employees
DROP COLUMN age;
```

### Rename Column (PostgreSQL)

```sql
ALTER TABLE employees
RENAME COLUMN name TO full_name;
```

### Modify Data Type (MySQL/PostgreSQL)

```sql
ALTER TABLE employees
ALTER COLUMN salary TYPE FLOAT;
```

## DROP TABLE: Deletes the entire table **permanently**, including all data and structure

```sql
DROP TABLE table_name;
```

**NOTE**: The action cannot be undone.

## TRUNCATE: Removes all rows from a table but **keeps the structure**

```sql
TRUNCATE TABLE table_name;
```