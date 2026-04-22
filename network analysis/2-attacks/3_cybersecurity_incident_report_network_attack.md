# Cybersecurity Incident Report: Network Attack Analysis

> **Document type:** Formal cybersecurity incident report  
> **Context:** Google Cybersecurity Certificate practical case study  
> **Skills demonstrated:** Attack classification, TCP handshake analysis, Wireshark log interpretation, impact assessment, remediation planning

---

## Section 1: Identify the Type of Attack That May Have Caused This Network Interruption

One potential explanation for the website's connection timeout error message is a **SYN flood denial of service (DoS) attack** targeting the company's web server (`192.0.2.1`).

The logs show that a single external IP address (`203.0.113.0`) began sending a large and continuous volume of TCP SYN packets to the web server on port 443. Unlike legitimate users — who complete the full three-way handshake (SYN → SYN-ACK → ACK) before making HTTP requests — the attacker's IP kept sending new SYN packets without ever completing the handshake. This created a growing backlog of half-open connections that consumed the server's available resources.

As the attack continued, the web server became progressively unable to process incoming requests from legitimate employee users (IP range `198.51.100.x`). The log entries shifted from a mix of normal green traffic and attack red traffic, to yellow entries reflecting failed legitimate connections, and finally to only red attack traffic — indicating the server had been fully overwhelmed.

This event is classified as a **direct DoS SYN flood attack** originating from a single source IP. It is not a distributed denial of service (DDoS) attack, as only one attacker IP address is present in the traffic log. The attack targets the transport layer of the TCP/IP model, exploiting the stateful nature of TCP connections to exhaust server resources without the need to complete any actual communication.

---

## Section 2: Explain How the Attack Is Causing the Website to Malfunction

**The TCP three-way handshake** is the process used to establish a reliable connection between a client and a server before any data can be exchanged. It works as follows:

1. **SYN** — The client sends a synchronization packet to the server, signaling its intent to establish a connection. The server receives this packet and reserves system resources (memory, connection table entry) in anticipation of completing the handshake.

2. **SYN-ACK** — The server responds with a synchronize-acknowledge packet, confirming it is ready to accept the connection and has reserved the necessary resources. It then waits for the client's final acknowledgment.

3. **ACK** — The client sends an acknowledgment packet, confirming receipt of the server's response. At this point the connection is fully established and data exchange — such as an HTTP GET request for a webpage — can begin.

**When a malicious actor sends a large number of SYN packets all at once**, the server dutifully responds to each one with a SYN-ACK and reserves resources for each anticipated connection. However, since the attacker never sends the final ACK, these connections remain in a **half-open state**. The server holds these open indefinitely while waiting for acknowledgments that will never arrive. As thousands of half-open connections accumulate, the server's connection table fills up completely. Once this limit is reached, the server can no longer accept new connection requests from any source — including legitimate users.

**The logs indicate** that beginning at log entry No. 52, the attacker's IP address (`203.0.113.0`) initiated repeated SYN requests to the web server at a rate of approximately three per second — far exceeding the pace of any legitimate user. The server initially responded normally, completing the first handshake and continuing to serve legitimate employee traffic in parallel (entries 55–62). However, as the flood of SYN packets continued without any corresponding ACKs, the server's available resources were progressively consumed.

By entries 73 and 77, the server began sending `[RST, ACK]` resets and `HTTP/1.1 504 Gateway Time-out` errors to legitimate employee connections — clear signs that it was failing to keep up. From log entry 125 onward, the web server ceased responding to all legitimate traffic entirely. Only attack packets from `203.0.113.0` appear in the remaining log entries, confirming that the server's connection capacity had been fully exhausted by the SYN flood.

The result for employees and customers was a **connection timeout error** when attempting to load the company's website — the browser sends a SYN packet, the server cannot respond with SYN-ACK, and after the timeout threshold is exceeded the browser gives up and displays the error. As long as the flood continues, no legitimate connection can be established.

---

*Document created as part of a professional cybersecurity portfolio.*
