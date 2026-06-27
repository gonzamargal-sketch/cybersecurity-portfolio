# 🛡️ Cybersecurity Portfolio

Welcome to my cybersecurity portfolio. This repository documents the hands-on labs, exercises, and analyses I completed as part of the **Google Cybersecurity Certificate**, covering the full curriculum from security fundamentals to Python automation.

## 👨‍💻 About Me

I hold the Google Cybersecurity Certificate and have built practical experience across the core domains of cybersecurity: auditing, network analysis, threat modeling, incident response, and security automation.

[View Professional Statement](about/professional-statement.md)

---

## 📂 Portfolio Contents

### 🔍 Security Audits
Internal security audit of a fictional company (Botium Toys), applying the NIST Cybersecurity Framework.

| File | Description |
|---|---|
| [Scope, Goals & Risk Assessment](audits/1-scope-goals-risk-assessment.md) | Defined audit scope, business goals, and initial risk scoring |
| [Control Categories](audits/2-control_categories.md) | Reviewed administrative, technical, and physical controls |
| [Controls & Compliance Checklist](audits/3-controls_and_compliance_checklist.md) | Assessed compliance with PCI-DSS, GDPR, and SOC standards |

---

### 🌐 Network Analysis

**Communication & Traffic Analysis**

| File | Description |
|---|---|
| [Network Traffic Analysis](network%20analysis/1-communication/1-network_traffic_analysis_explanation.md) | Explanation of DNS and ICMP traffic behavior |
| [Cybersecurity Incident Report](network%20analysis/1-communication/2-cybersecurity_incident_report.md) | Formal report on a simulated network incident |

**Network Attacks**

| File | Description |
|---|---|
| [Wireshark TCP/HTTP Log](network%20analysis/2-attacks/1_wireshark_TCP_HTTP_log.md) | Analysis of captured TCP and HTTP traffic |
| [Network Attack Analysis](network%20analysis/2-attacks/2_network_attack_analysis_explanation.md) | Identification and explanation of attack patterns |
| [Incident Report — Network Attack](network%20analysis/2-attacks/3_cybersecurity_incident_report_network_attack.md) | Formal incident documentation |

**OS & Network Hardening**

| File | Description |
|---|---|
| [TCP Traffic Log — OS Hardening](network%20analysis/3-OS_hardening/1-tcp_traffic_log_explanation.md) | Traffic log review in a brute-force context |
| [Security Incident Report](network%20analysis/3-OS_hardening/2-security_incident_report.md) | OS hardening recommendations post-incident |
| [Network Hardening Scenario](network%20analysis/4-network_hardening/1-network_hardening_scenario.md) | Scenario setup for network hardening analysis |
| [Security Risk Assessment](network%20analysis/4-network_hardening/2-security_risk_assessment_report.md) | Risk assessment and hardening controls |

**NIST CSF Application**

| File | Description |
|---|---|
| [DoS Attack Scenario](network%20analysis/5-Security_incident_using_NIST-CSF/1-DoS_attack_scenario.md) | Scenario description for a DDoS incident |
| [Incident Report Analysis (NIST CSF)](network%20analysis/5-Security_incident_using_NIST-CSF/2-incident_report_analysis.md) | Full incident analysis mapped to the 5 NIST CSF functions |

---

### 🐧 Linux & SQL

| File | Description |
|---|---|
| [Linux Activities](linux%20and%20sql/linux_simple_activities.md) | File permissions, user management, directory navigation |
| [SQL Activities](linux%20and%20sql/sql_simple_activities.md) | Filtering queries, joins, and data analysis for security investigations |

---

### 🔐 Assets, Threats & Vulnerabilities

| File | Description |
|---|---|
| [Encryption & Decryption](assets%20threats%20and%20vulnerabilities/1-encryption_decryption.md) | Caesar cipher (tr), AES-256-CBC with OpenSSL |
| [Creating Hash Values](assets%20threats%20and%20vulnerabilities/2-creating_hash_values.md) | SHA-256 file hashing and integrity verification with sha256sum and cmp |
| [Vulnerable System Analysis](assets%20threats%20and%20vulnerabilities/3-vulnerable_system_analysis.md) | Risk assessment using NIST SP 800-30 Rev. 1 |
| [Attack Vectors — USB Baiting](assets%20threats%20and%20vulnerabilities/4-attack_vectors.md) | Attacker mindset exercise: physical attack surfaces and social engineering |
| [PASTA Threat Model](assets%20threats%20and%20vulnerabilities/5-PASTA_threat_model.md) | Full 7-stage PASTA threat model for a mobile app |

---

### 🚨 Detection & Response

| File | Description |
|---|---|
| [Incident Handler's Journal](detection%20and%20response/incident_handlers_journal.md) | Cumulative journal with 4 entries covering ransomware, malware analysis, IDS usage, and file integrity |
| [Suricata — Custom Rules & Log Analysis](detection%20and%20response/1-suricata_logs.md) | Custom IDS rule authoring, PCAP analysis, fast.log and eve.json with jq |
| [Pyramid of Pain — IoC Analysis](detection%20and%20response/pyramid_of_pain.md) | VirusTotal hash analysis, IoC classification, Flagpro/BlackTech attribution |

---

### 🐍 Automate Tasks with Python

| File | Description |
|---|---|
| [Regular Expressions](automate%20tasks%20with%20python/1-regex.md) | `re.findall()`, character and quantity symbols, pattern construction for security data |
| [Importing & Parsing Files](automate%20tasks%20with%20python/2-import_and_parse.md) | File I/O with `open()`, `.read()`, `.write()`, `.split()`, and append/write modes |
| [Creating an Algorithm](automate%20tasks%20with%20python/3-create_algorithm.md) | IP allowlist updater: file read → list manipulation → `.join()` → file rewrite, wrapped into a reusable function |
| [Debugging Python Code](automate%20tasks%20with%20python/4-debugging_python_code.md) | Identifying and fixing syntax errors, exceptions, and logic errors in security-related code |

---

## 🛠️ Tools & Technologies

| Category | Tools / Technologies |
|---|---|
| **Operating system** | Linux (Bash) |
| **Network analysis** | Wireshark, tcpdump |
| **Intrusion detection** | Suricata (IDS/IPS), jq |
| **Threat intelligence** | VirusTotal |
| **Cryptography** | OpenSSL (AES-256-CBC), sha256sum, cmp |
| **Programming** | Python 3 (`re`, file I/O) |
| **Database** | SQL |
| **Version control** | Git & GitHub |
| **Frameworks** | NIST CSF, NIST SP 800-30 Rev. 1, NIST IR Lifecycle, PASTA, MITRE ATT&CK |
| **Standards** | PCI-DSS, GDPR, SOC |

---

## 🎯 Skills Demonstrated

- Security auditing and compliance assessment
- Network traffic analysis and incident reporting
- Threat modeling (PASTA) and attack surface evaluation
- Cryptographic hashing and file integrity verification
- Intrusion detection with Suricata and log analysis with jq
- IoC identification and threat attribution using the Pyramid of Pain
- Python scripting for security automation (file I/O, regex, algorithm design, debugging)
- Incident documentation following the NIST IR Lifecycle

---

## 📬 Contact

LinkedIn: [Gonzalo Maroto](https://www.linkedin.com/in/gonzalo-maroto/)

---

## 🇪🇸 Resumen en español

Este repositorio documenta el trabajo práctico realizado durante el **Certificado de Ciberseguridad de Google**, cubriendo auditorías de seguridad, análisis de redes, amenazas y vulnerabilidades, detección y respuesta a incidentes, y automatización con Python.

Los documentos están redactados en inglés para maximizar su utilidad en un entorno profesional internacional.
