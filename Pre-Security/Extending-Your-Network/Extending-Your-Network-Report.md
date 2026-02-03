# Technical Report: Extending Your Network (TryHackMe)
**Date:** February 3, 2026
**Category:** Network Security / Infrastructure
**Tools:** Network Simulator, VPN Clients, Firewall (ACL) Managers
**Author:** Cybersecurity Junior Pentester

---

## 1. Executive Summary
This report summarizes the technical configurations and theoretical concepts explored in the "Extending Your Network" module. The objective was to understand how data travels between different network segments and how security devices (Firewalls, VPNs, Routers) can be used to control, secure, and monitor that traffic. Key focus areas include port forwarding, stateful vs. stateless inspection, and network segmentation via VLANs.

---

## 2. Technical Methodology

### 2.1 Port Forwarding & NAT
Port forwarding allows external devices to access services within a private network.
* **Mechanism:** Mapping a communication request from one address and port number combination to another while the packets are traversing a network gateway (NAT).
* **Security Implication:** While necessary for hosting services (Web, SSH), it increases the attack surface by exposing internal ports to the public internet.

### 2.2 Firewall Implementation (Stateful vs. Stateless)
A critical component of network defense is the firewall, which acts as a border security device.
* **Stateless Firewalls:** Inspect individual packets based on static rules (Source/Dest IP, Port, Protocol). They are faster but vulnerable to advanced techniques like IP fragmentation (Tiny Fragment Attack).
* **Stateful Firewalls:** Maintain a "state table" of active connections. They understand the context of the TCP Three-Way Handshake and automatically block packets that do not belong to an established session.
* **Configuration:** During the practical task, we implemented `DROP` rules. Unlike `REJECT` (which sends an ICMP error), `DROP` silently discards packets, making reconnaissance more difficult for an attacker.

### 2.3 VPN (Virtual Private Networks) & Encryption
VPNs create an encrypted tunnel over the public internet to connect separate networks securely.
* **Tunneling Protocols:**
    * **PPTP:** Easy to set up but uses weak encryption; deprecated for sensitive data.
    * **IPSec:** Robust encryption integrated into the IP layer; the industry standard for site-to-site connectivity.
* **Privacy & Jurisdictions:** Analyzed the impact of the **5/9/14 Eyes** intelligence alliances on data privacy and the importance of choosing VPN providers in neutral jurisdictions (e.g., Switzerland).

### 2.4 LAN Hardware & Segmentation (VLANs)
Network infrastructure is built upon Layer 2 and Layer 3 devices.
* **Routers (Layer 3):** Connect different networks and determine the optimal path for data using IP addresses.
* **Switches (Layer 2/3):** Facilitate communication within a single network.
* **VLANs (Virtual LANs):** A method of creating logical network segments on a single physical switch. 
    * **Security Value:** VLANs prevent unauthorized lateral movement. For example, isolating the "Accounting" VLAN from the "Sales" VLAN ensures that a compromise in one department does not automatically expose the other.

---

## 3. Practical Phase: Network Simulation
Using a network simulator, we verified the communication flow between disparate hosts.

### 3.1 TCP/IP Three-Way Handshake
The connection between `computer1` and `computer3` followed the standard synchronization process:
1. **SYN:** Initial request from source.
2. **SYN/ACK:** Response from destination acknowledging the request.
3. **ACK:** Final confirmation from source to establish the session.

### 3.2 Network Log Analysis
```text
[LOG] HANDSHAKE: Starting TCP/IP Handshake between computer1 and computer3
[LOG] HANDSHAKE: Sending SYN Packet from computer1 to computer3
[LOG] HANDSHAKE: computer3 received SYN Packet, sending SYN/ACK
[LOG] HANDSHAKE: computer1 received SYN/ACK, sending ACK
[LOG] HANDSHAKE: Connection Established. Data transfer initiated.
```

---

## 4. Mitigation & Best Practices
* **Principle of Least Privilege:** Firewalls should be configured with a "Default Deny" policy, allowing only strictly necessary traffic.
* **Segmentation:** Implement VLANs to separate sensitive server environments from user workstations.
* **Encrypted Management:** Always use VPNs or SSH tunnels for remote administrative tasks to protect credentials from sniffing.

---

## 5. Key Takeaways
* **Visibility:** Understanding the difference between `DROP` and `REJECT` is vital for stealth and defense.
* **Logic over Hardware:** Networking is as much about logical rules (ACLs, VLANs) as it is about physical devices (Routers, Switches).
* **Hacker Mindset:** An attacker will always look for the weakest link in the "Extended Network," such as a poorly configured port forward or a stateless rule that can be bypassed by fragmentation.
