# TryHackMe Room: FakeBank - Gobuster Report

## Room Info
- Platform: TryHackMe
- Room: FakeBank
- Tool used: Gobuster
- Task: Enumerate hidden directories/pages on a simulated bank site

## Objective
The goal of this task was to find hidden pages/directories on the fakebank.thm website using Gobuster and identify any sensitive endpoints.

## Gobuster Command Used
gobuster -u http://fakebank.thm -w wordlist.txt dir


## Output Example
/images (Status: 301)
/bank-transfer (Status: 200)

## Steps
1. Ran Gobuster with the specified wordlist to brute force common directory names.
2. Discovered the hidden [bank-transfer] page, which allowed access to sensitive account functions.
3. Used information gathered to proceed with the next step of the room/challenge.

## Security Consideration (Write-up)
Finding hidden endpoints is crucial for penetration testing. In this case, the `/bank-transfer` endpoint was exposed, potentially allowing unauthorized money transfers. It's important that sensitive pages are properly secured and not discoverable via brute force or wordlists.

---

*This report is for educational purposes only. All activities were performed in a safe, simulated environment with permission, as provided by TryHackMe.*

