# Debugging Python Code — Security Automation

> **Document type:** Python debugging practical lab  
> **Context:** Google Cybersecurity Certificate — Automate Tasks with Python module  
> **Skills demonstrated:** Identifying syntax errors, logic errors, and exceptions; `for` loops, conditionals, list indexing, string methods, file I/O

---

## Overview

This lab presents seven buggy code snippets in a security context. For each task, the error type is identified and the fix is applied. Bugs fall into three categories:

| Error type | What it is | Example |
|---|---|---|
| **Syntax error** | Code violates Python grammar — interpreter can't parse it | Missing `:`, wrong operator |
| **Exception** | Code is syntactically valid but fails at runtime | `IndexError`, `AttributeError`, `NameError` |
| **Logic error** | Code runs without error but produces the wrong output | Wrong list index |

---

## Task 1 — Missing Colon in `for` Loop

**Bug type:** Syntax error

```python
# BUGGY
for i in range(10)
    print("Connection cannot be established")
```

**Problem:** The `for` statement is missing its required trailing colon.

```python
# FIXED
for i in range(10):
    print("Connection cannot be established")
```

---

## Task 2 — Missing Comma in List

**Bug type:** Syntax error

```python
# BUGGY
usernames_list = ["djames", "jpark", "tbailey", "zdutchma" "esmith", "srobinso", "dcoleman", "fbautist"]
print(usernames_list)
```

**Problem:** `"zdutchma"` and `"esmith"` are adjacent string literals without a comma. Python silently concatenates them into `"zdutchmaesmith"`, which is not the intended behavior and in stricter contexts raises a syntax warning.

```python
# FIXED
usernames_list = ["djames", "jpark", "tbailey", "zdutchma", "esmith", "srobinso", "dcoleman", "fbautist"]
print(usernames_list)
```

---

## Task 3 — Method Called on Wrong Object

**Bug type:** Syntax error / AttributeError

```python
# BUGGY
print("update needed").upper()
```

**Problem:** `.upper()` is called on the return value of `print()`, which is `None`. Calling a string method on `None` raises `AttributeError: 'NoneType' object has no attribute 'upper'`. The method must be applied to the string **before** it is passed to `print`.

```python
# FIXED
print("update needed".upper())
```

**Output:** `UPDATE NEEDED`

---

## Task 4 — Three Bugs: Wrong Variable Name, Assignment in Condition, Missing Indentation

**Bug type:** NameError (exception) + 2× syntax error

```python
# BUGGY
usernames_list = ["djames", "jpark", "tbailey", "zdutchma", "esmith", "srobinso", "dcoleman", "fbautist"]
username = "esmith"

for name in username_list:          # BUG 1: variable name typo
    if name = username:             # BUG 2: = instead of ==
    print("The user is an approved user")  # BUG 3: not indented under if
```

**Bugs:**
1. `username_list` → `usernames_list` — the variable was defined with an `s`; using the wrong name raises `NameError`.
2. `if name = username:` → `if name == username:` — a single `=` is assignment, not comparison; Python raises `SyntaxError` here.
3. The `print` is not indented under the `if` — it falls outside the conditional and runs on every iteration regardless of the match.

```python
# FIXED
usernames_list = ["djames", "jpark", "tbailey", "zdutchma", "esmith", "srobinso", "dcoleman", "fbautist"]
username = "esmith"

for name in usernames_list:
    if name == username:
        print("The user is an approved user")
```

---

## Task 5 — Index Out of Range

**Bug type:** IndexError (exception)

```python
# BUGGY
usernames_list = ["elarson", "bmoreno", "tshah", "sgilmore", "eraab"]
username = "eraab"

if username == usernames_list[5]:
    print("This username is the final one in the list.")
```

**Problem:** The list has 5 elements at indices `0` through `4`. Accessing `[5]` raises `IndexError: list index out of range`. The last element is at index `4` — or using `-1` to always refer to the last element regardless of list length.

```python
# FIXED
usernames_list = ["elarson", "bmoreno", "tshah", "sgilmore", "eraab"]
username = "eraab"

if username == usernames_list[-1]:
    print("This username is the final one in the list.")
```

> Using `[-1]` is preferred over `[4]` because it works correctly even if the list length changes.

---

## Task 6 — Missing Colon on `with` Statement and Reversed Method Call

**Bug type:** Syntax error + exception

```python
# BUGGY
import_file = "allow_list.txt"
remove_list = ["192.168.97.225", "192.168.158.170", "192.168.201.40", "192.168.58.57"]

with open(import_file, "r") as file      # BUG 1: missing colon
    ip_addresses = file.read()

ip_addresses = split.ip_addresses()      # BUG 2: method and object reversed

for element in remove_list:
    if element in ip_addresses:
        ip_addresses.remove(element)

print(ip_addresses)
```

**Bugs:**
1. `with open(import_file, "r") as file` is missing the `:` at the end — `SyntaxError`.
2. `split.ip_addresses()` — the object and method are reversed. `split` is not a standalone function; `.split()` is a string method called on `ip_addresses`.

```python
# FIXED
import_file = "allow_list.txt"
remove_list = ["192.168.97.225", "192.168.158.170", "192.168.201.40", "192.168.58.57"]

with open(import_file, "r") as file:
    ip_addresses = file.read()

ip_addresses = ip_addresses.split()

for element in remove_list:
    if element in ip_addresses:
        ip_addresses.remove(element)

print(ip_addresses)
```

---

## Task 7 — Wrong List Indices (Logic Errors)

**Bug type:** Logic error — code runs but produces wrong output

```python
# BUGGY
system = "OS 2"
patch_schedule = ["March 1st", "April 1st", "May 1st"]

if system == "OS 1":
    print("Patch date:", patch_schedule[2])   # BUG: prints "May 1st" instead of "March 1st"

elif system == "OS 2":
    print("Patch date:", patch_schedule[0])   # BUG: prints "March 1st" instead of "April 1st"

elif system == "OS 3":
    print("Patch date:", patch_schedule[2])   # correct
```

**Problem:** The indices are mismatched. `patch_schedule[0]` = `"March 1st"`, `[1]` = `"April 1st"`, `[2]` = `"May 1st"`. OS 1 and OS 2 are pointing to the wrong dates.

| OS | Correct index | Correct date |
|---|---|---|
| OS 1 | `[0]` | March 1st |
| OS 2 | `[1]` | April 1st |
| OS 3 | `[2]` | May 1st |

```python
# FIXED
system = "OS 2"
patch_schedule = ["March 1st", "April 1st", "May 1st"]

if system == "OS 1":
    print("Patch date:", patch_schedule[0])

elif system == "OS 2":
    print("Patch date:", patch_schedule[1])

elif system == "OS 3":
    print("Patch date:", patch_schedule[2])
```

**Output for `system = "OS 2"`:** `Patch date: April 1st`

---

## Error Type Reference

| Error | When it occurs | How to find it |
|---|---|---|
| `SyntaxError` | Before the code runs — Python can't parse the file | Check the line the interpreter points to: missing `:`, `==` vs `=`, unmatched parentheses |
| `NameError` | Variable or function used before being defined or with a typo | Check variable names for spelling; Python is case-sensitive |
| `IndexError` | List accessed at an index that doesn't exist | Check list length vs the index used; use `[-1]` for last element |
| `AttributeError` | Method called on the wrong type (e.g., `None.upper()`) | Check what the expression returns before the `.method()` call |
| Logic error | No error raised, but output is wrong | Trace through the code manually; test with known inputs |

---

*Document created as part of a professional cybersecurity portfolio.*
