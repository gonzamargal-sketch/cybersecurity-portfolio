# Incident Report Analysis — NIST CSF

> **Document type:** Formal incident report analysis  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** NIST CSF application, DoS incident analysis, network security planning, incident response and recovery documentation

---

## Summary

At approximately 2:00 PM, the company's internal network experienced a sudden and complete loss of service availability. All network resources became inaccessible to internal employees, halting normal business operations for approximately two hours. Investigation revealed that a malicious external actor had exploited an unconfigured firewall to launch an **ICMP flood Denial of Service (DoS) attack** against the organization's network.

The attacker sent a continuous high-volume flood of ICMP echo request (ping) packets into the network. Because the firewall had no rules in place to rate-limit or filter ICMP traffic, every packet reached the internal network and consumed processing and bandwidth resources until the network was fully overwhelmed and unable to respond to legitimate traffic.

The incident management team responded by blocking all incoming ICMP packets at the firewall, taking non-critical services offline to preserve resources, and progressively restoring critical services. Full network functionality was restored after approximately two hours. Following the incident, the security team implemented four new controls: an ICMP rate-limiting firewall rule, source IP address verification to detect spoofed addresses, network monitoring software for abnormal traffic detection, and an IDS/IPS system to filter suspicious ICMP traffic.

**Attack type:** ICMP Flood Denial of Service (DoS)  
**Systems affected:** Entire internal network infrastructure — all services inaccessible during the attack  
**Attack source:** External malicious actor exploiting an unconfigured network firewall  
**Estimated impact:** Two hours of full operational downtime; inability of all employees to access network resources; potential impact on client-facing service delivery

---

## Identify

The incident management team conducted an audit of the network systems, devices, access policies, and configurations involved in the attack to identify the security gaps that made it possible.

The audit found that the organization's **network firewall had no rules configured to limit or filter ICMP traffic**. ICMP is a legitimate protocol used for network diagnostics (e.g., ping), and its absence from firewall rule sets is a common oversight. However, this gap meant that the attacker could send an unrestricted flood of ICMP packets directly into the internal network without any traffic being dropped or rate-limited at the perimeter.

The affected systems encompassed the **entire internal network**, including all services and resources accessible to employees. No specific data exfiltration was identified — the attack's goal was disruption of availability rather than data theft. The affected business processes included all internal operations dependent on network connectivity, including client project delivery, internal communications, and access to shared resources.

The root cause was a single, well-defined configuration gap: **a firewall with no ICMP traffic controls**, which allowed a volumetric attack to proceed unimpeded until manual intervention stopped it.

---

## Protect

To prevent a recurrence of this type of attack and improve the organization's overall network security posture, the following protective measures have been implemented or are recommended for immediate implementation:

**Implemented immediately following the incident:**
- **ICMP rate-limiting firewall rule:** A new firewall rule caps the volume of incoming ICMP packets allowed per unit of time, preventing any future flood from overwhelming the network even if it originates from a previously unseen source.
- **Source IP address verification:** The firewall now checks incoming ICMP packets for spoofed source IP addresses — a common technique attackers use to obscure their identity and bypass IP-based blocks.

**Recommended additional protective measures:**
- **Firewall rule review and hardening:** All existing firewall rules should be audited to identify other protocols or ports that may be similarly unrestricted. A deny-by-default policy should be enforced, allowing only explicitly required traffic.
- **Access control review:** Network access privileges should be reviewed to ensure internal systems are not unnecessarily exposed to external traffic.
- **Employee and IT staff awareness training:** Staff should be informed about this incident, its cause, and the controls implemented. IT staff in particular should receive training on firewall configuration best practices to prevent similar misconfigurations in the future.
- **Documented firewall configuration procedures:** Standard operating procedures for firewall setup and change management should be created or updated to require mandatory review of ICMP and other common attack vectors before any new firewall is deployed.

---

## Detect

To improve the organization's ability to detect similar attacks quickly in the future — ideally before services are fully disrupted — the following detection capabilities have been implemented or are recommended:

**Implemented following the incident:**
- **Network monitoring software:** Continuously monitors network traffic for abnormal patterns, such as sudden spikes in ICMP packet volume or traffic from non-trusted external IP addresses. Alerts are generated when traffic deviates significantly from established baselines.
- **IDS/IPS system:** An Intrusion Detection and Prevention System has been deployed to analyze ICMP and other traffic in real time, filtering packets that match suspicious signatures or behavioral patterns associated with flood attacks.

**Recommended additional detection measures:**
- **SIEM (Security Information and Event Management) tool:** Centralizes log data from the firewall, IDS/IPS, and network monitoring software into a single platform. Enables the security team to correlate events across multiple data sources and detect attack patterns that might be missed when reviewing each log individually.
- **Firewall logging and alerting:** Configure the firewall to log all dropped packets and generate automated alerts when ICMP rate limits are triggered or source IP verification fails. This provides early warning of attack attempts before they escalate.
- **Traffic baseline establishment:** Define normal traffic volumes and patterns for the organization's network so that anomalies — such as a tenfold increase in ICMP packets — trigger immediate alerts rather than being noticed only after services fail.

---

## Respond

The following response actions were taken during this incident and will form the basis of the organization's response plan for future cybersecurity events:

**Actions taken during this incident:**
1. The incident management team identified the anomalous ICMP traffic as the source of the service disruption.
2. **All incoming ICMP packets were blocked** at the firewall to stop the flood immediately.
3. **Non-critical network services were taken offline** to free up remaining processing resources and prevent further degradation.
4. **Critical network services were restored progressively**, prioritizing the systems most essential to business continuity.
5. The security team conducted a post-incident investigation to identify the root cause and the firewall misconfiguration that allowed the attack to succeed.

**Response plan for future incidents:**

- **Containment:** Upon detection of an ICMP flood or similar DoS attack, immediately apply firewall rules to block or rate-limit the offending traffic type. Isolate affected network segments if necessary to protect critical systems.
- **Neutralization:** Use the IDS/IPS system to identify and block traffic matching the attack signature. Apply source IP blocking for identified attacker addresses, while recognizing that IP spoofing may require rate-limiting rather than IP-specific blocks as a more robust mitigation.
- **Analysis:** Collect and preserve firewall logs, IDS/IPS alerts, and network monitoring data from the time of the attack for forensic review. Determine the attack's origin, duration, peak volume, and which systems were most affected.
- **Communications:** Notify internal management promptly. If client-facing services were disrupted, communicate the nature and duration of the outage to affected clients. Document the incident in the organization's security incident log.
- **Improvements:** After each incident, conduct a post-mortem review to identify gaps in the response process and update response playbooks accordingly. Ensure all response procedures are documented and accessible to the entire security team.

---

## Recover

The following steps were taken to restore the organization's systems and operations following the attack, and will guide recovery from similar future incidents:

**Immediate recovery actions:**
1. Once ICMP traffic was blocked, the network's processing load returned to normal levels.
2. Critical network services — including internal communications, shared file access, and client project systems — were restored first, in order of operational priority.
3. Non-critical services were brought back online progressively once critical systems were confirmed stable.
4. Full network functionality was restored approximately **two hours after the attack began**.

**Recovery plan for future incidents:**

- **Prioritization:** Maintain and regularly update a documented list of critical vs. non-critical systems and services, so that recovery sequencing can be executed immediately without requiring ad-hoc decisions during a stressful incident.
- **System restoration verification:** Before returning systems to full operation, verify that the attack has been fully neutralized and that no residual malicious traffic is present. Confirm that all new firewall rules and IDS/IPS configurations are active and functioning.
- **Data integrity check:** Although this attack targeted availability rather than data, confirm that no data was corrupted or lost during the incident. If backups are needed, restore from the most recent verified clean backup.
- **Internal communications:** Inform all employees when systems are restored and provide a brief explanation of what occurred and what has been done to prevent recurrence. This reduces uncertainty and maintains trust in the security team.
- **Post-recovery review:** Schedule a formal post-incident review within 48–72 hours of full restoration. Document lessons learned, update security procedures, and assign owners to any outstanding remediation tasks identified during the incident.

---

## Reflections / Notes

This incident highlights a recurring pattern in cybersecurity: **the most damaging attacks often exploit the simplest oversights**. The ICMP flood succeeded not because of a sophisticated exploit or zero-day vulnerability, but because a firewall had never been configured with basic traffic controls — a gap that could have been identified through a routine security audit.

Applying the NIST CSF to this incident makes clear that the organization's weakest phase was **Identify** — without regular audits, the firewall misconfiguration went undetected until it was exploited. The post-incident controls primarily strengthen **Detect** and **Protect**, but the most valuable long-term investment is in building a culture of proactive risk identification before incidents occur.

---

*Document created as part of a professional cybersecurity portfolio.*
