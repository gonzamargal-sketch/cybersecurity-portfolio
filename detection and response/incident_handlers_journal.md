# Incident Handler's Journal

> **Document type:** Cumulative incident response journal  
> **Context:** Google Cybersecurity Certificate — Detection and Response module  
> **Skills demonstrated:** Incident documentation, the 5 W's framework, NIST IR lifecycle, tool usage logging, post-incident reflection

---

## How to Read This Journal

Each entry documents a security incident scenario or tool usage analyzed during the course. Entries follow a consistent structure:

- **The 5 W's** — Who, What, Where, When, Why (for incident investigations)
- **Tools used** — any platforms, frameworks, or utilities applied
- **Additional notes** — reflections, open questions, or lessons learned

---

## Entries

---

### Entry #1

**Date:** 2026-06-22  
**Entry number:** 1  
**Description:** A small U.S. healthcare clinic suffered a ransomware attack that encrypted critical files and shut down business operations. The incident was initiated through phishing emails and resulted in the clinic being unable to access patient records. This entry corresponds to the **Detection and Analysis** phase of the NIST Incident Response Lifecycle, as the focus is on identifying what happened, who caused it, and how the attack was carried out.

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
**Description:** As a Level 1 SOC analyst at a financial services company, I investigated an alert triggered by a suspicious file downloaded on an employee's computer via a phishing email disguised as a password-protected spreadsheet. I used VirusTotal to analyze the file's SHA256 hash and identify associated IoCs. This entry falls within the **Detection and Analysis** phase — the goal was to confirm whether the file was malicious, attribute it to a known threat actor, and extract indicators for containment.

#### The 5 W's

| | |
|---|---|
| **Who** | An unknown threat actor, potentially linked to the BlackTech group based on the Flagpro malware identified by VirusTotal vendors. |
| **What** | A malicious file was downloaded and executed on an employee's computer, creating multiple unauthorized executables and triggering a SOC alert via the intrusion detection system. |
| **When** | The incident began at 1:11 p.m. when the employee received the phishing email. The malicious payload executed at 1:13 p.m., unauthorized executables were created at 1:15 p.m., and the IDS alert was sent at 1:20 p.m. |
| **Where** | An employee's workstation at a financial services company. |
| **Why** | The employee was socially engineered into opening a password-protected spreadsheet attached to a phishing email. The password was included in the email to bypass automated attachment scanning, and opening the file triggered execution of the embedded malicious payload. |

#### Tools used

- **VirusTotal** — used to analyze the SHA256 hash, retrieve detection verdicts from 72 security vendors, and identify associated IoCs (IP addresses, domains, additional hashes, and behavioral sandbox data).

#### Additional notes

1. 58 out of 72 security vendors flagged the file as malicious, with a negative community score — strong consensus that the file is malware (identified as Flagpro).
2. The password-in-email technique is a deliberate evasion tactic to bypass email security gateways — how can this specific vector be detected or blocked upstream?

---

### Entry #3

**Date:** 2026-06-24  
**Entry number:** 3  
**Description:** Configured and ran Suricata, an open-source IDS/IPS, to detect suspicious network traffic using custom rules applied to a PCAP file. Examined alert output in both fast.log and eve.json formats, and used jq to query structured JSON log data. This entry corresponds to the **Detection and Analysis** phase — Suricata is a detection tool used to identify threats in network traffic and generate evidence for further investigation.

#### The 5 W's

N/A — this entry documents tool usage rather than a specific incident investigation.

#### Tools used

- **Suricata** — open-source IDS/IPS used to write and trigger custom detection rules against captured network traffic (`sample.pcap`). Generated alert logs in `fast.log` and full event telemetry in `eve.json`.
- **jq** — command-line JSON processor used to parse, filter, and extract specific fields from `eve.json` for analysis.

#### Additional notes

1. `fast.log` is useful for quick rule validation during development but is considered a deprecated format — `eve.json` is the standard for production environments and SIEM ingestion.
2. The `flow_id` field in `eve.json` links all log records belonging to the same network conversation, enabling analysts to reconstruct the full sequence of a security event across multiple packets.

---

### Entry #4

**Date:** 2026-06-24  
**Entry number:** 4  
**Description:** Investigated a suspicious file hash as a SOC analyst by generating SHA256 hash values in Linux using `sha256sum` and comparing them to detect tampering. The activity demonstrated that two files with visually identical content can have completely different hash values, confirming that one had been modified. This entry corresponds to the **Detection and Analysis** phase — hash comparison is a key technique for verifying file integrity and identifying malicious files during an investigation.

#### The 5 W's

N/A — this entry documents a forensic technique and tool usage rather than a specific incident investigation.

#### Tools used

- **sha256sum** (Linux) — used to generate SHA256 cryptographic hashes for files and export them to hash files for comparison.
- **cmp** (Linux) — used to compare two hash files byte by byte and identify the exact position of any difference.

#### Additional notes

1. Visual inspection of file content is not a reliable method for detecting tampering — a single invisible character (null byte, trailing space, different line ending) will not appear in terminal output but will produce a completely different hash.
2. Hash comparison is foundational to forensic evidence handling: disk images are hashed before and after analysis to prove the evidence was not altered.

---

*Journal maintained as part of a professional cybersecurity portfolio.*
