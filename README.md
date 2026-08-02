 # Phishing Incident Response Report: Email Header & Artifact Analysis

## Objective
The primary objective of this project was to perform a comprehensive incident response investigation on suspicious emails flagged in an enterprise environment. By analyzing raw email structures, parsing headers, verifying domain authentication mechanisms, and inspecting suspicious payloads in an isolated lab, this project demonstrates end-to-end phishing analysis, threat intelligence integration, and actionable remediation planning.

---

## Skills Learned
* **Email Header Analysis & Forensics:** Extracting true originating IPs, mapping mail transfer hops, and interpreting transport metadata.
* **Domain Authentication Auditing:** Verifying and diagnosing failures in **SPF**, **DKIM**, and **DMARC** policies to detect domain spoofing.
* **Safe Link De-obfuscation & Defanging:** Safely extracting embedded hyperlinks and using **CyberChef** to defang URLs (e.g., `hxxp://`) to prevent accidental execution.
* **Static Payload & Attachment Analysis:** Generating SHA-256 cryptographic file hashes using Linux CLI utilities to identify malware families.
* **Threat Intelligence Integration:** Querying global blocklists (**VirusTotal**, **Cisco Talos**) to confirm malicious infrastructure reputations.
* **Incident Containment & Playbook Execution:** Formulating operational countermeasures, including mailbox purging, perimeter blocking, and credential revocation strategies.

---

## Tools Used

<a href="https://any.run/" target="_blank">
  <img src="https://img.shields.io/badge/-ANY.RUN-0080FF?style=for-the-badge&logo=any.run&logoColor=white" />
<a href="https://gchq.github.io/CyberChef/" target="_blank">
  <img src="https://img.shields.io/badge/-CyberChef-A42E2B?style=for-the-badge&logo=cyberchef&logoColor=white" />
</a>
<a href="https://www.virustotal.com/" target="_blank">
  <img src="https://img.shields.io/badge/-VirusTotal-3949AB?style=for-the-badge&logo=virustotal&logoColor=white" />
</a>
<a href="https://talosintelligence.com/" target="_blank">
  <img src="https://img.shields.io/badge/-Cisco%20Talos-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white" />
</a>
<a href="https://www.linux.org/" target="_blank">
  <img src="https://img.shields.io/badge/-Linux%20CLI-FCC624?style=for-the-badge&logo=linux&logoColor=black" />
</a>
<a href="https://tryhackme.com/" target="_blank">
  <img src="https://img.shields.io/badge/-TryHackMe-212C42?style=for-the-badge&logo=tryhackme&logoColor=white" />
</a>

---

## Executive Summary

This repository contains a professional Incident Response (IR) report documenting the technical investigation of suspicious emails flagged by enterprise users. Utilizing an isolated lab environment from TryHackMe (*Phishing Analysis Tools*), I analyzed raw email structures, parsed transport headers, evaluated domain authentication safety mechanisms, and checked infrastructure reputations to identify credential harvesting and delivery vectors.

---

## Investigation Process & Technical Steps

### 1. Core Header Analysis & Authentication Auditing

For each suspicious email file (`.eml` / `.msg`), I extracted the raw internet headers to evaluate message authenticity and track transport hops.
* **Step:** Parsed the routing headers to locate the true origin server.
* **Authentication Verification:** Inspected security protocols to identify configuration mismatches or bypasses:
  * **SPF (Sender Policy Framework):** Validated if the sending mail server's IP address was authorized by the domain owner.
  * **DKIM (DomainKeys Identified Mail):** Inspected cryptographic signatures to verify the message body was not modified mid-transit.
  * **DMARC:** Checked alignment policies to verify overall domain validation compliance.
 <img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/8cd7e148-8926-472e-aa1a-9d086fc8b61b" />


---

### 2. URL & Link Analysis (De-obfuscation)

Attackers frequently mask malicious endpoints using URL shorteners, open redirects, or look-alike domains (typosquatting).
* **Step:** Located hyperlinks embedded within the HTML body of the email.
* **Action:** Extracted raw links safely without execution and used CyberChef to defang strings (e.g., converting `http://` to `hxxp://`) for secure documentation.
* **Reputation Check:** Submitted extracted domains to VirusTotal and Cisco Talos to check global blocklists and identify active credential-harvesting kits.

<img width="1366" height="655" alt="image" src="https://github.com/user-attachments/assets/31b8e252-561f-42cc-a902-abde159e6fbc" />

---

### 3. Malicious Attachment Inspection

When phishing emails contained suspicious attachments (such as fake invoices, payment updates, or financial spreadsheets), I isolated the files to perform static payload inspection and gather structural Indicators of Compromise (IoCs).

1. **Extraction & Local Hashing:** Isolated the attached files (`Payment-updateid.pdf` and `CBJ200620039539.xlsx`) and generated cryptographic SHA-256 hashes using the Linux CLI to establish file integrity and unique identification:

  <img width="1366" height="637" alt="image" src="https://github.com/user-attachments/assets/42270ad0-5824-4d21-ad6e-bf4ffc0e69da" />
   <img width="1366" height="628" alt="image" src="https://github.com/user-attachments/assets/8da2d136-21a0-4c78-90d8-fd39e7892b60" />
<img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/a7e8e308-6b9c-4008-89a1-210158fcf3dc" />
