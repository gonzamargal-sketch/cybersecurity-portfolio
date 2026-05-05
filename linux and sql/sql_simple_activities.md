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
