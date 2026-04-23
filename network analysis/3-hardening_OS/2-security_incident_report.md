# Security Incident Report

---

## Section 1: Identify the Network Protocol Involved in the Incident

The protocols involved in this incident operate across multiple layers of the TCP/IP model:

- **DNS (Domain Name System)** — Application layer. Used at `14:18:32` to resolve `yummyrecipesforme.com` and again at `14:20:32` to resolve `greatrecipesforme.com`. Both queries are sent via UDP to `dns.google.domain` on port 53.
- **HTTP (HyperText Transfer Protocol)** — Application layer. Used to request and transfer content from both `yummyrecipesforme.com` and `greatrecipesforme.com` over port 80 (`.http`).
- **TCP (Transmission Control Protocol)** — Transport layer. Used to establish connections to both websites via the three-way handshake (`Flags [S]` → `Flags [S.]` → `Flags [.]`) before any HTTP data is exchanged.

The primary protocol affected by the attack is **DNS**, as the incident exploits the DNS resolution process to silently redirect the user from a legitimate website to a malicious spoofed one.

---

## Section 2: Document the Incident

### tcpdump log

```
14:18:32.192571 IP your.machine.52444 > dns.google.domain: 35084+ A? yummyrecipesforme.com. (24)
14:18:32.204388 IP dns.google.domain > your.machine.52444: 35084 1/0/0 A 203.0.113.22 (40)

14:18:36.786501 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [S], seq 2873951608, win 65495, options [mss 65495,sackOK,TS val 3302576859 ecr 0,nop,wscale 7], length 0
14:18:36.786517 IP yummyrecipesforme.com.http > your.machine.36086: Flags [S.], seq 3984334959, ack 2873951609, win 65483, options [mss 65495,sackOK,TS val 3302576859 ecr 3302576859,nop,wscale 7], length 0
14:18:36.786529 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 0
14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 73: HTTP: GET / HTTP/1.1
14:18:36.786595 IP yummyrecipesforme.com.http > your.machine.36086: Flags [.], ack 74, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 0

…<a lot of traffic on port 80>...

14:20:32.192571 IP your.machine.52444 > dns.google.domain: 21899+ A? greatrecipesforme.com. (24)
14:20:32.204388 IP dns.google.domain > your.machine.52444: 21899 1/0/0 A 192.0.2.17 (40)

14:25:29.576493 IP your.machine.56378 > greatrecipesforme.com.http: Flags [S], seq 1020702883, win 65495, options [mss 65495,sackOK,TS val 3302989649 ecr 0,nop,wscale 7], length 0
14:25:29.576510 IP greatrecipesforme.com.http > your.machine.56378: Flags [S.], seq 1993648018, ack 1020702884, win 65483, options [mss 65495,sackOK,TS val 3302989649 ecr 3302989649,nop,wscale 7], length 0
14:25:29.576524 IP your.machine.56378 > greatrecipesforme.com.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 0
14:25:29.576590 IP your.machine.56378 > greatrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 73: HTTP: GET / HTTP/1.1
14:25:29.576597 IP greatrecipesforme.com.http > your.machine.56378: Flags [.], ack 74, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 0

…<a lot of traffic on port 80>...
```

### Incident narrative

Several customers reported that when visiting `yummyrecipesforme.com`, they were prompted to download a file claiming to offer free recipes. After downloading and running the file, their browsers began automatically redirecting to a different website: `greatrecipesforme.com`. The website owner reported that they had noticed a former disgruntled employee may have accessed the admin panel using a brute force attack to obtain the administrator password and embed malicious code into the website's source.

The tcpdump log confirms this sequence of events. At **14:18:32**, the user's machine (`your.machine.52444`) sends a DNS query for `yummyrecipesforme.com` to Google's DNS server. The server responds with the legitimate IP address `203.0.113.22`. A full TCP three-way handshake follows at **14:18:36** (`Flags [S]` → `Flags [S.]` → `Flags [.]`), after which the browser sends an HTTP GET request to the website (`Flags [P.]`). A large volume of HTTP traffic on port 80 follows, consistent with content being downloaded from the site — likely the malicious file.

At **14:20:32** — approximately two minutes later, and without any user-initiated navigation — a new DNS query is sent, this time for `greatrecipesforme.com` (query ID `21899`). The DNS server resolves this domain to `192.0.2.17`, a completely different IP address from the original site. At **14:25:29**, the browser establishes a full TCP connection to `greatrecipesforme.com` on port 80 and sends an HTTP GET request, followed by another large volume of traffic on port 80.

The user did not manually navigate to `greatrecipesforme.com`. The redirect was triggered automatically — consistent with malicious code embedded in `yummyrecipesforme.com` that executed upon the file download, modifying the browser's DNS cache or host configuration to point to the attacker-controlled domain.

---

## Section 3: Recommend One Remediation for Brute Force Attacks

The root cause of this incident was unauthorized access to the website's admin panel via a **brute force attack**, which allowed the attacker to plant malicious code on the site.

**Recommended remediation: Implement multi-factor authentication (MFA) on all administrative accounts.**

Brute force attacks succeed by systematically guessing passwords until the correct one is found. MFA adds a second verification step — such as a one-time code sent to a mobile device or generated by an authenticator app — that the attacker cannot obtain even if they successfully guess the password. This means that knowledge of the correct password alone is no longer sufficient to gain access to the admin panel, directly neutralizing the effectiveness of brute force as an attack method.

Additional supporting measures to reinforce this remediation include:

- **Account lockout policies** — automatically lock an account after a defined number of failed login attempts (e.g., 5), forcing a waiting period or manual unlock before further attempts can be made. This limits the number of guesses an attacker can make within a given timeframe.
- **Strong password requirements** — enforce minimum length, complexity, and rotation policies to increase the time and resources required for a brute force attack to succeed. Disallowing previously used passwords prevents attackers from cycling back to known credentials.
- **Login attempt monitoring and alerting** — log all authentication attempts and trigger alerts when an unusual number of failed logins is detected from a single IP address, enabling early detection and response before an attacker successfully gains access.
- **Frequent mandatory password changes** — require administrative users to update their passwords on a regular schedule, reducing the window of time during which a compromised or guessed password remains valid.

---

*Document created as part of a professional cybersecurity portfolio.*
