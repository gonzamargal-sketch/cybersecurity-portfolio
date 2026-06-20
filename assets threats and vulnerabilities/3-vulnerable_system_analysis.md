# Vulnerability Assessment Report — Publicly Accessible Database Server

> **Document type:** Vulnerability assessment report  
> **Context:** Google Cybersecurity Certificate — Assets, Threats and Vulnerabilities module  
> **Skills demonstrated:** Risk analysis, NIST SP 800-30 Rev. 1 framework, threat modeling, qualitative risk scoring, remediation planning

---

## System Description

The server hardware consists of a powerful CPU processor and 128GB of memory. It runs on the latest version of the Linux operating system and hosts a MySQL database management system. It is configured with a stable network connection using IPv4 addresses and interacts with other servers on the network. Security measures include SSL/TLS encrypted connections. The database has been publicly accessible since the company's launch three years ago, allowing remote employees across multiple locations to query customer data directly over the internet.

---

## Scope

The scope of this vulnerability assessment relates to the current access controls of the system. The assessment will cover a period of three months, from June 2026 to August 2026. This assessment focuses specifically on the confidentiality, integrity, and availability of the data stored on the database server — it does not cover the physical security of the server hardware or other related IT systems. NIST SP 800-30 Rev. 1 is used to guide the risk analysis of the information system.

---

## Purpose

The database server is a critical business asset that stores sensitive customer and prospect data used by remote employees worldwide to identify potential customers. Securing this data is essential to maintaining customer trust, complying with data protection obligations, and protecting the company's reputation. If the server were compromised or disabled, the business could face significant financial losses, legal liability, and an inability to carry out core sales operations. This assessment evaluates the risks introduced by the server's public accessibility and recommends controls to reduce the company's exposure.

---

## Risk Assessment

| Threat source | Threat event | Likelihood | Severity | Risk |
|---|---|---|---|---|
| Hacker (outsider) | Perform reconnaissance and surveillance of organization | 3 | 2 | 6 |
| Competitor | Obtain sensitive information via exfiltration | 2 | 3 | 6 |
| Hacktivist | Conduct Denial of Service (DoS) attacks | 2 | 3 | 6 |

**Risk score:** Likelihood × Severity = Risk

---

## Approach

This assessment used a qualitative methodology, drawing on the threat source and threat event categories defined in NIST SP 800-30 Rev. 1. The three risks selected reflect the most realistic threats against a publicly accessible database server: external actors probing the system, a competitor attempting to steal customer data, and an outsider disrupting service availability. Likelihood and severity scores were estimated based on how exposed the server is to the public internet and how critical the affected data and systems are to daily business operations. The main limitation of this assessment is its reliance on subjective judgment rather than empirical attack data or penetration testing results.

---

## Remediation Strategy

To reduce the identified risks, the company should restrict direct public access to the database server and place it behind a firewall or VPN, allowing connections only from authenticated, authorized sources. Implementing the principle of least privilege would ensure employees can only query the specific data required for their role, while multi-factor authentication (MFA) would reduce the risk of credential-based exfiltration. Adopting a defense-in-depth strategy — combining network segmentation, intrusion detection, and rate limiting — would also help mitigate denial-of-service attempts and unauthorized reconnaissance against the server.

---

*Document created as part of a professional cybersecurity portfolio.*