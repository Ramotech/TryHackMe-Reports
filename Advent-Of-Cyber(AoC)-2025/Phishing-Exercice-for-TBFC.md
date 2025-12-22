# 🎄 Advent of Cyber 2025 - Day 2: Red Teaming & Social Engineering
**Category:** Red Teaming / Social Engineering / Phishing
**Platform:** TryHackMe - Advent of Cyber 2025
**Tools Used:** Social-Engineer Toolkit (SET), Python Custom Script, SMTP Relaying

## 1. Executive Summary
As part of the internal security assessment for *The Best Festival Company* (TBFC), a simulated phishing campaign was conducted to evaluate the security awareness of the staff regarding unsolicited emails and suspicious links.
The objective was to execute a **Credential Harvesting** attack using industry-standard tools to replicate a realistic threat scenario. The simulation successfully compromised target accounts, highlighting a critical vulnerability in the "Human Factor" of the company's security posture.

## 2. Technical Methodology

The attack was orchestrated using a "Split Infrastructure" approach: a listening server to capture data and a delivery system to send the bait.

### 2.1 The Trap: Fake Login Portal
Instead of a full web-server deployment, a lightweight Python script (`server.py`) was utilized to host the malicious landing page.
* **Function:** The script initializes an HTTP server on port `8000` listening on all interfaces (`0.0.0.0`).
* **Mechanism:** It is designed to accept `POST` requests. When a user submits the login form, the script intercepts the payload (Username/Password) and prints it to the attacker's terminal instead of forwarding it to a legitimate authentication service.

```bash
# Command used to start the listener
./server.py
# Output: Starting server on [http://0.0.0.0:8000](http://0.0.0.0:8000)
