# SQL Activities

## Activity 1 — Retrieve Data with Basic SELECT Queries

> **Environment:** Relational database (MariaDB shell)  
> **Tool:** SQL (Structured Query Language) — used to interact with and retrieve data stored in relational databases

---

| Clause / Keyword | Purpose |
|---|---|
| `SELECT` | Specify which columns to retrieve from a table. Only the listed columns are included in the result |
| `SELECT *` | Retrieve all columns from a table. The `*` wildcard acts as a shorthand for every column without naming them individually |
| `FROM` | Specify the table to query. Always used together with `SELECT` to indicate the data source |
| `ORDER BY` | Sort the result rows by one or more columns. By default the sort is ascending (`ASC`); append `DESC` to reverse the order |

---

## Activity 2 — Filter Query Results

> **Environment:** Relational database (MariaDB shell)  
> **Tool:** SQL (Structured Query Language)

---

| Clause / Keyword | Purpose |
|---|---|
| `DESCRIBE` | Display the structure of a table: column names, data types, and constraints. Useful for understanding a table before querying it |
| `WHERE` | Filter rows returned by a query, including only rows where the specified condition is true |
| `LIKE` | Used with `WHERE` to filter text columns by pattern matching. Combined with wildcard characters to match partial strings |
| `%` | Wildcard that matches zero or more characters in any position. For example, `'a%'` matches any value starting with `a`, and `'%a%'` matches any value containing `a` |
| `_` | Wildcard that matches exactly one character. For example, `'a_'` matches any two-character value starting with `a` |

---

## Activity 3 — Filter by Numeric and Date Ranges

> **Environment:** Relational database (MariaDB shell)  
> **Tool:** SQL (Structured Query Language)

---

| Clause / Keyword | Purpose |
|---|---|
| `>` | Greater than. Returns rows where the column value is strictly greater than the specified value |
| `>=` | Greater than or equal to. Returns rows where the column value is greater than or equal to the specified value |
| `<` | Less than. Returns rows where the column value is strictly less than the specified value |
| `<=` | Less than or equal to. Returns rows where the column value is less than or equal to the specified value |
| `BETWEEN` | Returns rows where the column value falls within a range, inclusive of both boundary values. Equivalent to `>= lower AND <= upper` |

---

## Activity 4 — Filter with Logical Operators

> **Environment:** Relational database (MariaDB shell)  
> **Tool:** SQL (Structured Query Language)

---

| Clause / Keyword | Purpose |
|---|---|
| `AND` | Combines two conditions and returns rows where both are true. Used to narrow results by requiring multiple criteria to be met simultaneously |
| `OR` | Combines two conditions and returns rows where at least one is true. Used to broaden results by accepting multiple valid values |
| `NOT` | Negates a condition and returns rows where the condition is false. Often combined with `LIKE` to exclude rows that match a pattern |
| Combined example | `SELECT * FROM log_in_attempts WHERE login_time > '18:00' AND (login_date = '2022-05-08' OR login_date = '2022-05-09') AND NOT country LIKE 'MEX%';` — retrieves failed after-hours login attempts on either of two dates that did not originate from Mexico |

---
## Activity 5 — Apply Filters to SQL Queries *(portfolio activity)*

> **Environment:** Relational database (MariaDB shell)  
> **Tool:** SQL (Structured Query Language)

---

```sql
-- Retrieve failed login attempts after business hours
SELECT * FROM log_in_attempts
WHERE login_time > '18:00:00' AND success = 0;

-- Retrieve login attempts on two specific dates
SELECT * FROM log_in_attempts
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';

-- Retrieve login attempts outside of Mexico
SELECT * FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';

-- Retrieve Marketing employees in the East building
SELECT * FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';

-- Retrieve employees in Finance or Sales
SELECT * FROM employees
WHERE department = 'Finance' OR department = 'Sales';

-- Retrieve all employees not in Information Technology
SELECT * FROM employees
WHERE NOT department = 'Information Technology';
```

---

## Activity 6 — Join Tables *(portfolio activity)*

> **Environment:** Relational database (MariaDB shell)  
> **Tool:** SQL (Structured Query Language)

---

| Clause / Keyword | Purpose |
|---|---|
| `INNER JOIN` | Returns only rows where there is a matching value in both tables on the join column. Records that have no match in either table are excluded |
| `LEFT JOIN` | Returns all rows from the left table (after `FROM`) and the matching rows from the right table. Rows from the left table with no match appear with `NULL` in the right table's columns |
| `RIGHT JOIN` | Returns all rows from the right table (after `JOIN`) and the matching rows from the left table. Rows from the right table with no match appear with `NULL` in the left table's columns |

---

## Activity 7 — Aggregate Functions

> **Environment:** Relational database (MariaDB shell)  
> **Tool:** SQL (Structured Query Language)

---

| Clause / Keyword | Purpose |
|---|---|
| `COUNT` | Returns the number of rows that match the query, excluding `NULL` values. Can be applied to any column |
| `AVG` | Returns the average of all non-`NULL` values in a numerical column |
| `SUM` | Returns the total sum of all non-`NULL` values in a numerical column |

---