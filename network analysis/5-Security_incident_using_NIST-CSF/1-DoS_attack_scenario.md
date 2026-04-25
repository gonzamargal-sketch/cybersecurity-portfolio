# Scenario Overview — ICMP Flood DoS Attack & NIST CSF Response

> **Document type:** Scenario explanation and framework context  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** DoS attack analysis, NIST Cybersecurity Framework (CSF) application, incident response lifecycle, network security improvement planning

---

## What is this document?

This document explains the context of a cybersecurity incident affecting a multimedia company and introduces the framework used to structure the security response. The goal is not only to respond to the incident, but to use it as a foundation for building a more resilient network security strategy going forward.

---

## The Organization

A multimedia company offering **web design, graphic design, and social media marketing** services to small businesses. The company operates an internal network that supports its employees and the delivery of services to clients. As a service provider, the confidentiality, integrity, and availability of its systems are critical — any disruption directly impacts both operations and client trust.

---

## The Incident

The organization experienced a **Denial of Service (DoS) attack** that took the internal network offline for approximately **two hours**.

### What happened — timeline

**During the attack:**
- The network was hit by a sudden **flood of ICMP (Internet Control Message Protocol) ping packets** originating from a malicious external actor.
- The volume of incoming ICMP traffic overwhelmed the network's capacity to process requests.
- All internal network services **stopped responding** — employees could not access any network resources.
- Normal business operations were fully disrupted for the duration of the attack.

**Immediate incident response (by the incident management team):**
- **Blocked all incoming ICMP packets** at the network perimeter to stop the flood.
- **Took all non-critical network services offline** to free up resources and reduce the attack surface.
- **Restored critical network services** progressively to resume operations.

**Root cause (identified during investigation):**
- A malicious actor exploited an **unconfigured firewall** — one that had no rules in place to limit or filter ICMP traffic.
- This allowed the attacker to send an unrestricted flood of ICMP pings directly into the network without being blocked.
- The attack is classified as an **ICMP flood DoS attack**: a single-source volumetric attack using ICMP echo requests (pings) to exhaust network bandwidth and processing capacity.

---

## Why ICMP Flood Attacks Are Effective

ICMP (Internet Control Message Protocol) operates at the **Internet layer** of the TCP/IP model and is used for network diagnostics — most famously by the `ping` command. It is a legitimate and necessary protocol, which is why it is often left unrestricted in firewall configurations.

An attacker exploits this by sending ICMP echo requests at a rate that far exceeds the network's ability to process them. Unlike a TCP SYN flood (which exploits the three-way handshake), an ICMP flood does not require the attacker to maintain state — each packet is independent, making it simple to generate at high volume. If the firewall has no rate limiting rules for ICMP, every packet reaches the internal network and consumes resources.

The key vulnerability here was not the ICMP protocol itself, but the **absence of any firewall configuration** to control it.

---

## Post-Incident Security Measures Implemented

Following the investigation, the network security team implemented four specific controls:

| Measure | Purpose |
|---|---|
| **Firewall rule to rate-limit incoming ICMP packets** | Caps the volume of ICMP traffic allowed per unit of time, preventing floods from overwhelming the network |
| **Source IP address verification on the firewall** | Checks for spoofed IP addresses on incoming ICMP packets — attackers often spoof source IPs to evade IP-based blocks |
| **Network monitoring software** | Detects abnormal traffic patterns in real time, enabling faster identification of future attacks |
| **IDS/IPS system for ICMP traffic filtering** | Analyzes ICMP packet characteristics and filters out traffic matching suspicious signatures or behavioral patterns |

These measures address the immediate vulnerability, but the organization now needs a **broader, structured security strategy** to prevent and manage similar incidents in the future.

---

## The Framework: NIST Cybersecurity Framework (CSF)

To move from reactive incident response to proactive security management, the organization will apply the **NIST Cybersecurity Framework (CSF)** — a widely adopted standard developed by the National Institute of Standards and Technology that provides a common language and structured approach for managing cybersecurity risk.

The CSF is organized around **five core functions**, each representing a phase of the security lifecycle:

```
┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
│   IDENTIFY  │ → │   PROTECT   │ → │   DETECT    │ → │   RESPOND   │ → │   RECOVER   │
└─────────────┘   └─────────────┘   └─────────────┘   └─────────────┘   └─────────────┘
```

### 1. Identify
Understand the organization's environment to manage cybersecurity risk to systems, assets, data, and capabilities.

**In this context:** Conduct regular audits of internal networks, systems, devices, and access privileges to identify potential security gaps — such as the unconfigured firewall that allowed this attack to succeed.

---

### 2. Protect
Develop and implement appropriate safeguards to ensure delivery of critical services and limit the impact of a potential security event.

**In this context:** Implement policies, procedures, training, and technical tools that reduce the likelihood and impact of future ICMP flood or similar DoS attacks — including the firewall rate-limiting rule and source IP verification already deployed.

---

### 3. Detect
Develop and implement appropriate activities to identify the occurrence of a cybersecurity event as quickly as possible.

**In this context:** Improve monitoring capabilities so that abnormal traffic patterns — like a sudden spike in ICMP packets — are detected in real time rather than only after services have already gone down. Network monitoring software and IDS/IPS systems are key tools here.

---

### 4. Respond
Develop and implement appropriate activities to take action regarding a detected cybersecurity incident — containing, neutralizing, and analyzing it.

**In this context:** Document and improve the incident response process used during the attack, including the steps taken to block ICMP traffic, take non-critical services offline, and restore critical services. Define clear response playbooks for similar future events.

---

### 5. Recover
Develop and implement appropriate activities to restore capabilities or services that were impaired due to a cybersecurity incident.

**In this context:** Restore all affected systems to normal operation, recover any data or services impacted during the two-hour outage, and communicate with internal stakeholders. Incorporate lessons learned to improve resilience against future incidents.

---

## Why the NIST CSF Is the Right Tool for This Situation

The NIST CSF is particularly well-suited to this scenario for three reasons:

1. **It is incident-agnostic** — the five functions apply regardless of the type of attack, making it a stable framework for building a general security strategy rather than just patching the specific vulnerability exploited.

2. **It covers the full lifecycle** — from proactive risk identification and protection, through real-time detection and response, to post-incident recovery. This prevents organizations from focusing only on the most visible phase (response) while neglecting the others.

3. **It scales to any organization size** — the framework is designed to be applicable to small and medium-sized businesses, not just large enterprises, making it appropriate for a company of this type.

---

*Document created as part of a professional cybersecurity portfolio.*
