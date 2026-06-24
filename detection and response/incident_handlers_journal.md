# Incident Handler's Journal

> **Document type:** Cumulative incident response journal  
> **Context:** Google Cybersecurity Certificate — Detection and Response module  
> **Skills demonstrated:** Incident documentation, the 5 W's framework, NIST IR lifecycle, tool usage logging, post-incident reflection

---

## How to Read This Journal

Each entry documents a security incident scenario analyzed during the course. Entries follow a consistent structure:

- **The 5 W's** — Who, What, Where, When, Why
- **Tools used** — any platforms, frameworks, or utilities applied
- **Additional notes** — reflections, open questions, or lessons learned

---

## Entries

---

### Entry #1

**Date:** 2026-06-22  
**Entry number:** 1  
**Description:** A small U.S. healthcare clinic suffered a ransomware attack that encrypted critical files and shut down business operations. The incident was initiated through phishing emails and resulted in the clinic being unable to access patient records or operate normally.

#### The 5 W's

| | |
|---|---|
| **Who** | An organized group of unethical hackers known to target organizations in the healthcare and transportation industries. |
| **What** | A ransomware attack that encrypted all company files and displayed a ransom note demanding payment in exchange for the decryption key, causing a complete shutdown of business operations. |
| **When** | Tuesday morning at approximately 9:00 a.m. |
| **Where** | A small U.S. health care clinic specializing in primary-care services. |
| **Why** | The attackers gained initial access by sending targeted phishing emails to employees. The emails contained malicious attachments that, once downloaded, installed malware and subsequently deployed ransomware across the network. |

#### Tools used

None — this entry documents the incident based on reported details. No technical analysis tools were used at this stage.

#### Additional notes

1. How could the health care company prevent an incident like this from occurring again?
2. Should the company pay the ransom to retrieve the decryption key?

---

### Entry #2

**Date:** 2026-06-24
**Entry number:** 2
**Description:** As a Level 1 SOC analyst at a financial services company, I investigated an alert triggered by a suspicious file downloaded on an employee's computer. The file was delivered via a phishing email disguised as a password-protected spreadsheet. Once opened, the file executed a malicious payload. I used VirusTotal to analyze the file's SHA256 hash and applied the Pyramid of Pain framework to identify associated IoCs.

#### The 5 W's

| | |
|---|---|
| **Who** | An unknown threat actor, potentially linked to the BlackTech group based on the Flagpro malware identified by VirusTotal vendors. |
| **What** | A malicious file was downloaded and executed on an employee's computer, creating multiple unauthorized executables and triggering a SOC alert via the intrusion detection system. |
| **When** | The incident began at 1:11 p.m. when the employee received the phishing email. The malicious payload executed at 1:13 p.m., unauthorized executables were created at 1:15 p.m., and the IDS alert was sent at 1:20 p.m. |
| **Where** | An employee's workstation at a financial services company. |
| **Why** | The employee was socially engineered into opening a password-protected spreadsheet attached to a phishing email. The password was included in the email to bypass automated attachment scanning, and opening the file triggered execution of the embedded malicious payload. |

#### Tools used

- **VirusTotal** — used to analyze the SHA256 hash, retrieve detection verdicts from 72 security vendors, and identify associated IoCs (IP addresses, domains, additional hashes, behavioral data).

#### Additional notes

1. 58 out of 72 security vendors flagged the file as malicious, with a negative community score — strong consensus that the file is malware (identified as Flagpro).
2. The password-in-email technique is a deliberate evasion tactic to bypass email security gateways that scan attachments — how can this specific vector be detected or blocked upstream?

---

*Journal maintained as part of a professional cybersecurity portfolio.*
