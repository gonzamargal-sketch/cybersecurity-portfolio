# Regular Expressions in Python — Security Pattern Matching

> **Document type:** Concept reference and practical examples  
> **Context:** Google Cybersecurity Certificate — Automate Tasks with Python module  
> **Skills demonstrated:** Python regex, `re` module, `re.findall()`, pattern construction for security use cases (IP addresses, device IDs, usernames, login data)

---

## Setup

```python
import re
```

Always required before using any regex function. The main function used is:

```python
re.findall(pattern, string)  # returns a list of all matches
```

---

## Character Type Symbols

These symbols each match **one character** of a specific type:

| Symbol | Matches |
|---|---|
| `\w` | Any alphanumeric character (A-Z, a-z, 0-9) **or** underscore `_` |
| `\d` | Any single digit (0-9) |
| `\s` | Any whitespace character (space, tab, newline) |
| `.` | Any character except newline |
| `\.` | A literal period `.` — backslash escapes its special meaning |

```python
re.findall("\w", "h32rb17")   # ['h','3','2','r','b','1','7']
re.findall("\d", "h32rb17")   # ['3','2','1','7']
```

---

## Quantity Symbols

Place these **after** a character or type symbol to control how many repetitions to match:

| Symbol | Repetitions | Example |
|---|---|---|
| `+` | 1 or more | `\d+` matches `"32"`, `"7"`, `"825"` |
| `*` | 0 or more (includes empty strings) | `\d*` matches digits and empty positions |
| `{n}` | Exactly n | `\d{2}` matches exactly two consecutive digits |
| `{m,n}` | Between m and n (inclusive) | `\d{1,3}` matches 1, 2, or 3 consecutive digits |

```python
re.findall("\d+",    "h32rb17")              # ['32', '17']
re.findall("\d{2}",  "h32rb17 k825t0m")     # ['32', '17', '82']
re.findall("\d{1,3}","h32rb17 k825t0m c2994eh")  # ['32', '17', '825', '0', '299', '4']
```

> **Note:** Python scans left-to-right. When three digits appear in a row (e.g. `123`), `\d{2}` matches `12` and restarts from `3`.

---

## Constructing a Pattern

Break the target pattern into parts, assign a symbol to each, then combine:

**Goal:** extract `username: login_attempts` from an employee log string.

```python
import re

pattern = "\w+:\s\d+"
employee_logins_string = "1001 bmoreno: 12 Marketing 1002 tshah: 7 Human Resources 1003 sgilmore: 5 Finance"

print(re.findall(pattern, employee_logins_string))
# ['bmoreno: 12', 'tshah: 7', 'sgilmore: 5']
```

**Pattern breakdown:**

| Component | Symbol | Matches |
|---|---|---|
| Username (variable length) | `\w+` | One or more alphanumeric characters |
| Colon separator | `:` | Literal colon |
| Space | `\s` | One whitespace character |
| Login attempts (variable) | `\d+` | One or more digits |

---

## Quick Reference — All Symbols on the Same String

Using `"a53-32c .E"` as the test string to compare outputs side by side:

```python
import re
s = "a53-32c .E"

re.findall("a53", s)   # ['a53']               — literal match (no special symbols)
re.findall("\w",  s)   # ['a','5','3','3','2','c','E']
re.findall("\d",  s)   # ['5','3','3','2']
re.findall("\s",  s)   # [' ']
re.findall(".",   s)   # ['a','5','3','-','3','2','c',' ','.','E']
re.findall("\.",  s)   # ['.']

re.findall("\w+", s)   # ['a53', '32c', 'E']          — groups of alphanumerics
re.findall("\w*", s)   # ['a53', '', '32c', '', '', 'E', '']  — includes zero-length matches
re.findall("\w{3}", s) # ['a53', '32c']                — exactly 3 alphanumeric chars
```

> The difference between `+` and `*` is visible here: `\w+` skips non-alphanumeric characters entirely, while `\w*` also matches the empty string at each non-alphanumeric position.

---

## Security Use Cases

| Target | Pattern | Example match |
|---|---|---|
| Device ID (alphanumeric) | `\w+` | `h32rb17` |
| IP address octet | `\d{1,3}` | `192`, `168`, `1` |
| Username with digits | `\w+` | `tshah`, `bmoreno` |
| Log timestamp digits | `\d{2}:\d{2}:\d{2}` | `12:38:34` |
| File extension | `\.\w+` | `.exe`, `.log`, `.pcap` |

---

*Document created as part of a professional cybersecurity portfolio.*
