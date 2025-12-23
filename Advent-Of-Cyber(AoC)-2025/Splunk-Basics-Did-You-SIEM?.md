# 🎄 Advent of Cyber 2024 - Day 3: Log Analysis & Incident Response

**Category:** Blue Team / SOC / SIEM Forensics
**Platform:** TryHackMe
**Tools Used:** Splunk Enterprise, SPL (Search Processing Language)
**Scenario:** "King Malhare" Ransomware Investigation

## 1. Executive Summary

The "The Best Festival Company" (TBFC) Security Operations Center (SOC) detected a critical security incident involving a ransomware attack attributed to the threat actor "King Malhare". As a SOC Analyst, I was tasked with performing a forensic investigation using **Splunk** to analyze web server telemetry and firewall logs.

The investigation successfully reconstructed the full kill chain. The attacker originated from a single external IP address, leveraged automated scanning tools to identify a **Time-Based Blind SQL Injection** vulnerability, and used this entry point to upload a web shell. This access allowed for the exfiltration of sensitive data and the execution of the `bunnylock` ransomware. Post-exploitation analysis confirmed active Command & Control (C2) communication.

---

## 2. Technical Methodology

### 2.1 Data Ingestion & Initial Triage
The investigation commenced by verifying the log sources available in the SIEM. Accessing the `main` index revealed two distinct data sources pre-ingested for analysis:
* **`web_traffic`**: HTTP access logs containing request paths, methods, user agents, and status codes.
* **`firewall_logs`**: Network traffic logs indicating allowed/blocked connections and ports.

**SPL Query:**
```splunk
index=main sourcetype=* | stats count by sourcetype
```

### 2.2 Temporal Analysis (Anomaly Detection)
To identify the specific timeframe of the attack, I conducted a time-series analysis on the web traffic. Visualizing the event count over time revealed a significant anomaly: a massive spike in HTTP requests occurring over a specific 24-hour window, indicating the onset of an automated scanning or brute-force phase.

**SPL Query:**
```splunk
index=main sourcetype=web_traffic | timechart span=1d count | sort -count
```
> **Observation:** The traffic volume increased by over 400% compared to the baseline, pinpointing the attack window.


### 2.3 Threat Hunting: Attacker Profiling
I pivoted to analyzing the `user_agent` field to fingerprint the adversary. By filtering out legitimate browser traffic (Mozilla, Chrome, Safari), I exposed the use of offensive security tools.

**SPL Query:**
```splunk
index=main sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* | top limit=20 user_agent
```

**Identified Attack Tools:**
* **Havij:** An automated SQL Injection tool.
* **sqlmap:** A standard penetration testing tool for detecting SQL flaws.
* **zgrab:** A banner grabbing and scanning utility.
* **curl / wget:** Command-line tools used for staging payloads.

By correlating these malicious User Agents with the `client_ip` field, I was able to isolate a single malicious IP address responsible for **100%** of the hostile traffic.


---

## 3. Attack Chain Reconstruction

### Phase 1: Reconnaissance
The attacker initiated the campaign by probing the web application for sensitive configuration files and common vulnerabilities.
* **Targets:** Requests were observed for `/.env`, `/.git`, `/config`, and `/phpinfo.php`.
* **Techniques:** Directory Traversal attempts (e.g., `../../etc/passwd`) and Open Redirect tests (`?redirect=http://evil.site`).

### Phase 2: Exploitation (SQL Injection)
The logs revealed a successful exploitation of the `/item.php` endpoint. The attacker utilized a Time-Based Blind SQL Injection payload to verify database control.

**Detected Payload:**
```http
GET /item.php?id=1 AND SLEEP(5)--
```
**Analysis:** The injection of the `SLEEP(5)` command instructs the database to pause for 5 seconds. The server's response time confirmed the vulnerability, allowing the attacker to execute queries without needing visual feedback (blind injection).


### Phase 3: Action on Objectives (RCE & Ransomware)
Following the database compromise, the attacker escalated privileges to Remote Code Execution (RCE).
1.  **Web Shell Upload:** The file `shell.php` was accessed to execute arbitrary system commands (e.g., `cmd=whoami`).
2.  **Data Exfiltration:** Successful requests were recorded for sensitive archives `backup.zip` and `logs.tar.gz`.
3.  **Ransomware Deployment:** The final payload was executed via the web shell.

**Evidence of Execution:**
```http
GET /shell.php?cmd=./bunnylock.bin
```


### Phase 4: Command & Control (C2) Correlation
To confirm the success of the data exfiltration and ongoing control, I pivoted to the `firewall_logs`. I searched for outbound connections originating from the compromised web server (`src_ip="10.10.1.5"`) destined for the Attacker's IP.

**Findings:**
* **Status:** `ALLOWED`
* **Destination Port:** `8080` (A non-standard web port often used for C2).
* **Data Volume:** A summation of `bytes_transferred` confirmed a large amount of data was sent out of the network.

**SPL Query:**
```splunk
sourcetype=firewall_logs src_ip="10.10.1.5" dest_ip="[Attacker_IP]" action="ALLOWED"
```


---

## 4. Indicators of Compromise (IoCs)

| Type | Indicator | Description |
| :--- | :--- | :--- |
| **User Agent** | `Havij/1.17`, `sqlmap/1.7.9`, `zgrab/0.x` | Offensive tools used for scanning. |
| **File Artifact** | `shell.php` | Web Shell used for RCE. |
| **File Artifact** | `bunnylock.bin` | Ransomware executable. |
| **Network** | Port `8080` (Outbound) | Command & Control channel. |
| **Pattern** | `AND SLEEP(5)--` | SQL Injection Signature. |

---

## 5. Remediation & Recommendations

Based on the forensic findings, the following mitigation strategies are recommended:

1.  **Perimeter Defense:** Immediately block the identified malicious IP address at the firewall level.
2.  **Vulnerability Patching:** Sanitize all user inputs on `item.php` (and similar endpoints) using **Prepared Statements** to prevent SQL Injection.
3.  **Application Hardening:**
    * Disable file execution permissions in upload directories to prevent Web Shells from running.
    * Restrict the types of files that can be uploaded (allow-list approach).
4.  **Network Segmentation & Egress Filtering:** Implement strict firewall rules to block outbound connections from web servers to non-standard ports (like 8080) and untrusted external IPs.

---
*Report generated for Advent of Cyber 2025.*
