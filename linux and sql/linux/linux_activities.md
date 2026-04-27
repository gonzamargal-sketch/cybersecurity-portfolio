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
