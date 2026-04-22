# Cybersecurity Control Categories

> **Document type:** Technical reference material  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** Security control frameworks, defense in depth, control classification

---

## What is this document?

This document is a **reference guide on cybersecurity control types and categories**. Controls are safeguards or countermeasures that organizations implement to reduce risk, protect assets, and ensure business continuity.

Understanding this taxonomy is essential for conducting security audits, designing defense-in-depth architectures, and communicating risk to organizational stakeholders.

---

## The Three Main Control Categories

Cybersecurity controls are grouped into three broad categories based on their nature:

```
┌─────────────────────────────────────────────────────────────┐
│                    SECURITY CONTROLS                        │
├───────────────────┬──────────────────┬──────────────────────┤
│  Administrative   │    Technical     │  Physical/Operational│
│  (people and      │  (technology)    │  (physical access)   │
│   policies)       │                  │                      │
└───────────────────┴──────────────────┴──────────────────────┘
```

### 1. Administrative/Managerial Controls
Address the **human component** of cybersecurity. These include policies and procedures that define how an organization manages data and clearly outlines employee responsibilities, including their role in protecting the organization. Although they are policy-based, enforcement may require the use of technical or physical controls.

**Examples:** Password policies, security awareness training, incident response plans.

### 2. Technical Controls
These are **technology-based solutions** that protect information systems. They can be used in multiple ways to meet an organization's security goals.

**Examples:** Firewalls, IDS/IPS, antivirus, encryption, password management systems.

### 3. Physical/Operational Controls
Limit **physical access** to tangible organizational assets to prevent unauthorized access.

**Examples:** Door locks, CCTV cameras, badge readers, security lighting.

---

## The Four Control Types

Regardless of category, each control can be classified by its **function or purpose** within the security strategy:

| Type | Function | Description |
|---|---|---|
| **Preventative** | Prevent incidents | Designed to stop a security incident from occurring in the first place |
| **Corrective** | Restore assets | Used to restore an asset or system after an incident |
| **Detective** | Detect incidents | Determines whether an incident has occurred or is in progress |
| **Deterrent** | Discourage attacks | Reduces the likelihood of an attack by making success seem less probable |

> These types work together to provide **defense in depth**.

---

## Detailed Control Tables

### Administrative/Managerial Controls

| Control | Type | Purpose |
|---|---|---|
| Least Privilege | Preventative | Reduce risk and overall impact of malicious insider or compromised accounts |
| Disaster recovery plans | Corrective | Provide business continuity |
| Password policies | Preventative | Reduce likelihood of account compromise through brute force or dictionary attack techniques |
| Access control policies | Preventative | Bolster confidentiality and integrity by defining which groups can access or modify data |
| Account management policies | Preventative | Manage account lifecycle, reduce attack surface, and limit impact from disgruntled former employees and default account usage |
| Separation of duties | Preventative | Reduce risk and overall impact of malicious insider or compromised accounts |

### Technical Controls

| Control | Type | Purpose |
|---|---|---|
| Firewall | Preventative | Filter unwanted or malicious traffic from entering the network |
| IDS/IPS | Detective | Detect and prevent anomalous traffic that matches a signature or rule |
| Encryption | Deterrent | Provide confidentiality to sensitive information |
| Backups | Corrective | Restore/recover from an event |
| Password management | Preventative | Reduce password fatigue and promote password strength |
| Antivirus (AV) software | Corrective | Detect and quarantine known threats |
| Manual monitoring, maintenance, and intervention for legacy systems | Preventative | Identify and manage threats, risks, or vulnerabilities in out-of-date systems |

### Physical/Operational Controls

| Control | Type | Purpose |
|---|---|---|
| Time-controlled safe | Deterrent | Reduce attack surface and overall impact from physical threats |
| Adequate lighting | Deterrent | Deter threats by limiting hiding places |
| Closed-circuit television (CCTV) | Preventative/Detective | Its presence can reduce the risk of certain events, and footage can be used after an event to inform on conditions |
| Locking cabinets (for network gear) | Preventative | Prevent unauthorized personnel from physically accessing or modifying network infrastructure |
| Signage indicating alarm service provider | Deterrent | Deter certain types of threats by making a successful attack seem less likely |
| Locks | Preventative/Deterrent | Bolster integrity by deterring and preventing unauthorized physical access to assets |
| Fire detection and prevention (alarm, sprinkler system, etc.) | Detective/Preventative | Detect fire and prevent damage to physical assets such as inventory and servers |

---

## Key Concept: Defense in Depth

**Defense in Depth** is a security strategy that applies multiple layers of controls of different types and categories. If one control fails, additional layers continue to protect assets.

```
Example of defense-in-depth layers:

[Physical layer]      → Locks + CCTV
[Network layer]       → Firewall + IDS/IPS
[Endpoint layer]      → Antivirus + disk encryption
[Data layer]          → Encryption + access control
[Human layer]         → Training + password policies
```

No single control is sufficient on its own. Combining preventative, detective, corrective, and deterrent controls across all three categories (administrative, technical, and physical) is what provides robust security.

---

*Document created as part of a professional cybersecurity portfolio.*
