# Phishing-Analysis
Phishing
# Phishing Incident Response Report: Email Header & Artifact Analysis

## Executive Summary
This repository contains a professional Incident Response (IR) report documenting the technical investigation of suspicious emails flagged by enterprise users. Utilizing an isolated lab environment from TryHackMe (Phishing Analysis Tools), I analyzed raw email structures, parsed transport headers, evaluated domain authentication safety mechanisms, and checked infrastructure reputations to identify credential harvesting and delivery vectors.

---

## Tools & Threat Intelligence Platforms Used
* **PhishTool / Text Editors** — Email header parsing and metadata extraction.
* **CyberChef** — URL defanging, decoding obfuscated strings, and decoding Base64 artifacts.
* **VirusTotal & Cisco Talos** — Domain, URL, and IP reputation auditing.
* **Linux CLI (`sha256sum`, `grep`)** — File hash generation and pattern matching.

---

## Investigation Process & Technical Steps

### 1. Core Header Analysis & Authentication Auditing
For each suspicious email file (`.eml` / `.msg`), I extracted the raw internet headers to evaluate message authenticity and track transport hops.

* **Step:** Parsed the routing headers to locate the true origin server.
* **Authentication Verification:** Checked security protocols to look for configuration mismatches or bypasses:
  * **SPF (Sender Policy Framework):** Validated if the sending mail server's IP address was authorized by the domain owner.
  * **DKIM (DomainKeys Identified Mail):** Inspected cryptographic signatures to verify the message body was not modified mid-transit.
  * **DMARC:** Checked alignment policies to see if the message properly passed overall domain validation checks.

*📸 **[PORTFOLIO NOTE: Insert a screenshot here of PhishTool or your text editor showing the parsed SPF/DKIM/DMARC results]***

---

### 2. URL & Link Analysis (De-obfuscation)
Attackers frequently mask malicious endpoints using URL shorteners, open redirects, or look-alike domains (typosquatting).

* **Step:** Located hyperlinks embedded within the HTML body of the email.
* **Action:** I safely extracted the raw links without executing them and used **CyberChef** to defang the strings (e.g., converting `http://` to `hxxp://`) to ensure safe documentation.
* **Reputation Check:** Submitted extracted domains to **VirusTotal** and **Cisco Talos** to check global blocklists and identify active credential-harvesting kits.

*📸 **[PORTFOLIO NOTE: Insert a screenshot of your CyberChef workspace defanging a link, or a VirusTotal report flagging a malicious URL]***

---

### 3. Malicious Attachment Inspection
When emails contained attachments (such as fake invoices or urgent forms), I isolated the files to collect structural Indicators of Compromise (IoCs).

* **Step:** Located the file within the email attachment field or extracted its base64 text payload from the raw source code.
* **Action:** Ran a local hashing command on the file:
  ```bash
  sha256sum suspicious_attachment.pdf
  ```
* **Analysis:** Queried the resulting SHA-256 cryptographic hash against threat intelligence databases to check for historical sandbox detonations and classify the exact malware family (e.g., trojans, macro-enabled scripts).

---

## Discovered Indicators of Compromise (IoCs)

Below is an aggregated summary of the threat intelligence data extracted during the case investigations:

| Artifact Type | Observed Value / String | Classification / Threat Provider |
| :--- | :--- | :--- |
| **Sender IP** | `REDACTED_TRUE_ORIGIN_IP` | Spoofed Sender Infrastructure |
| **Phishing URL** | `hxxps://look-alike-domain[.]com/login` | Credential Harvesting Page |
| **File Hash** | `[Insert SHA-256 Hash Found in Lab]` | Known Malicious Downloader |

---

## Remediation & Defensive Playbook
Based on the tactics observed across these phishing vectors, I documented standard operational countermeasures:
1. **Block Malicious Infrastructure:** Implement immediate firewall and email gateway blocks on the identified malicious domains and sender IPs.
2. **Purge the Mailboxes:** Execute an enterprise-wide search across the mail server (e.g., M365 Exchange) to identify and purge identical messages from other employee inboxes.
3. **Reset Compromised Credentials:** Force an immediate password change and revoke active session tokens for any corporate users who interacted with the links prior to containment.

---

## Disclaimer
The email samples, malicious links, and files analyzed in this case study were processed within a secured, isolated lab infrastructure provided by TryHackMe. All artifacts are defanged and handled strictly for educational investigation and threat-analysis documentation purposes.
