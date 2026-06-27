# Importing and Parsing Files in Python — Security Log Analysis

> **Document type:** Python file handling practical lab  
> **Context:** Google Cybersecurity Certificate — Automate Tasks with Python module  
> **Skills demonstrated:** Python file I/O, `with` statement, `open()` modes, `.read()`, `.write()`, `.split()`, security log parsing, IP allowlist management

---

## Overview

This lab covers reading, parsing, and writing files in Python using a security context: importing a login log file, appending missing entries, and creating an IP address allowlist. All file operations use the `with` statement to ensure files are automatically closed after use.

---

## Task 1: Open a Security Log File

```python
import_file = "login.txt"

with open(import_file, "r") as file:
```

The `with` statement opens the file and automatically closes it when the block ends. The `"r"` parameter sets the file to **read-only** mode.

---

## Task 2: Read the File and Display Its Contents

```python
import_file = "login.txt"

with open(import_file, "r") as file:
    text = file.read()

print(text)
```

`.read()` loads the entire file content into a single string stored in `text`. The `print(text)` call displays the full log as one block of text.

---

## Task 3: Split the File Content into Lines

```python
import_file = "login.txt"

with open(import_file, "r") as file:
    text = file.read()

print(text.split())
```

`.split()` breaks the single string into a **list**, with each line as a separate element. This makes individual log entries easier to iterate over and analyze.

> Note: `.split()` does not modify `text` — it returns a new list. To store the result, reassign: `text = text.split()`.

---

## Task 4: Append a Missing Entry to the Log File

```python
import_file = "login.txt"
missing_entry = "jrafael,192.168.243.140,4:56:27,2022-05-09"

with open(import_file, "a") as file:
    file.write(missing_entry)

with open(import_file, "r") as file:
    text = file.read()

print(text)
```

The `"a"` (append) mode adds content to the **end** of the existing file without overwriting it. After writing, the file is reopened in `"r"` mode to verify the entry was added correctly.

---

## Task 5: Define a New File and IP Address List

```python
import_file = "allow_list.txt"

ip_addresses = "192.168.218.160 192.168.97.225 192.168.145.158 192.168.108.13 192.168.60.153 192.168.96.200 192.168.247.153 192.168.3.252 192.168.116.187 192.168.15.110 192.168.39.246"

print(import_file)
print(ip_addresses)
```

Two variables are set up: the target filename and a space-separated string of allowed IP addresses. Displaying them confirms the values before writing.

---

## Task 6: Write the IP Addresses to the File

```python
import_file = "allow_list.txt"
ip_addresses = "192.168.218.160 192.168.97.225 192.168.145.158 192.168.108.13 192.168.60.153 192.168.96.200 192.168.247.153 192.168.3.252 192.168.116.187 192.168.15.110 192.168.39.246"

with open(import_file, "w") as file:
    file.write(ip_addresses)
```

The `"w"` (write) mode creates the file if it doesn't exist, or **overwrites** it if it does. `.write()` stores the IP address string directly into the file.

---

## Task 7: Read Back the Allowlist File

```python
import_file = "allow_list.txt"
ip_addresses = "192.168.218.160 192.168.97.225 192.168.145.158 192.168.108.13 192.168.60.153 192.168.96.200 192.168.247.153 192.168.3.252 192.168.116.187 192.168.15.110 192.168.39.246"

with open(import_file, "w") as file:
    file.write(ip_addresses)

with open(import_file, "r") as file:
    text = file.read()

print(text)
```

The file is read back and displayed to verify the write operation was successful. The output matches the original `ip_addresses` string.

---

## Key Concepts

### The `with` Statement

```python
with open(filename, mode) as file:
    # file operations here
```

Automatically closes the file when the block exits, even if an error occurs. This is the recommended pattern for all file I/O in Python.

### File Opening Modes

| Mode | Behavior |
|---|---|
| `"r"` | Read — file must exist |
| `"w"` | Write — creates the file or **overwrites** existing content |
| `"a"` | Append — creates the file or adds to the **end** of existing content |

### Key File Methods

| Method | What it does |
|---|---|
| `.read()` | Returns the entire file content as a single string |
| `.write(string)` | Writes the string to the file |
| `.split()` | Splits a string into a list (default separator: whitespace/newline) |

### Security Use Cases

| Task | Mode | Use case |
|---|---|---|
| Read a log file | `"r"` | Analyze login attempts, parse events |
| Append to a log | `"a"` | Add missing or new entries without losing existing data |
| Create an allowlist | `"w"` | Write or refresh a list of permitted IP addresses |
| Parse log entries | `.split()` | Separate individual records for iteration or filtering |

---

*Document created as part of a professional cybersecurity portfolio.*
