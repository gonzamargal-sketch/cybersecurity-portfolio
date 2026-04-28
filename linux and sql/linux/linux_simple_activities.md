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
| `ls -l` | List directory contents in long format, displaying file permissions, owner, size, and modification date |
| `ls -a` | List all files including hidden files (those whose names start with `.`) |
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
