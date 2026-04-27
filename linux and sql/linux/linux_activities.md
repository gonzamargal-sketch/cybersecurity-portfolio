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
