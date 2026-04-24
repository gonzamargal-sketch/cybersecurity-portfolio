# Security Risk Assessment Report

> **Document type:** Formal security risk assessment  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** Vulnerability prioritization, network hardening tool selection, risk-based decision making, security control justification

---

## Part 1: Select Up to Three Hardening Tools and Methods to Implement

Based on the four vulnerabilities identified during the network inspection — password sharing among employees, a default admin password on the database, firewalls with no traffic filtering rules, and the absence of multi-factor authentication — the following three hardening measures are recommended as the most effective response:

**1. Implement Multi-Factor Authentication (MFA)**
MFA requires users to verify their identity using two or more methods before gaining access to systems or the network. This directly addresses the absence of MFA (Vulnerability 4) and significantly reduces the risk posed by shared or compromised passwords (Vulnerability 1), since a stolen credential alone is no longer sufficient to authenticate.

**2. Enforce strong password policies aligned with NIST guidelines**
This includes setting unique, non-default passwords for all accounts — particularly the database administrator account — prohibiting password sharing, and applying salting and hashing to stored credentials. This directly addresses Vulnerabilities 1 and 2.

**3. Implement and maintain firewall rules with port filtering**
Configuring firewall rules to control inbound and outbound traffic, combined with port filtering to block unused or unnecessary ports, closes the open network perimeter that currently exists due to the absence of any filtering rules. This directly addresses Vulnerability 3.

---

## Part 2: Explain Your Recommendations

### Multi-Factor Authentication (MFA)

MFA is one of the most effective single controls available against unauthorized access. The organization's current situation — employees sharing passwords and MFA being absent — means that any attacker who obtains a single password gains unrestricted access to the associated account and potentially to sensitive customer data. MFA eliminates this risk by requiring a second verification factor (such as a one-time password sent to a mobile device, a hardware token, or biometric confirmation) that the attacker cannot obtain through credential theft or guessing alone.

MFA is particularly effective in this scenario because it provides a defense-in-depth layer that compensates for the existing password hygiene issues while those issues are being separately addressed through policy enforcement. Even if an employee continues to share a password, the shared credential alone cannot be used to access the system without the second factor.

**Implementation frequency:** MFA is configured once per account and then maintained on an ongoing basis. It requires periodic review to ensure coverage extends to all accounts — including administrative and service accounts — and to update authentication methods as new, more secure options become available.

---

### Password Policies (with enforcement of unique, non-default credentials)

The combination of shared passwords and a default database administrator password represents a critical and easily exploitable weakness. Default credentials for common database systems are publicly documented and are among the first things an attacker attempts. A single successful login to the admin account gives the attacker full control over the database and all customer data within it.

Enforcing a strong password policy — requiring unique passwords per user, prohibiting sharing, mandating that all default credentials be changed immediately upon deployment, and applying NIST-recommended practices such as salting and hashing stored passwords — addresses these vulnerabilities at their root. Unlike overly complex password requirements that push users toward workarounds (such as writing passwords down or reusing them), NIST's current guidelines focus on password length and uniqueness, which are more effective and more user-friendly.

**Implementation frequency:** Password policies are established once as organizational policy and enforced continuously through technical controls (such as a password manager or identity provider that rejects reused or default credentials). The policy itself should be reviewed periodically — at minimum annually, or following any security incident involving compromised credentials — to ensure it remains aligned with current NIST recommendations.

---

### Firewall Maintenance and Port Filtering

Firewalls with no rules in place provide no practical security benefit — all traffic, including malicious traffic, passes through freely. Configuring firewall rules to define what traffic is permitted in and out of the network creates the foundational perimeter defense that the organization currently lacks. Port filtering complements this by blocking traffic on ports that are not actively needed, reducing the number of potential entry points an attacker can probe.

Together, these measures prevent unauthorized external actors from reaching internal systems and databases directly, contain the spread of any future breach by limiting lateral movement within the network, and provide the basis for detecting anomalous traffic patterns through network log analysis.

**Implementation frequency:** Initial firewall rule configuration is a one-time setup, but firewall maintenance must be performed on a regular and ongoing basis. Rules should be reviewed whenever the network topology changes, after any security incident, and as part of scheduled security reviews (recommended at minimum quarterly). Firewall logs should be monitored continuously — ideally through a SIEM tool — to detect attempts to exploit open ports or bypass filtering rules.

---

*Document created as part of a professional cybersecurity portfolio.*
