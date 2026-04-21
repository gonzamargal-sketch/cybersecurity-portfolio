# Controls and Compliance Checklist — Botium Toys

> **Document type:** Security audit — Completed checklist  
> **Context:** Google Cybersecurity Certificate practical case study  
---

## What is this document?

This checklist is the practical output of the Botium Toys security audit. Based on the scope, goals, and risk assessment report, it determines which controls and compliance practices the organization currently has in place — and which are missing.

The goal is to identify **security gaps** and generate concrete recommendations to improve the company's security posture.

---

## Controls Assessment Checklist

For each control, the current implementation status is indicated based on the findings from the risk assessment report.

| Status | Control | Justification |
|---|---|---|
| ❌ No | **Least Privilege** | All employees have access to internal data, including PII/SPII |
| ❌ No | **Disaster recovery plans** | No business continuity plans or recovery procedures exist |
| ⚠️ Partial | **Password policies** | A policy exists, but requirements are minimal and outdated |
| ❌ No | **Separation of duties** | No separation of responsibilities controls have been implemented |
| ✅ Yes | **Firewall** | IT department has a firewall with a defined set of security rules |
| ❌ No | **Intrusion detection system (IDS)** | No IDS is installed on the network |
| ❌ No | **Backups** | No backups of critical data exist |
| ✅ Yes | **Antivirus software** | Installed and regularly monitored by the IT department |
| ⚠️ Partial | **Legacy system monitoring** | Monitored, but without a regular schedule or clear intervention procedures |
| ❌ No | **Encryption** | No encryption is used for credit card data or other sensitive information |
| ❌ No | **Password management system** | No centralized system; affects productivity and security |
| ✅ Yes | **Locks (offices, storefront, warehouse)** | Physical facilities have adequate locks in place |
| ✅ Yes | **CCTV surveillance** | Closed-circuit television system is operational and up to date |
| ✅ Yes | **Fire detection/prevention** | Fire alarms and sprinkler systems are functioning |

### Visual Summary of Control Status

```
Implemented controls ✅ (5/14):
  Firewall, Antivirus, Locks, CCTV, Fire detection/prevention

Partially implemented controls ⚠️ (2/14):
  Password policies, Legacy system monitoring

Missing controls ❌ (7/14):
  Least Privilege, DR Plans, Separation of duties,
  IDS, Backups, Encryption, Password management system
```

---

## Compliance Checklist

### 💳 PCI DSS — Payment Card Industry Data Security Standard

Applies because Botium Toys **accepts, processes, stores, and transmits** credit card data.

| Status | Best Practice | Justification |
|---|---|---|
| ❌ No | Only authorized users have access to customers' credit card information | All employees have unrestricted access to internal data |
| ❌ No | Credit card data is stored, accepted, processed, and transmitted in a secure environment | No encryption or adequate access controls are in place |
| ❌ No | Data encryption procedures are implemented to secure credit card transaction touchpoints | Encryption is not currently implemented |
| ❌ No | Secure password management policies are adopted | The existing policy does not meet current minimum standards |

> **PCI DSS Conclusion:** Botium Toys **does not comply** with any of the verified requirements. This represents a significant legal and financial risk, including potential fines and loss of the ability to process card payments.

---

### 🇪🇺 GDPR — General Data Protection Regulation

Applies because Botium Toys **has customers in the European Union**.

| Status | Best Practice | Justification |
|---|---|---|
| ❌ No | E.U. customers' data is kept private/secured | No encryption or adequate access control is in place |
| ✅ Yes | A plan exists to notify E.U. customers within 72 hours of a data breach | The IT department has established this plan |
| ❌ No | Data is properly classified and inventoried | Asset management is inadequate; not all stored data is known |
| ✅ Yes | Privacy policies, procedures, and processes are enforced to document and maintain data | Privacy policies are documented and enforced |

> **GDPR Conclusion:** **Partial compliance**. The 72-hour breach notification and privacy policies are in place, but the lack of encryption and data classification constitutes a violation of core GDPR principles (data minimization, integrity, and confidentiality).

---

### 🔒 SOC Type 1 & Type 2 — System and Organizations Controls

Applies to demonstrate the **reliability of internal controls** to clients and external auditors.

| Status | Best Practice | Justification |
|---|---|---|
| ❌ No | User access policies are established | No least privilege or separation of duties controls exist |
| ❌ No | Sensitive data (PII/SPII) is confidential/private | All employees have unrestricted access to sensitive data |
| ✅ Yes | Data integrity ensures data is consistent, complete, accurate, and validated | IT department has integrated data integrity controls |
| ✅ Yes | Data is available to individuals authorized to access it | Availability is managed by the IT department |

> **SOC Conclusion:** **Partial compliance**. Data integrity and availability are covered, but confidentiality and access policies have critical gaps.

---

## Priority Recommendations

Based on the audit findings, the following actions should be prioritized according to the risk posed if not implemented in a timely manner:

### 🔴 High Priority — Immediate Action Required

1. **Implement data encryption**  
   Especially for payment card data and PII/SPII. This is the most urgent requirement for PCI DSS and GDPR compliance.

2. **Apply least privilege and separation of duties**  
   Restrict employee access to only the data and systems needed for their role. This will drastically reduce the internal attack surface.

3. **Develop and implement disaster recovery plans and data backups**  
   Without backups or DR plans, a single incident could result in total data loss and business inoperability.

### 🟡 Medium Priority — Implement in the Short Term

4. **Install an intrusion detection system (IDS)**  
   Provides visibility into malicious network activity that a firewall alone cannot detect.

5. **Update the password policy and deploy a centralized password manager**  
   Adopt modern requirements (minimum 8 characters, combination of letters, numbers, and special characters) and enforce them automatically.

6. **Establish a regular maintenance schedule for legacy systems**  
   Define clear intervention procedures to reduce the risk associated with these vulnerable systems.

### 🟢 Low Priority — Ongoing Improvement

7. **Complete the asset inventory and data classification**  
   To comply with GDPR and improve risk management, the organization needs to know what data it stores and where.

---

## Personal Reflection

This checklist perfectly illustrates why security audits are so valuable: **before this audit, Botium Toys had no complete visibility into its own risks**. The company had a false sense of security from having a firewall and antivirus, without realizing that the most critical gaps were in administrative controls (access, passwords, training) and the absence of encryption.

The work of a cybersecurity analyst is not purely technical — it involves **communicating risks to business stakeholders** in clear, prioritized terms, as reflected in the recommendations section of this document.

The use of three regulatory frameworks (PCI DSS, GDPR, SOC) also demonstrates how an organization must respond to multiple simultaneous legal obligations, and how many controls serve to satisfy several standards at once.

---

*Document created as part of a professional cybersecurity portfolio.*
