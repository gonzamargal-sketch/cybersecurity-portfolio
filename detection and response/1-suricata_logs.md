# Suricata — Custom Rules and Log Analysis

> **Document type:** IDS/IPS practical lab  
> **Context:** Google Cybersecurity Certificate — Detection and Response module  
> **Skills demonstrated:** Suricata rule syntax, custom rule creation, PCAP-based traffic analysis, fast.log and eve.json log examination, jq JSON processing

---

## The Scenario

Working as a security analyst, the objective is to configure Suricata and trigger alerts by running custom rules against captured network traffic. The lab uses three key files:

| File | Purpose |
|---|---|
| `sample.pcap` | Packet capture file used to simulate network traffic |
| `custom.rules` | File containing the custom Suricata detection rules |
| `/var/log/suricata/fast.log` | Quick-check alert log (one line per alert) |
| `/var/log/suricata/eve.json` | Full event log in JSON format — the standard output for production use |

---

## Task 1: Examine a Custom Suricata Rule

Display the contents of the custom rules file:

```bash
cat custom.rules
```

Output:
```
alert http $HOME_NET any -> $EXTERNAL_NET any (msg:"GET on wire"; flow:established,to_server; content:"GET"; http_method; sid:12345; rev:3;)
```

### Rule Breakdown

A Suricata rule has three components: **action**, **header**, and **rule options**.

#### Action

The first keyword defines what Suricata does when the rule matches:

| Action | Behavior |
|---|---|
| `alert` | Generates an alert and logs the packet |
| `drop` | Generates an alert and drops the packet (IPS mode only) |
| `pass` | Allows the traffic through, can override other rules |
| `reject` | Drops the packet and sends a TCP reset to both endpoints |

In this rule, `alert` is used — the IDS inspects packets and sends an alert when the rule matches.

#### Header

```
http $HOME_NET any -> $EXTERNAL_NET any
```

| Field | Value | Meaning |
|---|---|---|
| Protocol | `http` | Rule applies only to HTTP traffic |
| Source IP | `$HOME_NET` | Variable defined in `suricata.yaml` representing the local network (`172.21.224.0/20`) |
| Source port | `any` | All ports |
| Direction | `->` | Traffic flowing from source to destination |
| Destination IP | `$EXTERNAL_NET` | Any address outside the home network |
| Destination port | `any` | All ports |

#### Rule Options

```
(msg:"GET on wire"; flow:established,to_server; content:"GET"; http_method; sid:12345; rev:3;)
```

| Option | Value | Meaning |
|---|---|---|
| `msg` | `"GET on wire"` | Alert message displayed when the rule triggers |
| `flow` | `established,to_server` | Match only packets in an established TCP connection going from client to server |
| `content` | `"GET"` | Look for the string `GET` in the HTTP method field |
| `http_method` | — | Restrict content match to the HTTP method specifically |
| `sid` | `12345` | Unique numeric signature ID |
| `rev` | `3` | Revision version of this rule |

**Summary:** This rule triggers an alert every time Suricata detects an HTTP GET request originating from the home network and directed to an external destination.

---

## Task 2: Trigger the Custom Rule

Check the log directory before running Suricata:

```bash
ls -l /var/log/suricata
```

The directory is empty — no logs exist yet.

Run Suricata against the packet capture file using the custom rules:

```bash
sudo suricata -r sample.pcap -S custom.rules -k none
```

| Option | Meaning |
|---|---|
| `-r sample.pcap` | Read traffic from the PCAP file instead of a live interface |
| `-S custom.rules` | Use the specified rules file (overrides default ruleset) |
| `-k none` | Disable checksum validation (not needed for captured traffic) |

Check the log directory again:

```bash
ls -l /var/log/suricata
```

Four files are now present, including `fast.log` and `eve.json`.

Display the alert log:

```bash
cat /var/log/suricata/fast.log
```

Output:
```
11/23/2022-12:38:34.624866  [**] [1:12345:3] GET on wire [**] [Classification: (null)] [Priority: 3] {TCP} 172.21.224.2:49652 -> 142.250.1.139:80
11/23/2022-12:38:58.958203  [**] [1:12345:3] GET on wire [**] [Classification: (null)] [Priority: 3] {TCP} 172.21.224.2:58494 -> 142.250.1.139:80
```

Two alerts were triggered — one for each HTTP GET request found in the captured traffic. Each line shows the timestamp, rule ID (`1:12345:3`), alert message, protocol, and the source/destination IP and port pair.

---

## Task 3: Examine the eve.json Output

Display the raw JSON log:

```bash
cat /var/log/suricata/eve.json
```

The raw output is difficult to read. Use `jq` to format it:

```bash
jq . /var/log/suricata/eve.json | less
```

The `jq` tool parses and pretty-prints the JSON. The first alert entry shows a **severity value of 3**.

Extract specific fields from all events:

```bash
jq -c "[.timestamp,.flow_id,.alert.signature,.proto,.dest_ip]" /var/log/suricata/eve.json
```

This command selects five fields from each event and outputs them as compact arrays:

| Field | Description |
|---|---|
| `.timestamp` | Time the event was recorded |
| `.flow_id` | Unique 16-digit ID shared by all packets in the same network flow |
| `.alert.signature` | The alert message from the matching rule |
| `.proto` | Protocol (e.g., TCP, UDP) |
| `.dest_ip` | Destination IP address |

The **destination IP of the last event** is `142.250.1.102`. The **alert signature of the first entry** is `GET on wire`.

Filter all events belonging to a specific network flow:

```bash
jq "select(.flow_id==X)" /var/log/suricata/eve.json
```

Replace `X` with any `flow_id` value from the previous query. All log records that share a `flow_id` belong to the same network conversation — this makes it easy to correlate related events across different log entries.

---

*Document created as part of a professional cybersecurity portfolio.*
