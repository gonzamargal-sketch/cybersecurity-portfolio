# Pyramid of Pain — Flagpro Malware Analysis

> **Document type:** IoC analysis using the Pyramid of Pain framework  
> **Context:** Google Cybersecurity Certificate — Detection and Response module  
> **Skills demonstrated:** VirusTotal analysis, IoC identification and classification, Pyramid of Pain framework, SHA256 file hashing, threat actor research

---

## Scenario

As a Level 1 SOC analyst at a financial services company, an alert was received about a suspicious file downloaded on an employee's computer via a phishing email containing a password-protected spreadsheet. The file's SHA256 hash was retrieved and analyzed using VirusTotal to identify indicators of compromise (IoCs) and assess the threat.

**SHA256 file hash:** `54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b`

**VirusTotal verdict:** 58/72 vendors flagged the file as malicious. Negative community score. Identified as **Flagpro** malware, associated with the **BlackTech** threat actor group.

---

## The Pyramid of Pain

The Pyramid of Pain is a framework that classifies IoCs by how difficult it is for an attacker to change them if defenders block them. The higher the level in the pyramid, the more painful it is for the attacker to adapt — making higher-level indicators more valuable for long-term defense.

```
        ▲
       /TTPs\              ← Hardest for attacker to change
      /───────\
     /  Tools  \
    /───────────\
   /Network/host \
  /    artifacts  \
 /─────────────────\
/   Domain names    \
/───────────────────\
/    IP addresses    \
/─────────────────────\
/      Hash values     \   ← Easiest for attacker to change
└──────────────────────┘
```

---

## IoCs Identified

### Hash Values
*Easiest level for attackers to evade — regenerating a file changes the hash.*

| Hash type | Value |
|---|---|
| SHA256 | `54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b` |
| SHA1 | `8f35a9e70dbec8f1904991773f394cd4f9a07f5e` |
| MD5 | `287d612e29b71c90aa54947313810a25` |

---

### IP Addresses
*Blocking IPs is slightly more disruptive to attackers, but IPs can be rotated easily.*

| IP address | Notes |
|---|---|
| `104.115.151.81` | Flagged as malicious by multiple vendors — C2 communication endpoint associated with this malware |

---

### Domain Names
*More disruptive than IPs — domain registration and reputation take time to rebuild.*

| Domain | Notes |
|---|---|
| `org.misecure.com` | Flagged as malicious — used for command and control (C2) communication by Flagpro |

---

### Network / Host Artifacts
*Observable traces left by the malware on the infected system or network.*

- Multiple **unauthorized executable files** created on the employee's computer between 1:13 p.m. and 1:15 p.m. after the malicious payload executed.
- Suspicious **outbound network connections** to the C2 domain and IP address identified above.

---

### Tools
*Software or utilities leveraged by the attacker — blocking specific tools forces them to retool.*

- **Flagpro** — a Windows-based backdoor tool used by the BlackTech threat group, designed for initial access and persistence. It communicates with C2 servers to receive instructions and can download additional payloads.

---

### TTPs (Tactics, Techniques, and Procedures)
*Highest value IoCs — TTPs describe how attackers operate and are the hardest to change.*

| MITRE ATT&CK Technique | Description |
|---|---|
| **T1566.001 — Phishing: Spearphishing Attachment** | Initial access gained via phishing email with a malicious file attachment |
| **T1204.002 — User Execution: Malicious File** | The employee was induced to open and execute the file manually |
| **T1059 — Command and Scripting Interpreter** | Malicious payload executed commands on the host after file was opened |
| **T1105 — Ingress Tool Transfer** | Unauthorized executables downloaded or created on the compromised host |
| **T1071 — Application Layer Protocol** | C2 communication conducted over standard application-layer protocols to blend with normal traffic |

---

## Is the File Malicious?

**Yes.** The evidence is conclusive:

- **58 out of 72** security vendors detected the file as malicious.
- **Negative community score** on VirusTotal.
- Multiple sandbox environments (CAPE Sandbox, Yomi Hunter, DAS-Security Orcas) confirmed malicious behavior.
- The file is identified as **Flagpro**, a known backdoor associated with the **BlackTech** APT group, which targets financial and technology organizations.

---

*Document created as part of a professional cybersecurity portfolio.*
