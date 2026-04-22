# Botium Toys — Scope, Goals, and Risk Assessment Report

> **Document type:** Security audit report  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** Risk analysis, asset identification, control assessment, NIST CSF

---

## What is this document?

This report defines the **scope and goals** of an internal security audit conducted on Botium Toys, a fictional toy company used as a case study. The objective is to assess the organization's security posture, identify its assets, and determine which controls and compliance practices need to be implemented.

---

## Audit Scope

The scope covers the **entire security program at Botium Toys**, including:

- All assets managed by the IT department
- Internal processes and procedures related to control implementation
- Applicable regulatory compliance practices

---

## Goals

1. Assess the organization's existing assets.
2. Complete the controls and compliance checklist.
3. Determine which controls and best practices must be implemented to improve Botium Toys' security posture.

---

## Current Asset Inventory

The IT department manages the following asset types:

| Category | Examples |
|---|---|
| On-premises equipment | Hardware for in-office business use |
| Employee equipment | Laptops, smartphones, headsets, surveillance cameras, etc. |
| Storefront products | Stock in physical store and warehouse |
| Systems and services | Accounting, telecommunications, database, e-commerce, inventory management |
| Network infrastructure | Internet access, internal network |
| Data | Storage and data retention |
| Legacy systems | End-of-life systems requiring manual monitoring |

---

## Risk Assessment

### Risk Description

Botium Toys has **inadequate asset management** and lacks the necessary controls to comply with domestic and international regulations.

### Risk Score

> **8 / 10** — High level

The elevated score is due to a lack of implemented controls and non-adherence to security best practices.

### Applied Framework: NIST CSF

The **first function of the NIST Cybersecurity Framework — Identify** is applied here. Botium Toys must allocate resources to identify, classify, and evaluate the potential impact of asset loss on business continuity.

---

## Key Findings (Detailed Analysis)

The following vulnerabilities were identified during the assessment, organized by risk level:

### 🔴 Critical Risks

| Finding | Impact |
|---|---|
| All employees have access to internal data, including payment card data (PII/SPII) | Violation of least privilege principle; high risk of data leakage |
| Encryption is not used for credit card data | Non-compliance with PCI DSS; exposure of sensitive financial data |
| No access controls in place (no least privilege or separation of duties) | High risk from insider threats or compromised accounts |
| No disaster recovery plans or backups of critical data | No incident response capability; risk of total data loss |
| No intrusion detection system (IDS) installed | No visibility into active network attacks |

### 🟡 Medium Risks

| Finding | Impact |
|---|---|
| Password policy exists but requirements are minimal and outdated | Vulnerability to brute force or dictionary attacks |
| No centralized password management system | Loss of productivity and weak passwords across the organization |
| Legacy systems are monitored but without a regular schedule or clear intervention procedures | Risk of undetected failures in critical infrastructure |

### 🟢 Controls in Good Standing

| Finding | Assessment |
|---|---|
| Firewall configured with defined security rules | ✅ Implemented |
| Antivirus installed and regularly monitored | ✅ Implemented |
| Data availability and integrity managed by IT department | ✅ Implemented |
| Plan to notify EU customers within 72 hours of a breach | ✅ Implemented (partial GDPR) |
| Privacy policies documented and enforced | ✅ Implemented |
| Physical security: locks, CCTV, fire prevention systems | ✅ Implemented |

---

## Personal Reflection

This report represents the **starting point of a full security audit**. The risk assessment highlights that Botium Toys prioritizes operational continuity over security — a very common situation in growing small and medium-sized businesses.

The most concerning findings are the absence of encryption for financial data and the lack of least-privilege access controls, as both constitute direct violations of regulations such as **PCI DSS** and **GDPR**, with serious legal and financial consequences.

Using the **NIST CSF** as a reference framework allows the audit to be structured systematically, starting with asset identification before addressing protection, detection, response, and recovery.

---

*Document created as part of a professional cybersecurity portfolio.*