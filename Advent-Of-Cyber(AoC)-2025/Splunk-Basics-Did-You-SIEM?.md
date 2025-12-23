# 🎄 Advent of Cyber 2025 - Day 3: Log Analysis & Incident Response

**Category:** Blue Team / SOC / SIEM Forensics
**Platform:** TryHackMe
**Tools Used:** Splunk Enterprise, SPL (Search Processing Language)
**Scenario:** "King Malhare" Ransomware Investigation

## 1. Executive Summary

The "The Best Festival Company" (TBFC) Security Operations Center (SOC) detected a ransomware incident attributed to the threat actor known as "King Malhare". I was tasked with conducting a forensic analysis using **Splunk** to investigate the web server and firewall logs.

The investigation successfully reconstructed the complete attack kill chain. The adversary originated from a single external IP address, conducted automated reconnaissance using known hacking tools, and exploited a **Time-Based Blind SQL Injection** vulnerability. This access was leveraged to upload a web shell, exfiltrate sensitive backup data, and deploy the `bunnylock` ransomware.

## 2. Investigation Methodology

### 2.1 Data Ingestion & Triage
The investigation began by verifying the log sources ingested into the `main` index. A broad search identified two primary sourcetypes:
* `web_traffic`: HTTP logs containing paths, user agents, and status codes.
* `firewall_logs`: Network traffic logs showing source/destination IPs and allowed/blocked actions.

**SPL Query:**
```splunk
index=main sourcetype=* | stats count by sourcetype
