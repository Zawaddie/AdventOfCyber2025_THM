# C2 Detection - Command & Carol
Explore how to analyze a large PCAP and extract valuable information.

---

The TBFC is very wary since the last series of attacks by the underlings of King Malhare. They are on full alert for anything happening. But they are getting restless; it is too quiet. Sir Elfo of the TBFC takes the initiative and proposes to hunt for Command and Control traffic using the meticulously collected network traffic. A majority of the TBFC elves object, we don't have the time to go through so much network traffic! Sir Elfo chuckles, don't fret, for I have a powerful tool to assist us! I present to you RITA, Real Intelligence Threat Analytics. We just need to convert our PCAP file to Zeek logs, and RITA will do the rest! Anyone can do it, just follow today's tasks.

## Learning Objectives
- Convert a PCAP to Zeek logs
- Use RITA to analyze Zeek logs
- Analyze the output of RITA

---

Below is a **clean, structured, copy-paste-ready THM GitHub write-up** in a standard **README.md** format. I’ve tightened wording, improved flow, and kept it aligned with how THM write-ups are usually presented.

---

# The Magic of RITA

**TryHackMe – Network Traffic Analysis**

## Overview

**Real Intelligence Threat Analytics (RITA)** is an open-source framework developed by **Active Countermeasures** to detect **command-and-control (C2)** activity by analysing network traffic. Rather than relying on signatures, RITA applies behavioural analytics to identify suspicious communication patterns that often indicate malware or post-compromise activity.

RITA is particularly effective at detecting low-and-slow C2 traffic that blends into normal network noise.

---

## Core Capabilities

RITA provides multiple detection and enrichment features, including:

* C2 beacon detection
* DNS tunnelling detection
* Long-lived connection detection
* Data exfiltration detection
* Threat intelligence feed correlation
* Severity-based connection scoring
* Counting internal hosts communicating with an external IP
* Identifying when an external host was first observed

---

## How RITA Works

The power of RITA lies in its **analytics engine**. It correlates and normalises multiple network attributes such as:

* Source and destination IPs
* Ports and protocols
* Timestamps
* Connection duration
* Data volume

Using this dataset, RITA evaluates behaviours commonly associated with malware and C2 infrastructure.

### Key Analytic Signals

RITA looks for patterns such as:

* Periodic or beacon-like connection intervals
* Excessive DNS requests
* Long or suspicious FQDNs
* Random or algorithmically generated subdomains
* Abnormal data volume over HTTPS, DNS, or non-standard ports
* Self-signed or short-lived TLS certificates
* Known malicious IPs/domains via threat intel feeds

---

## RITA Input Requirements

RITA **only accepts Zeek logs** as input.

### What Is Zeek?

**Zeek** is an open-source **Network Security Monitoring (NSM)** tool. It does not block traffic or use signatures like an IDS/IPS. Instead, it passively observes traffic via:

* SPAN ports
* Network taps
* Imported PCAP files

Zeek converts raw traffic into **structured, enriched logs** suitable for incident response and threat hunting.

Out of the box, Zeek provides:

* **Transaction data** (application-layer summaries)
* **Extracted content** (files and artifacts)

---

## Converting PCAPs to Zeek Logs

### Directory Structure

```bash
ubuntu@tryhackme$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
pcaps  zeek_logs
```

* **pcaps/** – Real-world malware PCAPs (Bradly Duncan collection)
* **zeek_logs/** – Output directory for parsed Zeek logs

---

### Parsing a PCAP

Convert a PCAP into Zeek logs using:

```bash
zeek readpcap pcaps/AsyncRAT.pcap zeek_logs/asyncrat
```

Zeek will process the PCAP and generate structured logs.

---

### Generated Zeek Logs

```bash
ubuntu@tryhackme$ cd zeek_logs/asyncrat && ls
conn.log  dns.log  http.log  ssl.log  x509.log
files.log known_hosts.log notice.log weird.log
```

These logs represent different protocol and behaviour observations. While detailed knowledge of each log is not required for RITA, they collectively form the dataset RITA analyses.

---

## Importing Zeek Logs into RITA

### Import Command

```bash
rita import --logs ~/zeek_logs/asyncrat/ --database asyncrat
```

During import, RITA:

* Parses Zeek logs
* Normalises fields
* Updates threat intelligence feeds
* Runs analysis modules

---

## Viewing RITA Results

```bash
rita view asyncrat
```

This launches RITA’s **terminal-based interface**, consisting of:

* Search bar
* Results pane
* Details pane

---

## RITA Interface Breakdown

### Search Bar

* Press `/` to enter search mode
* Use `?` to view available search fields
* Press `Esc` to exit search

---

### Results Pane

Each row represents a potentially suspicious connection. Key columns include:

* **Severity** – Calculated risk score
* **Source → Destination**
* **Beacon Likelihood**
* **Connection Duration**
* **Subdomain Count**
* **Threat Intel Matches**

Two notable findings in this dataset:

* `sunshine-bizrate-inc-software[.]trycloudflare[.]com`
* `91[.]134[.]150[.]150`

---

### Details Pane

#### Threat Modifiers

RITA uses modifiers to increase confidence in suspicious behaviour:

* **MIME type / URI mismatch**
* **Rare signature**
* **Low prevalence**
* **First seen recently**
* **Missing HTTP host header**
* **Large outbound data volume**
* **No direct connections**

These modifiers collectively influence the severity score.

---

#### Connection Info

Provides metadata such as:

* Connection count
* Total bytes sent
* Port, protocol, and service
* SSL/TLS usage

Non-standard ports or lack of encryption often warrant closer inspection.

---

## Analysis of Findings

### Entry 1 – Cloudflare Tunnel Domain

* Long and unusual FQDN
* Flagged as malicious on VirusTotal
* Rare TLS signature detected

This strongly suggests **C2 infrastructure hidden behind a tunnelling service**.

---

### Entry 2 – External IP

* IP flagged as malicious
* Long-lived connections
* Use of non-standard ports

These indicators point to potential **persistent C2 communication**.

---

## Important Considerations

* Smaller datasets increase false positives
* Severity score alone is not definitive
* Always validate findings with:

  * Threat intel
  * PCAP review
  * Zeek log pivoting

RITA is a **decision-support tool**, not an automated verdict engine.

---

## Final Challenge

You are now tasked with analysing a new capture:

```bash
~/pcaps/rita_challenge.pcap
```

### Objective

1. Convert the PCAP to Zeek logs
2. Import logs into RITA
3. Analyse the results
4. Identify suspicious indicators

Use the same workflow demonstrated above.

---

## Key Takeaways

* RITA excels at detecting behavioural C2 patterns
* Zeek provides the structured data RITA relies on
* Long connections, rare signatures, and low-prevalence hosts are strong indicators
* Threat hunting requires analyst judgement, not just scores






