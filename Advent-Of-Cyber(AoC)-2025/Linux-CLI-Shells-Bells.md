# 🎄 Advent of Cyber 2025 - Day 1: Incident Response & Linux Forensics

**Date:** December 21, 2025
**Category:** Digital Forensics / Incident Response (DFIR)
**Platform:** TryHackMe - Advent of Cyber 2025
**Target System:** `tbfc-web01` (Linux Server)

## 1. Executive Summary
This report details the investigation into a security incident affecting the *The Best Festival Company* (TBFC) infrastructure. Following the suspected abduction of the CISO (McSkidy), the `tbfc-web01` server began exhibiting anomalous behavior.
The investigation successfully identified a malicious script responsible for data destruction and defacement. Furthermore, advanced forensic analysis of the file system and version control history allowed for the recovery of encrypted sensitive data, leading to the full restoration of the service.

## 2. Main Investigation: "One Giant Leap for McSkidy"

### 2.1 Situational Awareness & Reconnaissance
Upon accessing the compromised Linux server, initial enumeration was conducted to understand the environment and locate hidden artifacts left by the administrator.

* **Command:** `ls -la /home/mcskidy/Guides`
* **Finding:** A hidden file named `.guide.txt` was discovered using the `-a` (all) flag, revealing initial instructions and confirming the system was under surveillance.

### 2.2 Log Analysis & Threat Hunting
To identify the source of the attack, the system authentication logs were analyzed for suspicious activity.

* **Methodology:** Utilized `grep` to filter for failed authentication attempts within `/var/log/auth.log`.
    ```bash
    cd /var/log
    grep "Failed password" auth.log
    ```
* **Finding:** A massive volume of failed SSH login attempts was detected targeting the user `socmas`.
* **Attribution:** The attacks originated from the hostname `eggbox-196.hopsec.thm`, confirming an external brute-force attack from the "HopSec" network.

### 2.3 Malware Discovery & Analysis
Following the intrusion vector, the file system was scanned for recently created malicious scripts.

* **Command:** `find /home/socmas -name *egg*`
* **Artifact:** A malicious script named `eggstrike.sh` was located in `/home/socmas/2025/`.
* **Payload Analysis:**
    The script utilized piping (`|`) and output redirection (`>`) to:
    1.  **Exfiltrate Data:** Extract unique entries from `wishlist.txt` to `/tmp/dump.txt`.
    2.  **Denial of Service:** Delete the original business-critical data (`rm wishlist.txt`).
    3.  **Defacement:** Replace legitimate data with propaganda (`mv eastmas.txt wishlist.txt`).

### 2.4 Post-Exploitation Forensics
Privileges were escalated to `root` using `sudo su` to inspect artifacts inaccessible to standard users.

* **Evidence:** The root user's command history was intact.
    ```bash
    cat /root/.bash_history
    ```
* **Exfiltration Confirmation:** The history revealed the attacker used `curl` to upload the stolen `/tmp/dump.txt` to a remote server (`http://files.hopsec.thm/upload`).

---

## 3. Advanced Side Quest: The Encrypted Vault

An encrypted file (`mcskidy_note.txt.gpg`) was discovered, requiring a three-part key to decrypt. A forensic scavenger hunt was conducted to retrieve the fragments.

### 3.1 Fragment 1: Environment Variables
The first clue indicated the key was "riding with the session".
* **Action:** Executed `printenv`.
* **Result:** Variable `PASSFRAG1` found containing the string `3ast3r`.

### 3.2 Fragment 2: Version Control Forensics (Git)
The second clue pointed to a "ledger" that "remembers yesterday". A hidden `.secret_git` directory was found, indicating a Git repository.
* **Action:** Analysis of the commit history to find deleted secrets.
    ```bash
    cd ~/.secret_git
    git log -p
    ```
* **Result:** The `diff` output revealed a deleted line in a previous commit containing `PASSFRAG2: -1s-`. This demonstrates the critical vulnerability of committing secrets to version control.

### 3.3 Fragment 3: Steganography
The final clue suggested looking at the "tail" of "pixels".
* **Action:** Navigated to `~/Pictures` and inspected the binary data of an image file.
    ```bash
    tail bitmap.png  # (Simulated filename)
    ```
* **Result:** The string `PASSFRAG3: c0M1nG` was appended in plaintext at the end of the binary file.

### 3.4 Decryption & Remediation
* **Key Assembly:** `3ast3r-1s-c0M1nG`
* **Decryption:**
    ```bash
    gpg --batch --passphrase "3ast3r-1s-c0M1nG" -d mcskidy_note.txt.gpg
    ```
* **Service Restoration:** Using `root` privileges, the corrupted `wishlist.txt` was manually patched with the recovered backup data.
* **Final Verification:** The web application at port 8080 was validated, and a final ciphertext was decoded using `openssl` to retrieve the Side Quest completion flag.

---

## 4. Key Takeaways & Mitigation

1.  **Hidden Files:** Attackers (and admins) heavily rely on dotfiles (e.g., `.bash_history`, `.ssh`) to hide tracks or configs. Standard `ls` is insufficient for forensics.
2.  **Git Security:** Deleting a file from a repository does not remove it from the history. Secrets must be scrubbed from the entire commit log (`git filter-branch`) or never committed at all.
3.  **Log Monitoring:** The brute-force attack was noisy and easily detectable via `auth.log`. Implementing tools like **Fail2Ban** would have automatically blocked the attacker's IP after minimal failed attempts.
4.  **CLI Proficiency:** This engagement highlighted that GUI tools are often unavailable in server environments; mastery of `grep`, `find`, and standard streams (`|`, `>`) is mandatory for Incident Response.

---
*Author: [Ramotech]*
