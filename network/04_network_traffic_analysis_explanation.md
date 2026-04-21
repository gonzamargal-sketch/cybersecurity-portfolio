# Network Traffic Analysis — DNS & ICMP Incident

> **Document type:** Network protocol analysis and incident explanation  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** tcpdump log analysis, DNS/UDP/ICMP protocol knowledge, network layer troubleshooting, TCP/IP model application

---

## What is this document?

This document analyzes a real-world cybersecurity incident involving DNS resolution failure for the website `www.yummyrecipesforme.com`. Using the network protocol analyzer tool **tcpdump**, traffic was captured and inspected to identify which protocols were involved and what caused users to be unable to access the website.

---

## The Scenario

Multiple customers reported being unable to access `www.yummyrecipesforme.com`, receiving the error **"destination port unreachable"** after waiting for the page to load.

As a cybersecurity analyst, the investigation steps were:

1. Attempt to visit the website — same error confirmed.
2. Launch **tcpdump** to capture live network traffic.
3. Attempt to load the page again while the analyzer runs.
4. Inspect the captured packets to identify the root cause.

---

## The tcpdump Log

Below is the raw output captured during the investigation:

```
13:24:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? yummyrecipesforme.com. (24)
13:24:36.098564 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 254

13:26:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? yummyrecipesforme.com. (24)
13:27:15.934126 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 320

13:28:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? yummyrecipesforme.com. (24)
13:28:50.022967 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 150
```

---

## How to Read This Log

Each pair of lines represents one complete request-response cycle. Here is a breakdown of every field:

### Line 1 (outgoing UDP request):
```
13:24:32.192571  IP  192.51.100.15.52444  >  203.0.113.2.domain:  35084+ A? yummyrecipesforme.com. (24)
```

| Field | Value | Meaning |
|---|---|---|
| `13:24:32.192571` | Timestamp | Time the packet was captured: 1:24 PM, 32.19 seconds |
| `IP` | Protocol | Internet Protocol — network layer |
| `192.51.100.15.52444` | Source IP + port | Analyst's machine, using ephemeral port 52444 |
| `>` | Direction | Packet traveling from source to destination |
| `203.0.113.2.domain` | Destination IP + service | DNS server IP; `.domain` = port 53 |
| `35084+` | Query ID + flag | Unique DNS query ID; `+` indicates flags are set |
| `A?` | DNS record type | Request for an **A record** (domain → IP address mapping) |
| `yummyrecipesforme.com.` | Queried domain | The domain being resolved |
| `(24)` | Packet length | 24 bytes |

### Line 2 (incoming ICMP error response):
```
13:24:36.098564  IP  203.0.113.2  >  192.51.100.15:  ICMP 203.0.113.2 udp port 53 unreachable length 254
```

| Field | Value | Meaning |
|---|---|---|
| `13:24:36.098564` | Timestamp | Response arrived ~4 seconds after the request |
| `203.0.113.2` | Source IP | The DNS server is sending back an error |
| `192.51.100.15` | Destination IP | The error is sent back to the analyst's machine |
| `ICMP` | Protocol | Error message delivered via ICMP, not UDP |
| `udp port 53 unreachable` | Error message | Port 53 on the DNS server is not accepting connections |
| `length 254` | Packet length | Size of the ICMP error response |

---

## Protocols Involved

Three protocols interact in this incident, each operating at a specific layer of the **TCP/IP model**:

```
┌─────────────────────────────────────────────────────────┐
│               TCP/IP MODEL — PROTOCOLS IN USE           │
├──────────────────┬──────────────────────────────────────┤
│ Application      │ DNS — resolves domain to IP address  │
├──────────────────┼──────────────────────────────────────┤
│ Transport        │ UDP — carries the DNS query          │
├──────────────────┼──────────────────────────────────────┤
│ Internet         │ IP — routes packets between hosts    │
│                  │ ICMP — delivers error messages       │
├──────────────────┼──────────────────────────────────────┤
│ Network Access   │ (Ethernet / physical transmission)   │
└──────────────────┴──────────────────────────────────────┘
```

### Why UDP for DNS?
DNS uses **UDP** (User Datagram Protocol) by default because it is lightweight and fast. DNS queries are small and a single round-trip is usually sufficient. TCP is only used for DNS in cases where the response data is too large.

### Why does ICMP respond instead of DNS?
**ICMP (Internet Control Message Protocol)** is used by network devices to send error and diagnostic messages. When a UDP packet arrives at a host on a port where no service is listening, the operating system automatically sends back an ICMP **"port unreachable"** message. In this case, the DNS server received the UDP packet on port 53, but there was no DNS service running to handle it — so the OS returned the ICMP error.

### What is Port 53?
Port 53 is the **standard port for DNS services**. Both UDP and TCP use port 53 for DNS. When this port is unreachable, it means either:
- The DNS service has crashed or stopped running.
- A firewall is blocking traffic to port 53.
- The DNS server is misconfigured.

---

## Pattern Analysis — Three Failed Attempts

The log shows the same error occurring **three times**, approximately 2 minutes apart:

| Attempt | Request Timestamp | Response Timestamp | Response Length |
|---|---|---|---|
| 1 | 13:24:32 | 13:24:36 | 254 bytes |
| 2 | 13:26:32 | 13:27:15 | 320 bytes |
| 3 | 13:28:32 | 13:28:50 | 150 bytes |

Key observations:
- The **source port (52444) and query ID (35084)** are identical in all three requests — confirming it is the same session retrying.
- The **response length varies** across the three ICMP replies, which may indicate the server is under load or its state is changing.
- The DNS server IP (`203.0.113.2`) **is reachable** at the network level — it responds — but the DNS **service itself** is not functioning.

---

## Root Cause Hypothesis

The evidence points to one of the following causes:

1. **DNS service is down** — The most likely cause. The DNS daemon (e.g., BIND, Unbound) on server `203.0.113.2` has stopped or crashed, leaving port 53 without a listener.
2. **Firewall blocking port 53** — A firewall rule change may have blocked inbound UDP traffic on port 53 to the DNS server.
3. **Denial of Service (DoS) attack** — An attacker may have overwhelmed or deliberately taken down the DNS service.

---

## Personal Reflection

This exercise demonstrates a fundamental truth in network security: **connectivity and availability are not the same thing**. The DNS server was reachable at the IP level — it responded with ICMP — but the service layer was completely down.

Reading tcpdump logs requires understanding how protocols interact across layers. A symptom visible at the application layer ("website won't load") can only be properly diagnosed by dropping down to the network layer and inspecting the actual packet exchange.

This type of analysis is a core skill for any SOC analyst or network security professional. Tools like tcpdump, Wireshark, and similar packet analyzers are essential for moving beyond assumptions and working with evidence.

---

*Document created as part of a professional cybersecurity portfolio.*
