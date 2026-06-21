# PASTA Threat Model — Sneaker Company Mobile App

> **Document type:** Threat modeling exercise using the PASTA framework  
> **Context:** Google Cybersecurity Certificate — Assets, Threats and Vulnerabilities module  
> **Skills demonstrated:** PASTA framework application, threat analysis, vulnerability identification, attack tree interpretation, security control selection

---

## The PASTA Framework

PASTA (Process for Attack Simulation and Threat Analysis) is a seven-stage, risk-centric threat modeling methodology. Each stage builds on the previous one, moving from business context down to concrete security controls.

| Stage | Purpose |
|---|---|
| I. Define business and security objectives | Understand why the app exists and what it must protect |
| II. Define the technical scope | Identify the technologies involved and their security implications |
| III. Decompose the application | Map how data flows through the system |
| IV. Perform a threat analysis | Identify realistic threats against the application and its data |
| V. Perform a vulnerability analysis | Find weaknesses in the attack surface that threats could exploit |
| VI. Conduct attack modeling | Build an attack tree that connects threats to vulnerabilities |
| VII. Analyze risk and impact | Define security controls that reduce the identified risks |

---

## Scenario

A sneaker enthusiast company is launching a mobile app that allows customers to buy and sell shoes peer-to-peer. The app will handle user accounts, messaging, seller ratings, and payment processing. As part of the security team, a PASTA threat model is performed before launch.

---

## PASTA Worksheet

### Stage I — Define Business and Security Objectives

1. Users must be able to create member profiles internally or by connecting external accounts (e.g., social login).
2. The app must process financial transactions securely between buyers and sellers.
3. The app must comply with PCI-DSS to meet payment card industry regulations and avoid legal liability.

---

### Stage II — Define the Technical Scope

**Technologies in use:** API, PKI (AES + RSA), SHA-256, SQL.

The **API** is the highest-priority technology to evaluate because it is the single entry point through which all data — user credentials, payment details, and inventory queries — flows between the mobile client and the backend. A misconfigured or unauthenticated API endpoint exposes every underlying system simultaneously, making it the widest attack surface and the most critical to harden first.

---

### Stage III — Decompose the Application

The data flow diagram below represents the product search process — one of the core application flows:

```
User  ──(searching for sneakers for sale)──▶  Product search process  ──(query)──▶  Database
User  ◀──(search results returned)──────────  Product search process  ◀──(listings of current inventory)──  Database
```

User input travels through the API to the product search process, which queries the SQL database and returns matching inventory listings. At each step, data must be validated, and the connection between the app and the database must be encrypted and access-controlled.

---

### Stage IV — Threat Analysis

| # | Threat type | Description |
|---|---|---|
| 1 | **External** — SQL injection | A threat actor crafts malicious SQL input through the app's search or login fields to manipulate database queries, potentially exfiltrating user data, payment records, or seller information. |
| 2 | **External** — Session hijacking | An attacker intercepts or steals a valid session token to impersonate an authenticated user, gaining access to their account, messages, and payment methods without knowing their credentials. |

---

### Stage V — Vulnerability Analysis

| # | Vulnerability | Where it appears |
|---|---|---|
| 1 | **Lack of prepared statements** | SQL queries built by concatenating user input are vulnerable to injection. Without parameterized queries, malicious input is executed as code against the database. |
| 2 | **Broken API token** | If API tokens are improperly validated, expired tokens are not revoked, or token secrets are weak, an attacker can forge or reuse them to authenticate as a legitimate user and hijack their session. |

---

### Stage VI — Attack Modeling

The attack tree maps how the primary goal (compromising user data) can be achieved via two distinct paths:

```
                        User data
                            │
          ┌─────────────────┴─────────────────┐
          │                                   │
    SQL injection                     Session hijacking
          │                                   │
  Lack of prepared                    Weak login
    statements                        credentials
```

- **Left branch:** An attacker exploits the absence of prepared statements to inject SQL and access the database directly.
- **Right branch:** An attacker takes advantage of weak credentials (or absence of MFA) to hijack an authenticated session and operate as a legitimate user.

---

### Stage VII — Risk Analysis and Impact

Four security controls that reduce the likelihood of the identified threats:

| # | Control | Type | Purpose |
|---|---|---|---|
| 1 | **Prepared statements / parameterized queries** | Technical | Directly prevents SQL injection by ensuring user input is never interpreted as executable SQL code |
| 2 | **Password policy** (e.g., MFA enforcement) | Managerial + Technical | Reduces the risk of session hijacking and credential attacks by making valid credentials harder to obtain or reuse |
| 3 | **SHA-256 with salting** | Technical | Hashes passwords and sensitive data at rest so a database breach does not directly expose usable credentials |
| 4 | **Principle of least privilege** | Managerial | Limits each user, API endpoint, and service to only the permissions required for its function, minimizing the blast radius of any successful attack |

---


*Document created as part of a professional cybersecurity portfolio.*
