# How to Read a tcpdump Traffic Log

## The example log

This reading uses a real tcpdump capture to explain each element of the output.
The incident involves a user visiting `yummyrecipesforme.com` and being silently
redirected to a spoofed site (`greatrecipesforme.com`).

---

## Part 1 — DNS resolution request

```
14:18:32.192571 IP your.machine.52444 > dns.google.domain: 35084+ A? yummyrecipesforme.com. (24)
14:18:32.204388 IP dns.google.domain > your.machine.52444: 35084 1/0/0 A 203.0.113.22 (40)
```

**Line 1 — outgoing DNS query:**

| Element | Value | Meaning |
|---|---|---|
| `14:18:32.192571` | Timestamp | Time the packet was captured (hours:minutes:seconds.microseconds) |
| `IP` | Protocol | Internet Protocol — network layer |
| `your.machine.52444` | Source | User's machine using ephemeral port 52444 |
| `>` | Direction | Packet traveling from source to destination |
| `dns.google.domain` | Destination | Google's DNS server; `.domain` = port 53 |
| `35084+` | Query ID | Unique identifier for this DNS request |
| `A?` | Record type | Request for an A record (domain → IP address) |
| `yummyrecipesforme.com.` | Domain | The domain being resolved |
| `(24)` | Length | Packet size in bytes |

**Line 2 — DNS response:**

The DNS server replies to the same query ID (`35084`) with the resolved IP address:
`203.0.113.22`. The browser now knows where to send its connection request.

---

## Part 2 — TCP connection to the website (three-way handshake)

```
14:18:36.786501 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [S], seq 2873951608, win 65495, options [...], length 0
14:18:36.786517 IP yummyrecipesforme.com.http > your.machine.36086: Flags [S.], seq 3984334959, ack 2873951609, win 65483, options [...], length 0
```

- The source machine opens a connection to `yummyrecipesforme.com` on port **80** (`.http`)
using a new ephemeral port: **36086** (different from the DNS port used earlier).
- **`Flags [S]`** — SYN: the user's machine initiates the connection.
- **`Flags [S.]`** — SYN-ACK: the server acknowledges and accepts the connection.
The `.` inside the brackets means **acknowledgment**.

The communication between the two machines continues for approximately **2 minutes**,
as seen from the timestamps (`14:18` → next DNS request at `14:20`).

### TCP Flag reference

| Flag | Meaning |
|---|---|
| `[S]` | Connection **S**tart — SYN |
| `[F]` | Connection **F**inish — FIN |
| `[P]` | Data **P**ush — data is being sent |
| `[R]` | Connection **R**eset — connection abruptly terminated |
| `[.]` | **Acknowledgment** — receipt confirmed |

---

## Part 3 — HTTP GET request

```
14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [...], length 73: HTTP: GET / HTTP/1.1
```

Once the TCP connection is established, the browser sends an HTTP GET request to
download the page content from `yummyrecipesforme.com`.

| Element | Meaning |
|---|---|
| `Flags [P.]` | Data Push + Acknowledgment — actual data is being transferred |
| `HTTP: GET / HTTP/1.1` | Browser requests the root page using HTTP version 1.1 |
| `length 73` | The request payload is 73 bytes |

This is the point at which a malicious file download could be triggered if the
website has been tampered with.

---

## Part 4 — Sudden redirect to a spoofed site

```
14:20:32.192571 IP your.machine.52444 > dns.google.domain: 21899+ A? greatrecipesforme.com. (24)
14:20:32.204388 IP dns.google.domain > your.machine.52444: 21899 1/0/0 A 192.0.2.172 (40)
14:25:29.576493 IP your.machine.56378 > greatrecipesforme.com.http: Flags [S], seq 1020702883, win 65495, options [...], length 0
14:25:29.576510 IP greatrecipesforme.com.http > your.machine.56378: Flags [S.], seq 1993648018, ack 1020702884, win 65483, options [...], length 0
```

At `14:20` — about 2 minutes after the original visit — a **new DNS query** is sent,
this time for a completely different domain: `greatrecipesforme.com`.

Key indicators that something is wrong:

- **New domain, new IP:** The DNS server resolves `greatrecipesforme.com` to `192.0.2.172`,
a different IP from the original site (`203.0.113.22`). This was not requested by the user.
- **New source port:** The machine now uses port `56378` instead of `36086`,
indicating a brand new TCP session to the new destination.
- **No user action:** The user did not type a new address — the redirect happened automatically,
which strongly suggests the original website served malicious code (e.g., a JavaScript redirect
or a downloaded file that modified the browser's behavior).
- **Same DNS port reused:** Port `52444` is used again for the DNS query,
consistent with the same browser session making the request.

---

## Summary — What the full log tells us

| Timestamp | Event | Significance |
|---|---|---|
| `14:18:32` | DNS query for `yummyrecipesforme.com` | Normal — user visits the site |
| `14:18:32` | DNS response: `203.0.113.22` | Normal — IP resolved correctly |
| `14:18:36` | TCP handshake with `yummyrecipesforme.com` | Normal — connection established on port 80 |
| `14:18:36` | HTTP GET request | Possible malicious file download triggered here |
| `14:20:32` | DNS query for `greatrecipesforme.com` | ⚠️ Suspicious — user never requested this |
| `14:20:32` | DNS response: `192.0.2.172` | ⚠️ Different IP — spoofed site |
| `14:25:29` | TCP handshake with `greatrecipesforme.com` | ⚠️ Browser connects to the malicious site |

The tcpdump log provides a clear chronological record of the DNS poisoning or browser
redirection attack: the user visits a legitimate site, something on that site triggers
a silent redirect, and the browser ends up communicating with a spoofed domain without
the user's knowledge or consent.

---

*Document created as part of a professional cybersecurity portfolio.*
