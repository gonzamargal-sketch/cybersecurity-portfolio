# Network Attack Analysis — SYN Flood DoS Incident

> **Document type:** Network attack analysis and incident explanation  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** DoS vs DDoS classification, SYN flood mechanics, TCP/IP attack surface analysis, incident detection and initial response

---

## What is this document?

This document analyzes a cybersecurity incident affecting a travel agency's web server. A malicious actor launched a **SYN flood denial of service (DoS) attack** that made the company's sales webpage inaccessible to employees and customers. Using Wireshark packet capture data, the attack was identified, classified, and responded to.

---

## The Scenario

A security analyst at a travel agency receives an **automated alert** from the monitoring system indicating a problem with the web server. Attempting to visit the company's website returns a **connection timeout error**.

A packet sniffer (Wireshark) is launched to capture live traffic to and from the web server. The log reveals:

- A **large number of TCP SYN requests** arriving from a single unfamiliar IP address: `203.0.113.0`
- The web server is becoming overwhelmed and losing its ability to respond to traffic
- Legitimate employee connections are beginning to fail

**Immediate response actions taken:**
1. The web server is taken **offline temporarily** to allow it to recover
2. The company's **firewall is configured to block** the attacker's IP address (`203.0.113.0`)
3. The analyst notes that IP blocking is a **temporary measure** — the attacker can spoof other IP addresses to bypass it

---

## Type of Attack Identified: SYN Flood DoS

### What is a DoS attack?
A **Denial of Service (DoS)** attack is any attack designed to make a system, service, or network resource unavailable to its intended users. The goal is not to steal data, but to disrupt availability — one of the three pillars of the CIA triad (Confidentiality, Integrity, **Availability**).

### What is a SYN flood attack?
A **SYN flood** is a specific type of DoS attack that exploits the TCP three-way handshake. The attacker sends a massive number of SYN (synchronize) packets to the target server without ever completing the handshake. This causes the server to:

1. Receive a SYN and respond with SYN-ACK, **reserving memory and resources** for each connection
2. Wait for the final ACK that never arrives
3. Accumulate thousands of **half-open connections** until its connection table is full
4. Become unable to accept new connections from legitimate users

```
ATTACKER                        SERVER
  |                               |
  |-------- [SYN] --------------->|   Server reserves resources ✓
  |                               |
  |<------- [SYN, ACK] -----------|   Server waits for ACK...
  |                               |
  |   (no ACK is ever sent)       |   ...but it never comes ✗
  |                               |
  |-------- [SYN] --------------->|   Another one arrives
  |-------- [SYN] --------------->|   And another
  |-------- [SYN] --------------->|   And another
  |                               |
  |              SERVER OVERWHELMED — legitimate users cannot connect
```

### DoS vs DDoS — what is the difference?

| | DoS | DDoS |
|---|---|---|
| **Source** | Single IP address | Multiple IP addresses (often a botnet) |
| **Volume** | Lower (limited by one machine) | Much higher (distributed across many machines) |
| **Detection** | Easier — one IP to block | Harder — many IPs, often spoofed |
| **Mitigation** | Block the IP | Rate limiting, geo-blocking, CDN, scrubbing |
| **This incident** | ✅ Yes — single attacker IP `203.0.113.0` | ❌ No |

Since **only one IP address** (`203.0.113.0`) appears in the attack traffic, this is classified as a **direct DoS SYN flood**, not a DDoS.

---

## How the Attack Progressed — Three Phases

### Phase 1: Attack begins, server still functional
The attacker starts sending SYN requests. The server responds normally to the first handshake (entries 52–54). Legitimate employees can still connect and load `sales.html` successfully. The attack traffic and normal traffic coexist momentarily.

### Phase 2: Server begins to degrade
As SYN requests continue to arrive at an abnormal rate, the server's connection table starts filling up. Legitimate users begin experiencing:
- **`[RST, ACK]`** responses — the server resets connections it cannot maintain
- **`HTTP/1.1 504 Gateway Time-out`** — the gateway server times out waiting for the web server

Yellow-coded entries (failures) start appearing alongside green (legitimate) and red (attack) traffic.

### Phase 3: Complete service disruption
From log entry **125 onward**, the web server stops responding to all legitimate traffic. Only attacker traffic (`203.0.113.0`) appears in the log. The server's resources are completely exhausted. Employees and customers receive timeout errors and cannot access the website.

---

## Why Connection Timeout Errors Appear

When a user's browser tries to load the website, it initiates a TCP three-way handshake by sending a SYN packet. Normally the server responds with SYN-ACK within milliseconds. During a SYN flood:

- The server's connection table is full of half-open connections from the attacker
- New SYN packets from legitimate users cannot be processed
- The browser waits for a SYN-ACK response that never arrives
- After a timeout threshold is exceeded, the browser displays: **"connection timeout"**

This is exactly the error that employees and customers reported seeing.

---

## Protocols and Layers Involved

| Layer (TCP/IP) | Protocol | Role in the incident |
|---|---|---|
| Application | HTTP/HTTPS | Final layer — never reached due to the attack |
| Transport | TCP | Exploited by the SYN flood via the three-way handshake |
| Internet | IP | Used to route packets; attacker uses a single source IP |
| Network Access | Ethernet | Physical transmission layer — not directly affected |

The attack operates entirely at the **transport layer** by abusing TCP's stateful connection mechanism. This is what makes SYN floods effective — the server must allocate resources before it can verify whether the requester is legitimate.

---

## Immediate Response and Limitations

| Action | Effect | Limitation |
|---|---|---|
| Take server offline | Server recovers and resources are freed | Website remains inaccessible during downtime |
| Block attacker IP (`203.0.113.0`) | Stops current attack traffic | Attacker can **spoof different IPs** to bypass the block |

The IP block is a short-term patch, not a solution. Additional measures are required to prevent recurrence.

---

## Recommended Mitigations

1. **SYN cookies** — A technique where the server does not allocate resources until the three-way handshake is complete, eliminating the half-open connection problem.
2. **Rate limiting** — Restrict the number of SYN requests accepted from a single IP per second.
3. **Firewall rules** — Configure stateful firewalls to detect and drop excessive SYN traffic automatically.
4. **Intrusion Prevention System (IPS)** — Deploy an IPS that can detect SYN flood patterns in real time and block them dynamically.
5. **Content Delivery Network (CDN) / DDoS protection service** — Route traffic through a scrubbing service that filters attack traffic before it reaches the origin server.

---

## Personal Reflection

This incident illustrates a fundamental vulnerability in the TCP protocol itself. The three-way handshake was designed for reliability, not security — it assumes good faith from both parties. A SYN flood exploits that assumption by never completing its part of the exchange.

What makes this attack particularly instructive is the progression visible in the Wireshark log: the server does not fail instantly. There is a gradual degradation — first yellow entries appear alongside green, then green disappears entirely, then only red remains. Recognizing this degradation pattern early is critical, as it allows analysts to respond before the server completely stops functioning.

The fact that a single attacker with one IP address could take down a web server also underscores the importance of **proactive defenses** like rate limiting and SYN cookies, rather than reactive measures like IP blocking after the fact.

---

*Document created as part of a professional cybersecurity portfolio.*
