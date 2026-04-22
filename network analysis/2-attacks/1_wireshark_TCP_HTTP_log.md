# How to Read a Wireshark TCP/HTTP Log

> **Document type:** Technical reference — Network traffic log analysis  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** Wireshark log interpretation, TCP three-way handshake, SYN flood attack pattern recognition, network protocol analysis

---

## What is Wireshark?

**Wireshark** is one of the most widely used network protocol analyzers in cybersecurity. It captures packets traveling to and from a network interface in real time and displays them in a structured, readable log format. Security analysts use it to inspect traffic, detect anomalies, and investigate incidents.

This document explains how to read and interpret a Wireshark TCP/HTTP log, using the specific log captured during the travel agency web server incident as a reference.

---

## Log Structure — The Five Columns

Every row in a Wireshark log represents a single packet. Each packet entry contains five key fields:

| Column | Description |
|---|---|
| **No.** | Sequential packet number since logging began |
| **Time** | Time in seconds (and milliseconds) since the capture started |
| **Source** | IP address of the machine that sent the packet |
| **Destination** | IP address of the intended recipient |
| **Protocol** | Network protocol used (TCP, HTTP, ICMP, etc.) |
| **Info** | Details about the packet: ports, flags, sequence numbers |

---

## Reading the Time Column

```
No.   Time
47    3.144521
48    3.195755
49    3.246989
```

The log in this activity starts at entry **No. 47**, recorded at **3.144521 seconds** after the capture began. This means approximately 47 packets had already been exchanged in just over 3 seconds — which is why Wireshark tracks time in **milliseconds**. Network traffic operates at speeds where even fractions of a second matter for analysis.

---

## Reading Source and Destination IP Addresses

```
Source           Destination
198.51.100.23    192.0.2.1
192.0.2.1        198.51.100.23
198.51.100.23    192.0.2.1
```

In this log:
- **`192.0.2.1`** — the company's **web server**
- **`198.51.100.0/24` range** — IP addresses belonging to **employee computers**
- **`203.0.113.0`** — the **attacker's IP address** (identified later in the log)

Packets flow in both directions. When a source is an employee and the destination is the web server, that is an outgoing request. When the source is the web server and the destination is an employee, that is a response.

---

## Reading the Protocol and Info Columns

```
Protocol   Info
TCP        42584->443 [SYN] Seq=0 Win=5792 Len=120...
TCP        443->42584 [SYN, ACK] Seq=0 Win=5792 Len=120...
TCP        42584->443 [ACK] Seq=1 Win=5792 Len=120...
```

### Protocol
The **Protocol** column shows which protocol is being used at that moment. In this log:
- **TCP** appears during connection establishment (transport layer)
- **HTTP** appears once the connection is successfully established and the browser makes a page request (application layer)

### Info — Ports
The Info column starts with the source port, an arrow, and the destination port:
- `42584->443` means a packet is going **from** port 42584 (the browser's ephemeral port) **to** port 443 (the web server's HTTPS port)
- **Port 443** is the standard port for encrypted HTTPS web traffic

### Info — TCP Flags
The flags in square brackets describe what type of TCP message this packet is:

| Flag | Full Name | Meaning |
|---|---|---|
| `[SYN]` | Synchronize | Initial connection request from client to server |
| `[SYN, ACK]` | Synchronize-Acknowledge | Server agrees to the connection and reserves resources |
| `[ACK]` | Acknowledge | Client confirms — connection is now established |
| `[RST, ACK]` | Reset-Acknowledge | Connection is forcibly terminated; sent when server cannot handle the request |

---

## The TCP Three-Way Handshake

Before any data (like a webpage) can be exchanged, TCP requires a **three-way handshake** to establish a reliable connection:

```
CLIENT                          SERVER
  |                               |
  |-------- [SYN] --------------->|   Step 1: "I want to connect"
  |                               |
  |<------- [SYN, ACK] -----------|   Step 2: "OK, I agree. Resources reserved."
  |                               |
  |-------- [ACK] --------------->|   Step 3: "Got it. Connection established."
  |                               |
  |-------- GET /sales.html ----->|   Now HTTP traffic can flow
  |<------- HTTP 200 OK ----------|
```

### Normal traffic example from the log:

| No. | Time | Source | Destination | Protocol | Info |
|---|---|---|---|---|---|
| 47 | 3.144521 | 198.51.100.23 | 192.0.2.1 | TCP | 42584->443 [SYN] |
| 48 | 3.195755 | 192.0.2.1 | 198.51.100.23 | TCP | 443->42584 [SYN, ACK] |
| 49 | 3.246989 | 198.51.100.23 | 192.0.2.1 | TCP | 42584->443 [ACK] |
| 50 | 3.298223 | 198.51.100.23 | 192.0.2.1 | HTTP | GET /sales.html HTTP/1.1 |
| 51 | 3.349457 | 192.0.2.1 | 198.51.100.23 | HTTP | HTTP/1.1 200 OK (text/html) |

The entire handshake takes a few milliseconds. Once complete, the browser sends an HTTP GET request for `sales.html` and the server responds with `200 OK` — success.

---

## The Color-Coding System

The log uses a color-coding system to make patterns easier to identify at a glance:

| Color | Meaning |
|---|---|
| 🟢 **Green** | Legitimate employee traffic (IP range `198.51.100.x`) |
| 🔴 **Red** | Attacker traffic (IP `203.0.113.0`) |
| 🟡 **Yellow** | Failed or disrupted connections — legitimate visitors who cannot connect |

---

## Reading the Attack Pattern

### Phase 1 — Initial attack (entries 52–62): Server still responding normally

The attacker (`203.0.113.0`) begins sending SYN requests. At first, the server responds normally:

| Color | No. | Source | Destination | Info |
|---|---|---|---|---|
| 🔴 Red | 52 | 203.0.113.0 | 192.0.2.1 | [SYN] — attacker initiates |
| 🔴 Red | 53 | 192.0.2.1 | 203.0.113.0 | [SYN, ACK] — server responds |
| 🔴 Red | 54 | 203.0.113.0 | 192.0.2.1 | [ACK] — attacker acknowledges |
| 🔴 Red | 57 | 203.0.113.0 | 192.0.2.1 | [SYN] — **another** SYN, abnormal |
| 🔴 Red | 59 | 203.0.113.0 | 192.0.2.1 | [SYN] — **another** SYN, keeps going |

Meanwhile, legitimate employees (green) can still connect and load `sales.html` successfully.

### Phase 2 — Server degrading (entries 63–83): First failures appear

The server starts struggling. Yellow entries appear — legitimate visitors begin receiving errors:
- **`[RST, ACK]`** — the server forcibly resets connections it cannot maintain
- **`HTTP/1.1 504 Gateway Time-out`** — the server takes too long to respond

### Phase 3 — Server overwhelmed (entries 119–152): Complete failure

From entry 125 onward, the web server **stops responding to legitimate traffic entirely**. Only red (attacker) entries appear in the log. The server's resources are completely consumed by the flood of SYN requests from `203.0.113.0`.

---

## Key Error Messages to Recognize

| Error | Meaning |
|---|---|
| `HTTP/1.1 504 Gateway Time-out` | A gateway server timed out waiting for the web server to respond |
| `[RST, ACK]` | The server is resetting connections it cannot handle — visitors get a timeout error |
| `[SYN, ACK]` with no following `[ACK]` | Half-open connection — server reserved resources but the handshake was never completed |

---

## Personal Reflection

Reading a Wireshark log for the first time can feel overwhelming due to the volume and speed of packets. The key is to focus on **patterns** rather than individual packets:

- Is one IP address sending far more packets than others?
- Are the packets all the same type (e.g., all `[SYN]`)?
- Are legitimate users starting to receive errors at the same time the anomalous traffic appears?

These three questions alone are enough to identify a SYN flood attack in a Wireshark log. The color-coding system used in this exercise simulates the kind of filtering and highlighting that real analysts apply in tools like Wireshark to make patterns visible at a glance.

---

*Document created as part of a professional cybersecurity portfolio.*
