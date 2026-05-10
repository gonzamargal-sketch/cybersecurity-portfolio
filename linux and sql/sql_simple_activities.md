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
