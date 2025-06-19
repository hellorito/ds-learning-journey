# SQL Built-in Functions Overview

SQL provides many built-in functions to perform operations on data. These functions fall into several categories:

- Aggregate Functions
- String Functions
- Numeric Functions
- Date/Time Functions
- Conversion Functions
- Null Handling Functions

---

## 1. Aggregate Functions

Used to perform calculations on a set of rows.

| Function  | Description     | Example                        |
|-----------|-----------------|--------------------------------|
| `COUNT()` | Number of rows  | `COUNT(*)`                     |
| `SUM()`   | Sum of values   | `SUM(salary)`                  |
| `AVG()`   | Average of values| `AVG(score)`                   |
| `MIN()`   | Minimum value   | `MIN(age)`                     |
| `MAX()`   | Maximum value   | `MAX(price)`                   |

---

## 2. String Functions

| Function           | Description                             | Example                              |
|--------------------|------------------------------------------|---------------------------------------|
| `LENGTH()`         | Returns length of a string               | `LENGTH(name)`                        |
| `UPPER()` / `LOWER()` | Converts to upper/lower case        | `UPPER(name)`                         |
| `TRIM()`           | Removes leading/trailing spaces          | `TRIM('  hello ')` → `'hello'`       |
| `SUBSTRING()`      | Extracts part of a string                | `SUBSTRING(name, 1, 3)`               |
| `CONCAT()`         | Combines strings                         | `CONCAT(first_name, ' ', last_name)` |
| `REPLACE()`        | Replaces part of a string                | `REPLACE(email, '.com', '.org')`     |

---

## 3. Numeric Functions

| Function      | Description                      | Example                     |
|---------------|----------------------------------|-----------------------------|
| `ROUND()`     | Rounds a number                  | `ROUND(123.456, 2)` → `123.46` |
| `CEIL()`      | Rounds up                        | `CEIL(3.14)` → `4`          |
| `FLOOR()`     | Rounds down                      | `FLOOR(3.99)` → `3`         |
| `ABS()`       | Absolute value                   | `ABS(-42)` → `42`           |
| `MOD()`       | Remainder of division            | `MOD(10, 3)` → `1`          |
| `POWER(x, y)` | Exponentiation                   | `POWER(2, 3)` → `8`         |

---

## 4. Date & Time Functions

## ✅ Commonly Used Date/Time Functions

### 1. Get Current Date and Time

| Function | Description |
|----------|-------------|
| `NOW()` | Returns current date and time |
| `CURRENT_DATE` | Returns current date |
| `CURRENT_TIME` | Returns current time |

**Example:**
```sql
SELECT NOW();  -- 2025-06-18 14:03:21
SELECT CURRENT_DATE;  -- 2025-06-18
```

### 2. Extract Date Parts

| Function | Description |
|----------|-------------|
|`YEAR(date_col)`|Extracts the year|
|`MONTH(date_col)`|Extracts the month|
|`DAY(date_col)`|Extracts the day|
|`HOUR(date_col)`|Extracts the hour|
|`MINUTE(date_col)`|Extracts the minute|
|`SECOND(date_col)`|Extracts the second|

**Example:**

```sql
SELECT 
  YEAR(birth_date) AS birth_year,
  MONTH(birth_date) AS birth_month,
  DAY(birth_date) AS birth_day
FROM users;
```

### 3. Calculate Date Differences

| Function             | Description                                | 
|----------------------|--------------------------------------------|
|`DATEDIFF(date1, date2)`|Returns the number of days between two dates|

**Example:**

```sql
SELECT DATEDIFF(NOW(), register_date) AS days_since_register
FROM users;
```

### 4. Date Arithmetic

| Function             | Description                     | 
|----------------------|---------------------------------|
|`DATE_ADD(date, INTERVAL n DAY)`|Add days to a date       |
|`DATE_SUB(date, INTERVAL n DAY)`|Subtract days from a date|

**Example:**

```sql
SELECT 
  DATE_ADD(order_date, INTERVAL 7 DAY) AS delivery_estimate,
  DATE_SUB(order_date, INTERVAL 3 DAY) AS early_ship
FROM orders;
```

## 🚀 Advanced Date/Time Functions

### 1. Extract Date Parts

| Function             | Description                     | 
|----------------------|---------------------------------|
|`EXTRACT(part FROM date)`|ANSI standard to get part of a date|
|`DATE_PART('part', date)`|PostgreSQL equivalent|

**Example (PostgreSQL):**

```sql
SELECT 
  EXTRACT(YEAR FROM birth_date),
  DATE_PART('month', birth_date)
FROM users;
```

### 2. AGE() (PostgreSQL)

| Function             | Description                     | 
|----------------------|---------------------------------|
|`AGE(date1, date2)`     |Returns interval between two dates (years, months, days)|

**Example:**

```sql
SELECT AGE(NOW(), birth_date) FROM users;
-- Returns: "25 years 3 mons 12 days"
```

### 3. Truncate Date

| Function             | Description                     | 
|----------------------|---------------------------------|
|`DATE_TRUNC('unit', date)`|Truncates to start of specified unit (PostgreSQL)|
|`TRUNC(date, 'MM')`|Oracle-style truncation|

**Example (PostgreSQL):**

```sql
SELECT DATE_TRUNC('month', order_date) AS month_start FROM orders;
-- Output: 2025-06-01 00:00:00
```

### 4. Format Date 

| Function             | Description                     | 
|----------------------|---------------------------------|
|`FORMAT(date, 'yyyy-MM')`|Formats date to specific string (SQL Server)|
|`DATE_FORMAT(date, '%Y-%m')`|MySQL formatting|
| `TO_CHAR()`        | Format date/time as string (PostgreSQL)   |

**Example:**

```sql
SELECT 
    DATE_FORMAT(order_date, '%Y-%m') AS order_month  -- Output: 2025-06
    TO_CHAR(NOW(), 'YYYY-MM-DD') AS NOW              -- 2025-06-19
    DATE_FORMAT('2025-06-18', '%b') AS month_short   -- Jun
FROM orders;
```

---

## 5. Conversion Functions

| Function      | Description                          | Example                            |
|---------------|--------------------------------------|-------------------------------------|
| `CAST()`      | Convert between data types           | `CAST(123 AS VARCHAR)`             |
| `CONVERT()`   | Like `CAST()` (SQL Server)           | `CONVERT(VARCHAR, 123)`            |
| `TO_CHAR()`   | Convert date/number to text          | `TO_CHAR(salary, '9999.99')`       |

---

## 6. Null Handling Functions

| Function          | Description                              | Example                              |
|-------------------|------------------------------------------|---------------------------------------|
| `COALESCE()`      | Return first non-null value              | `COALESCE(phone, 'N/A')`             |
| `NULLIF()`        | Returns NULL if two values are equal     | `NULLIF(score1, score2)`             |
| `IS NULL` / `IS NOT NULL` | Check for null                   | `email IS NULL`                      |

---

## Example Query

```sql
SELECT 
  department,
  COUNT(*) AS headcount,
  ROUND(AVG(salary), 2) AS avg_salary,
  MAX(UPPER(job_title)) AS highest_title
FROM employees
WHERE hire_date IS NOT NULL
GROUP BY department
HAVING COUNT(*) > 5
ORDER BY avg_salary DESC;
```


---

# Thanks for reading!

#### Written by @hellorito