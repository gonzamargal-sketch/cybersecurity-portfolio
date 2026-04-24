# Network Hardening — Scenario Overview and Available Tools

> **Document type:** Security risk context and hardening tools reference  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** Vulnerability identification, network hardening strategy, security tool selection, risk assessment

---

## What is this document?

This document provides context for a security risk assessment activity. A social media organization recently suffered a major data breach that exposed customers' personal information (names and addresses). The organization has tasked its security analysts with identifying existing vulnerabilities and recommending network hardening measures to prevent future incidents.

---

## The Scenario

A security analyst inspects the organization's network following a data breach and identifies **four critical vulnerabilities** that likely contributed to the incident and continue to put the organization at risk:

| # | Vulnerability | Risk |
|---|---|---|
| 1 | **Employees share passwords** | A single compromised password exposes multiple accounts; no accountability per user |
| 2 | **Database admin password is set to default** | Default credentials are publicly known and trivial to exploit — the first thing any attacker tries |
| 3 | **Firewalls have no traffic filtering rules** | No barrier between the internal network and malicious inbound or outbound traffic |
| 4 | **Multi-factor authentication (MFA) is not used** | Credentials alone are the only line of defense; brute force or credential stuffing attacks succeed immediately |

If left unaddressed, these vulnerabilities expose the organization to repeated data breaches, regulatory penalties, reputational damage, and loss of customer trust.

---

## What is Network Hardening?

**Network hardening** is the process of securing a network by reducing its attack surface — eliminating unnecessary services, enforcing access controls, applying configurations that follow security best practices, and monitoring for anomalies. It is both a one-time setup and an ongoing practice, since new vulnerabilities emerge continuously and configurations drift over time.

Network hardening operates across all layers of the TCP/IP model:
- **Network access layer** — physical security, port filtering, hardware disposal
- **Internet layer** — firewall rules, IP access controls
- **Transport/Application layers** — encryption standards, MFA, password policies, log analysis

---

## Available Network Hardening Tools

The following tools and practices are available to address the organization's vulnerabilities. Each entry includes a description and its common use cases.

---

### 🔐 Authentication & Access Controls

#### Multifactor Authentication (MFA)
A security measure requiring a user to verify their identity in two or more ways before accessing a system or network. MFA options include passwords, PIN numbers, one-time passwords (OTP) sent to a mobile device, biometrics, and physical badges.

**Common uses:** Protection against brute force attacks and credential stuffing. Implemented once and maintained ongoing. Directly addresses **Vulnerability 4**.

---

#### Password Policies
Based on NIST's latest recommendations: focus on salting and hashing stored passwords rather than requiring overly complex passwords or enforced frequent changes. Policies prevent attackers from guessing passwords manually or via automated scripts.

**Common uses:** Prevention of brute force and dictionary attacks. Directly addresses **Vulnerabilities 1 and 2**.

---

#### Network Access Privileges
Permits, limits, or blocks access to network assets for specific users, roles, groups, IP addresses, or MAC addresses. Enforces the principle of least privilege — users only access what they need for their role.

**Common uses:** Reduces risk from unauthorized users and social engineering. Addresses the password-sharing vulnerability by ensuring each user has individual, role-appropriate credentials.

---

### 🔥 Firewall & Traffic Management

#### Firewall Maintenance
Regular checking and updating of firewall security configurations to stay ahead of emerging threats. Includes reviewing and updating rules that govern what traffic is allowed in and out of the network.

**Common uses:** Ongoing protection; updating rules in response to incidents; protection against DDoS attacks. Directly addresses **Vulnerability 3**.

---

#### Port Filtering
A firewall function that blocks or allows specific port numbers to control network traffic and limit unwanted communication.

**Common uses:** Prevents attackers from entering a private network through open or unused ports. Works in combination with firewall maintenance.

---

#### Disabling Unused Ports
Ports can be blocked on firewalls, routers, and servers to prevent potentially dangerous network traffic from entering through open but inactive ports.

**Common uses:** Proactive defense before an incident; post-incident hardening to close vectors used in an attack.

---

### 🔒 Data Protection & Encryption

#### Encryption Using the Latest Standards
Application of current encryption methods to conceal outgoing data and decrypt incoming data. Includes assessing whether current encryption standards remain secure for the organization's needs.

**Common uses:** Regular assessment of encryption effectiveness; updating standards after a data breach to protect stored and transmitted data.

---

#### Server and Data Storage Backups
Protection of data assets through regular backups stored in physical locations or cloud repositories.

**Common uses:** Recovery from attacks, human error, equipment failures, or unplanned data loss. Essential for business continuity after an incident.

---

### 🖥️ System Maintenance & Monitoring

#### Baseline Configurations
A documented set of specifications for a system, used as a reference for future builds, releases, and updates.

**Common uses:** Restoring a system to a known secure state after a network outage or unauthorized configuration change.

---

#### Configuration Checks
Reviewing system configurations — including database encryption standards — to detect unauthorized changes.

**Common uses:** Identifying drift from secure baselines; detecting tampering or misconfiguration.

---

#### Patch Updates
Software and operating system updates that address known security vulnerabilities. Attackers actively target unpatched systems once a vulnerability is publicly disclosed.

**Common uses:** Ongoing maintenance to close known security gaps before attackers can exploit them.

---

#### Removing or Disabling Unused Applications and Services
Unused applications and services receive fewer security updates and become entry points for attackers.

**Common uses:** Reducing the network's attack surface by eliminating components that are no longer needed.

---

#### Hardware & Software Disposal
Proper wiping and disposal of old hardware and software to prevent data recovery or exploitation of outdated systems without current security patches.

**Common uses:** Preventing attackers from accessing the network through decommissioned but improperly disposed devices.

---

### 📊 Detection & Testing

#### Network Log Analysis
Examination of network logs to identify events of interest — anomalous traffic, unauthorized access attempts, or signs of an active attack. Commonly implemented through a **SIEM (Security Information and Event Management)** tool.

**Common uses:** Real-time alerting on abnormal traffic; forensic analysis during and after incidents; continuous monitoring.

---

#### Penetration Testing (Pen Test)
A simulated attack conducted by security professionals to identify vulnerabilities in systems, networks, websites, applications, and processes before real attackers find them.

**Common uses:** Proactive identification of weaknesses; validating the effectiveness of existing security controls.

---

## Mapping Tools to Vulnerabilities

| Vulnerability | Recommended Tools |
|---|---|
| 1. Employees share passwords | Password policies, Network access privileges, MFA |
| 2. Default admin password on database | Password policies, Configuration checks, Baseline configurations |
| 3. No firewall rules | Firewall maintenance, Port filtering, Disabling unused ports |
| 4. MFA not implemented | Multifactor authentication (MFA) |

---

*Document created as part of a professional cybersecurity portfolio.*
