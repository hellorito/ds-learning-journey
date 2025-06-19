# 🧠 SQL Intermediate Queries: Subqueries & Multi-Table Operations

## 1. Subqueries

### 1.1 What is a Subquery?

A **subquery** is a query nested inside another SQL query. It can be used in `SELECT`, `WHERE`, `FROM`, or `HAVING` clauses to return a value, row, column, or table.

> Subqueries can be:
- Scalar (returning a single value)
- Row/Column (returning a row or list of values)
- Table-level (used as virtual tables in FROM)

---

### 1.2 Types of Subqueries

| Type                | Used In               | Returns        |
|---------------------|------------------------|----------------|
| Scalar Subquery     | SELECT, WHERE, HAVING  | Single value   |
| Column Subquery     | IN, ANY, ALL           | One column     |
| Row Subquery        | WHERE, FROM            | One row        |
| Table Subquery      | FROM                   | Virtual table  |
| Correlated Subquery | Depends on outer query | Value(s)/row   |

---

### 1.3 Common Use Cases

- Filtering: `WHERE IN (...)`, `WHERE EXISTS (...)`
- Aggregation: Return count, average, max, etc.
- Row-wise logic: correlated subqueries
- Derived tables in FROM for grouped stats

---

### 1.4 Examples

```sql
-- Subquery in WHERE (IN)
SELECT name
FROM students
WHERE id IN (
  SELECT student_id FROM enrollments WHERE course = 'SQL'
);

-- Scalar subquery in SELECT
SELECT 
  name,
  (SELECT COUNT(*) FROM enrollments e WHERE e.student_id = s.id) AS total_courses
FROM students s;

-- Subquery in FROM (derived table)
SELECT course, avg_score
FROM (
  SELECT course, AVG(score) AS avg_score
  FROM enrollments
  GROUP BY course
) AS course_stats
WHERE avg_score > 80;

-- Subquery in HAVING
SELECT course, COUNT(*) AS student_count
FROM enrollments
GROUP BY course
HAVING COUNT(*) > (
  SELECT AVG(cnt)
  FROM (
    SELECT COUNT(*) AS cnt
    FROM enrollments
    GROUP BY course
  ) AS avg_counts
);

-- Correlated subquery using EXISTS
SELECT name
FROM students s
WHERE EXISTS (              -- EXISTS returns True/False, if True, outputs this row
  SELECT 1                  -- SELECT not important here
  FROM enrollments e
  WHERE e.student_id = s.id AND e.course = 'Python'
);
```

## 2. Working with Multiple Tables

### 2.1 JOIN Overview (INNER, LEFT, RIGHT, FULL)

#### 1. INNER JOIN

- Returns only rows that have matching values in **both tables**.
- Most commonly used join.

#### 2. LEFT JOIN (LEFT OUTER JOIN)

- Returns **all rows from the left table**, and matched rows from the right table.
- If no match, NULLs appear on the right side.

#### 3. RIGHT JOIN (RIGHT OUTER JOIN)

- Returns **all rows from the right table**, and matched rows from the left table.
- If no match, NULLs appear on the left side.

#### 4. FULL OUTER JOIN

- Returns **all rows** from both tables.
- If there’s no match, NULLs will appear from the missing side.


**Example:**

```sql
-- INNER JOIN
SELECT s.name, e.course
FROM students s
INNER JOIN enrollments e ON s.id = e.student_id;

-- LEFT JOIN
SELECT s.name, e.course
FROM students s
LEFT JOIN enrollments e ON s.id = e.student_id;

-- RIGHT JOIN (not in all databases like SQLite)
SELECT s.name, e.course
FROM students s
RIGHT JOIN enrollments e ON s.id = e.student_id;

-- FULL OUTER JOIN (supported in PostgreSQL, SQL Server)
SELECT s.name, e.course
FROM students s
FULL OUTER JOIN enrollments e ON s.id = e.student_id;
```



### 2.2 UNION vs JOIN

- UNION: Combine rows from multiple queries (same column count)
- JOIN: Combine columns from multiple tables based on keys
  
```sql
-- UNION example
SELECT name FROM students
UNION
SELECT name FROM alumni;
```


### 2.3 CROSS JOIN and SELF JOIN

#### CROSS JOIN

- Produces the **Cartesian product** of two tables.
- Syntax: `SELECT * FROM A CROSS JOIN B;`
- No `ON` clause needed.
- Total rows returned = A.rows × B.rows.

**Example:**

```sql
colors                    sizes
+--------+              +-------+
| color  |              | size  |
+--------+              +-------+
| red    |              | S     |
| blue   |              | M     |
+--------+              +-------+

SELECT * FROM colors CROSS JOIN sizes;

--output:

color | size
------+------
red   | S
red   | M
blue  | S
blue  | M
```

#### SELF JOIN

- Joins a table to itself, using aliases.
- Common for hierarchical data: employee-manager, parent-child.

**Example:**

```sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```

---

# Thanks for reading!

#### Written by @hellorito