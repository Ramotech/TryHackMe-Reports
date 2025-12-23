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
2.2 Temporal Analysis (Identifying the Anomaly)

To pinpoint the timeframe of the attack, I performed a time-series analysis of the web traffic volume. A visualization of the logs revealed a massive spike in activity, deviating significantly from the baseline. This indicated the start of an automated scanning or brute-force phase.

SPL Query:
Snippet di codice

index=main sourcetype=web_traffic | timechart span=1d count | sort -count

    Finding: The attack window was clearly identified by a sudden 400% increase in HTTP requests.

2.3 Threat Hunting: Attacker Profiling

I analyzed the user_agent field to identify the nature of the client. By excluding standard browsers (Mozilla, Chrome, Safari), I uncovered the use of offensive security tools.

SPL Query:
Snippet di codice

index=main sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* | top limit=20 user_agent

Tools Identified:

    Havij: Automated SQL Injection tool.

    sqlmap: Exploitation tool.

    zgrab: Banner grabbing/scanner.

    curl/wget: Used for payload staging.

By correlating these tools with the client_ip field, I isolated a single malicious IP responsible for the attack.
3. Attack Chain Reconstruction
Phase 1: Reconnaissance

The attacker probed the web application for configuration files and common vulnerabilities.

    Artifacts: Requests for /.env, /.git, /config.

    Vulnerabilities Tested: Directory Traversal (../../etc/passwd) and Open Redirects (?redirect=http://evil.site).

Phase 2: Exploitation (SQL Injection)

The logs revealed a successful exploitation of the /item.php endpoint. The attacker utilized a specific payload to confirm database control.

Payload Identified:
HTTP

/item.php?id=1 AND SLEEP(5)--

Analysis: The injection of SLEEP(5) confirms a Time-Based Blind SQL Injection. The attacker forced the database to pause for 5 seconds, verifying execution privileges without needing visual feedback on the page.
Phase 3: Action on Objectives (RCE & Ransomware)

Following the breach, the attacker escalated to Remote Code Execution (RCE).

    Web Shell Upload: The file shell.php was accessed to execute system commands (cmd=whoami).

    Data Exfiltration: Successful downloads of backup.zip and logs.tar.gz.

    Ransomware Deployment: The final payload was executed via the web shell.

Evidence of Execution:
HTTP

GET /shell.php?cmd=./bunnylock.bin

Phase 4: Command & Control (C2) Correlation

To confirm the data exfiltration, I pivoted to the firewall_logs. I searched for outbound connections from the compromised web server (src_ip="10.10.1.5") to the attacker's IP.

Findings:

    Connection Status: ALLOWED

    Destination Port: 8080 (Non-standard web port)

    Volume: A summation of bytes_transferred confirmed a large amount of data leaving the network.

4. Remediation Recommendations

Based on the forensic findings, the following mitigation steps are recommended:

    Network Defense: Immediately block the identified Attacker IP at the perimeter firewall.

    Vulnerability Patching: Sanitize all inputs on item.php (and similar endpoints) using Prepared Statements to prevent SQL Injection.

    AppSec Hardening: Disable file execution in upload directories to prevent Web Shells from running.

    Egress Filtering: Restrict outbound connections from web servers; block traffic to non-standard ports like 8080.

Author: [Ramotech]
