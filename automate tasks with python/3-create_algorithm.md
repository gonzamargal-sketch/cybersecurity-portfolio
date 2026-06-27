# Creating an Algorithm — IP Allowlist Update

> **Document type:** Python algorithm development lab  
> **Context:** Google Cybersecurity Certificate — Automate Tasks with Python module  
> **Skills demonstrated:** File I/O, list manipulation, `for` loops, conditional statements, `.split()`, `.join()`, `.remove()`, function definition, security automation

---

## Scenario

A text file `allow_list.txt` stores IP addresses permitted to access restricted information. Some of those IPs must be revoked. The goal is to build a Python algorithm that reads the allowlist, removes the flagged IPs, and writes the updated list back to the file — then wrap it all into a reusable function.

```python
import_file = "allow_list.txt"
remove_list = ["192.168.97.225", "192.168.158.170", "192.168.201.40", "192.168.58.57"]
```

---

## Task 1: Set Up the Variables

```python
import_file = "allow_list.txt"
remove_list = ["192.168.97.225", "192.168.158.170", "192.168.201.40", "192.168.58.57"]

print(import_file)
print(remove_list)
```

Displays both variables to confirm their contents before operating on them.

---

## Task 2: Open the File

```python
with open(import_file, "r") as file:
```

Opens `allow_list.txt` in read mode. The `with` statement ensures the file closes automatically when the block ends.

---

## Task 3: Read the File into a Variable

```python
with open(import_file, "r") as file:
    ip_addresses = file.read()

print(ip_addresses)
```

`.read()` loads the entire file as a single string into `ip_addresses`. The output at this point is one long string with all IPs separated by spaces.

---

## Task 4: Convert the String to a List

```python
with open(import_file, "r") as file:
    ip_addresses = file.read()

ip_addresses = ip_addresses.split()

print(ip_addresses)
```

`.split()` breaks the string at each whitespace and returns a list — one IP address per element. The variable is reassigned so the change is stored. This makes it possible to iterate over individual IPs and use `.remove()`.

---

## Task 5: Build the Iterative Statement

```python
with open(import_file, "r") as file:
    ip_addresses = file.read()

ip_addresses = ip_addresses.split()

for element in ip_addresses:
    print(element)
```

A `for` loop iterates through each IP address in the list. At this stage it just prints each element — the removal logic is added in the next task.

---

## Task 6: Add the Conditional Removal

```python
with open(import_file, "r") as file:
    ip_addresses = file.read()

ip_addresses = ip_addresses.split()

for element in ip_addresses:
    if element in remove_list:
        ip_addresses.remove(element)

print(ip_addresses)
```

Inside the loop, an `if` statement checks whether the current element is in `remove_list`. If it is, `.remove()` deletes that element from `ip_addresses`. After the loop, the list contains only the IPs that are still allowed.

---

## Task 7: Convert Back to String and Rewrite the File

```python
with open(import_file, "r") as file:
    ip_addresses = file.read()

ip_addresses = ip_addresses.split()

for element in ip_addresses:
    if element in remove_list:
        ip_addresses.remove(element)

ip_addresses = " ".join(ip_addresses)

with open(import_file, "w") as file:
    file.write(ip_addresses)
```

`.join()` converts the list back to a space-separated string — required because `.write()` only accepts strings. The file is then reopened in `"w"` mode to overwrite its contents with the updated allowlist.

**Why `.join()` works this way:**
```python
" ".join(["192.168.1.1", "192.168.1.2"])
# "192.168.1.1 192.168.1.2"
```
The string `" "` is the separator inserted between each element of the iterable.

---

## Task 8: Verify the Updated File

```python
with open(import_file, "r") as file:
    ip_addresses = file.read()

ip_addresses = ip_addresses.split()

for element in ip_addresses:
    if element in remove_list:
        ip_addresses.remove(element)

ip_addresses = " ".join(ip_addresses)

with open(import_file, "w") as file:
    file.write(ip_addresses)

with open(import_file, "r") as file:
    text = file.read()

print(text)
```

The file is read back into `text` and displayed to confirm the revoked IPs no longer appear in `allow_list.txt`.

---

## Task 9: Wrap Everything into a Function

```python
def update_file(import_file, remove_list):

    with open(import_file, "r") as file:
        ip_addresses = file.read()

    ip_addresses = ip_addresses.split()

    for element in ip_addresses:
        if element in remove_list:
            ip_addresses.remove(element)

    ip_addresses = " ".join(ip_addresses)

    with open(import_file, "w") as file:
        file.write(ip_addresses)
```

All the steps are encapsulated in a reusable function `update_file()` that takes two parameters:
- `import_file` — the path to the allowlist file
- `remove_list` — a list of IP addresses to revoke

Defining the function does not produce output — it only stores the logic for later use.

---

## Task 10: Call the Function and Verify

```python
def update_file(import_file, remove_list):

    with open(import_file, "r") as file:
        ip_addresses = file.read()

    ip_addresses = ip_addresses.split()

    for element in ip_addresses:
        if element in remove_list:
            ip_addresses.remove(element)

    ip_addresses = " ".join(ip_addresses)

    with open(import_file, "w") as file:
        file.write(ip_addresses)


update_file("allow_list.txt", ["192.168.25.60", "192.168.140.81", "192.168.203.198"])

with open("allow_list.txt", "r") as file:
    text = file.read()

print(text)
```

The function is called with `"allow_list.txt"` and a new set of IPs to remove. The file is then read back to verify the result.

---

## Final Algorithm — Complete Code

```python
def update_file(import_file, remove_list):
    """Removes revoked IP addresses from an allowlist file."""

    with open(import_file, "r") as file:
        ip_addresses = file.read()

    ip_addresses = ip_addresses.split()

    for element in ip_addresses:
        if element in remove_list:
            ip_addresses.remove(element)

    ip_addresses = " ".join(ip_addresses)

    with open(import_file, "w") as file:
        file.write(ip_addresses)
```

---

## Key Concepts

### Algorithm Steps

| Step | Operation | Purpose |
|---|---|---|
| 1 | `open(file, "r")` + `.read()` | Load allowlist into memory as a string |
| 2 | `.split()` | Convert string to list for iteration |
| 3 | `for` + `if in` + `.remove()` | Remove revoked IPs |
| 4 | `" ".join()` | Convert list back to string |
| 5 | `open(file, "w")` + `.write()` | Overwrite file with updated allowlist |

### `.join()` vs `.split()`

| Method | Direction | Example |
|---|---|---|
| `.split()` | String → List | `"a b c".split()` → `["a","b","c"]` |
| `" ".join()` | List → String | `" ".join(["a","b","c"])` → `"a b c"` |

They are inverses of each other. The separator used in `.join()` should match the one `.split()` would use.

### `.remove()` Behavior

`.remove(element)` deletes the **first occurrence** of the element from a list and modifies it in place. If the element is not in the list, it raises a `ValueError` — the `if element in remove_list` guard prevents this.

---

*Document created as part of a professional cybersecurity portfolio.*
