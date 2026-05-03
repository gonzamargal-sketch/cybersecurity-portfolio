# Linux Activities

## Activity 1 — Install and Manage Applications with APT

> **Environment:** Linux Bash shell (Debian-based)  
> **Tool:** APT (Advanced Package Tool) — the default package manager for Debian-based Linux distributions

---

### Commands used

| Command | Purpose |
|---|---|
| `apt` | Verify that the APT package manager is installed and display basic usage information |
| `sudo apt install suricata` | Install the Suricata network analysis and intrusion detection application. `sudo` is required to grant elevated privileges for software installation |
| `suricata` | Verify that Suricata was installed correctly by running the application and checking for version and usage output |
| `sudo apt remove suricata` | Uninstall the Suricata application. `sudo` is required to grant elevated privileges for software removal |
| `sudo apt install tcpdump` | Install tcpdump, a command-line tool for capturing network traffic on a Linux Bash shell |
| `apt list --installed` | Display a list of all applications currently installed on the system, used to confirm which packages are present and their versions |

---

## Activity 2 — Input and Output in the Bash Shell

> **Environment:** Linux Bash shell  
> **Tools:** `echo`, `expr`, `clear` — built-in Bash commands for generating output, performing arithmetic, and managing the terminal display

---

### Commands used

| Command | Purpose |
|---|---|
| `echo hello` | Print the string `hello` to the shell output |
| `echo "hello"` | Print the same string using quotes, which group a series of characters and prevent misinterpretation of special characters |
| `echo "name"` | Print a custom name string to the shell output |
| `expr 32 - 8` | Calculate the number of false positives by subtracting 8 actionable alerts from 32 total alerts, returning `24` |
| `expr 3500 * 12` | Calculate the expected total yearly access attempts by multiplying the monthly average (3,500) by 12, returning `42000` |
| `clear` | Clear all previous input and output from the Bash shell, returning the cursor to the top-left of the terminal window |

---

## Activity 3 — Navigate the File System

> **Environment:** Linux Bash shell  
> **Tools:** `cd`, `pwd`, `ls`, `cat`, `head` — built-in Bash commands for navigating directories and reading file contents

---

### Commands used

| Command | Purpose |
|---|---|
| `pwd` | Print the current working directory, showing the absolute path of the directory you are in |
| `ls` | List the files and subdirectories inside the current directory |
| `cd /home/analyst` | Change the current working directory to `/home/analyst` using an absolute path |
| `cd logs` | Change into the `logs` subdirectory using a relative path |
| `cd ..` | Move up one level to the parent directory |
| `cat updates.txt` | Display the full contents of the file `updates.txt` in the terminal |
| `head updates.txt` | Display only the first 10 lines of `updates.txt`, useful for previewing large files without reading them entirely |
| `head -n 5 updates.txt` | Display the first 5 lines of `updates.txt`, using `-n` to specify the number of lines |

---

## Activity 4 — Search Files and Filter Output

> **Environment:** Linux Bash shell  
> **Tools:** `grep`, `|`, `find` — built-in Bash commands for searching content inside files, chaining commands, and locating files in the file system

---

### Commands used

| Command | Purpose |
|---|---|
| `grep "error" server_logs.txt` | Search the file `server_logs.txt` for lines containing the string `error` and print only those lines |
| `grep -i "error" server_logs.txt` | Same search using `-i` to make it case-insensitive, matching `error`, `Error`, `ERROR`, etc. |
| `grep -r "failed" /home/analyst/logs` | Recursively search all files under `/home/analyst/logs` for lines containing `failed` |
| `grep -n "root" passwd.txt` | Search `passwd.txt` for lines containing `root` and display the line numbers alongside the matches |
| `grep -c "error" server_logs.txt` | Count the number of lines in `server_logs.txt` that contain the string `error` |
| `ls /home/analyst/reports \| grep "users"` | List the files in the `reports` directory and pipe the output to `grep` to filter and show only filenames that contain `users`. The pipe `\|` passes the output of the first command as input to the second |
| `cat server_logs.txt \| grep "error"` | Display the contents of `server_logs.txt` and pipe the output to `grep` to filter and show only lines containing `error` |
| `find /home/analyst -name "*.txt"` | Search the `/home/analyst` directory and all its subdirectories for files whose names end in `.txt` |
| `find /home/analyst -type d` | Search under `/home/analyst` for items of type directory (`d`), listing all subdirectories found |
| `find /home/analyst -mtime -3` | Find all files under `/home/analyst` that were modified within the last 3 days, useful for identifying recently changed files |

---

## Activity 5 — Manage Files and Directories

> **Environment:** Linux Bash shell  
> **Tools:** `mkdir`, `rmdir`, `touch`, `rm`, `mv`, `cat`, `cp` — built-in Bash commands for creating, deleting, moving, and editing files and directories

---

### Commands used

| Command | Purpose |
|---|---|
| `mkdir logs` | Create a new directory named `logs` inside the current working directory |
| `mkdir -p reports/2024/april` | Create a nested directory structure in one command. `-p` creates all intermediate directories that do not yet exist |
| `rmdir logs` | Remove an empty directory named `logs`. Fails if the directory contains any files |
| `touch report.txt` | Create a new empty file named `report.txt`. If the file already exists, it updates its last-modified timestamp without changing the content |
| `rm report.txt` | Delete the file `report.txt` permanently. This action cannot be undone |
| `rm -r old_reports` | Recursively delete the directory `old_reports` and all its contents. `-r` is required to remove non-empty directories |
| `mv report.txt /home/analyst/reports` | Move the file `report.txt` to the `/home/analyst/reports` directory |
| `mv report.txt summary.txt` | Rename the file `report.txt` to `summary.txt` by moving it to the same location with a new name |
| `cp report.txt /home/analyst/backup` | Copy the file `report.txt` to the `/home/analyst/backup` directory, leaving the original file in place |
| `cp -r logs /home/analyst/backup` | Recursively copy the entire `logs` directory and its contents to `/home/analyst/backup`. `-r` is required to copy directories |
| `cat report.txt` | Display the full contents of `report.txt` in the terminal |
| `cat file1.txt file2.txt > combined.txt` | Concatenate the contents of `file1.txt` and `file2.txt` and write the result into a new file `combined.txt` using output redirection (`>`) |

---

## Activity 6 — Manage File and Directory Permissions (Authorization)

> **Environment:** Linux Bash shell  
> **Tools:** `ls -l`, `ls -la`, `chmod` — built-in Bash commands for inspecting and modifying file and directory permissions
>
> **Concept:** This activity focuses on **authorization** — determining what resources a user is allowed to access after their identity has already been established. In Linux, authorization is enforced through file and directory permissions that control which users and groups can read, write, or execute each resource. This is distinct from **authentication** (covered in the next activity), which focuses on verifying who the user is.

---

### Permission string format

Each file or directory entry returned by `ls -l` begins with a 10-character permission string, for example `drwxrwxrwx`:

| Position | Meaning |
|---|---|
| 1 | File type: `d` = directory, `-` = regular file |
| 2–4 | **User** (owner) permissions: `r` read, `w` write, `x` execute |
| 5–7 | **Group** permissions: `r` read, `w` write, `x` execute |
| 8–10 | **Other** (everyone else) permissions: `r` read, `w` write, `x` execute |

A `-` in any position means that permission is not granted.

---

### Commands used

| Command | Purpose |
|---|---|
| `ls -l` | List directory contents in long format, showing the permission string, owner, group, size, and modification date of each file |
| `ls -la` | Same as `ls -l` but also includes hidden files (names starting with `.`). Used to audit all files, including archived or configuration files |
| `chmod o-w project_k.txt` | Remove write permission for **other** on `project_k.txt`. `o` targets the other owner type, `-` removes the permission, `w` is write |
| `chmod g-r project_m.txt` | Remove read permission for the **group** on `project_m.txt` |
| `chmod u-w,g-w,g+r .project_x.txt` | Remove write for user and group, and ensure group has read, on the hidden file `.project_x.txt`. Multiple changes can be combined with a comma |
| `chmod g-x drafts` | Remove execute permission for the **group** on the `drafts` directory. Execute on a directory controls the ability to enter it and access its contents |

---

### `chmod` operator reference

| Symbol | Meaning |
|---|---|
| `u` | User (file owner) |
| `g` | Group |
| `o` | Other (everyone else) |
| `+` | Grant the permission |
| `-` | Remove the permission |
| `r` | Read |
| `w` | Write |
| `x` | Execute |

---

## Activity 7 — Manage Users and Groups (User Management)

> **Environment:** Linux Bash shell  
> **Tools:** `useradd`, `usermod`, `chown`, `userdel`, `groupdel` — built-in Linux commands for managing user accounts, group membership, and file ownership
>
> **Concept:** This activity focuses on **user management** — the process of creating, modifying, and deleting user accounts and groups in the system. User management is distinct from both **authentication** (verifying who a user is, e.g. via passwords or SSH keys) and **authorization** (what a user is allowed to access, covered in the previous activity). However, it underpins both: without properly managed accounts and group memberships, neither authentication nor authorization can be enforced correctly. As a security analyst, managing the user lifecycle — onboarding, role changes, and offboarding — is a key part of maintaining a secure system.

---

### Commands used

| Command | Purpose |
|---|---|
| `sudo useradd researcher9` | Create a new user account named `researcher9` on the system. `sudo` is required to grant elevated privileges for user management. Linux automatically creates a group with the same name as the user |
| `sudo usermod -g research_team researcher9` | Set `research_team` as the **primary group** of `researcher9`. `-g` (lowercase) modifies the primary group, which is the default group assigned to files created by this user |
| `sudo chown researcher9 /home/researcher2/projects/project_r.txt` | Transfer ownership of `project_r.txt` to `researcher9`. The new owner gains user-level permissions on the file |
| `sudo usermod -a -G sales_team researcher9` | Add `researcher9` to `sales_team` as a **supplementary group**, while keeping `research_team` as the primary group. `-a` appends without removing existing groups; `-G` (uppercase) specifies the supplementary group. Omitting `-a` would replace all existing supplementary groups |
| `sudo userdel researcher9` | Delete the user account `researcher9` from the system. The user's primary group (auto-created at account creation) is not removed automatically if it has no other members |
| `sudo groupdel researcher9` | Delete the now-empty group `researcher9` that was automatically created when the user was added. Removing leftover empty groups is a security best practice to keep the system clean |

---

## Activity 8 — Get Help in the Linux Shell

> **Environment:** Linux Bash shell  
> **Tools:** `whatis`, `man`, `apropos` — built-in Linux commands for accessing documentation and discovering commands directly from the shell

---

### Commands used

| Command | Purpose |
|---|---|
| `whatis cat` | Display a single-line description of the `cat` command. Useful for a quick reminder of what a command does without opening the full manual |
| `man cat` | Open the full manual page for `cat`, including a detailed description and all available options. Press `ENTER` to scroll line by line, `SPACE` to go to the next page, and `Q` to exit |
| `apropos -a first part file` | Search manual page descriptions for commands matching all the supplied keywords. `-a` requires every keyword to appear in the description, narrowing results. Useful when you know what you want to do but not which command to use |

---
