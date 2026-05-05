# SQL Activities

## Activity 1 — Retrieve Data with Basic SELECT Queries

> **Environment:** Relational database (MariaDB shell)  
> **Tool:** SQL (Structured Query Language) — used to interact with and retrieve data stored in relational databases

---

### Clauses used

| Clause / Keyword | Purpose |
|---|---|
| `SELECT` | Specify which columns to retrieve from a table. Only the listed columns are included in the result |
| `SELECT *` | Retrieve all columns from a table. The `*` wildcard acts as a shorthand for every column without naming them individually |
| `FROM` | Specify the table to query. Always used together with `SELECT` to indicate the data source |
| `ORDER BY` | Sort the result rows by one or more columns. By default the sort is ascending (`ASC`); append `DESC` to reverse the order |

---

### Queries used

| Query | Purpose |
|---|---|
| `SELECT employee_id, device_id FROM employees;` | Retrieve only the `employee_id` and `device_id` columns from the `employees` table |
| `SELECT * FROM employees;` | Retrieve every column from the `employees` table |
| `SELECT * FROM log_in_attempts;` | Retrieve every column from the `log_in_attempts` table, listing all recorded login events |
| `SELECT * FROM log_in_attempts ORDER BY login_date;` | Retrieve all login attempts and sort the results by `login_date` in ascending order (oldest first) |
| `SELECT * FROM log_in_attempts ORDER BY login_date DESC;` | Retrieve all login attempts and sort the results by `login_date` in descending order (most recent first). `DESC` reverses the default ascending sort |
| `SELECT * FROM log_in_attempts ORDER BY login_date, login_time;` | Sort results first by `login_date` and then by `login_time` within each date, applying multiple sort criteria in sequence |

---
