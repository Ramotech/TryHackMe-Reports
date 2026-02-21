# TryHackMe - Defensive Security Lab: Blocking a Malicious IP

## Scenario
During this hands-on lab, I detected suspicious activity from a specific IP address using open-source reputation tools. After investigating, I confirmed the IP was malicious and followed the correct escalation process within a simulated SOC environment. I then blocked the IP on the firewall to prevent further unauthorized access attempts.

## Steps taken
1. Identified unusual authentication attempts from IP: `143.110.250.149`.
2. Verified the reputation of the IP using an online scanner, confirming it was malicious.
3. Escalated the incident to the SOC Team Lead to request authorization to block the IP.
4. Implemented the block rule on the company firewall.
5. Confirmed the threat was mitigated successfully.

## Key concepts practiced
- Security event escalation process
- Using IP reputation services
- Basic network defense and firewall administration
- Incident response workflow

---

*This report is part of my ongoing study and documentation of cybersecurity labs on TryHackMe. The activity was carried out in a legal, simulated environment for educational purposes only.*
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
Subnetting Report

This document summarizes the main concepts and practical details about subnetting as learned during the TryHackMe module.
What is Subnetting?

Subnetting is the process of dividing a larger network into smaller, logical segments called subnets. Each subnet acts as a mini-network, which makes managing, securing, and routing traffic much easier and more efficient.

​
Why Is Subnetting Important?

    Efficient use of IP addresses: Instead of wasting addresses, subnetting allows assignment of suitable-sized address spaces for each segment of a network.

​

Improved network performance: Smaller networks mean less collision and broadcast traffic, enhancing stability and speed.

​

Security and control: Segments are isolated, so traffic can be filtered or managed between departments (for example, separating employees and guest WiFi).
Subnet and Subnet Mask

    IP Address: Written in dotted decimal (e.g., 192.168.1.8), composed of four octets (each 8 bits, for a total of 32 bits in IPv4).

​

Subnet Mask: Also 32 bits; it determines which portion of an IP address refers to the network and which to the host within that network (e.g., 255.255.255.0).

How Subnetting Works

    Subnetting “borrows” bits from the host portion of the address to create additional networks.

​

More bits borrowed = more subnets, but fewer hosts per subnet.

​

Subnetting is independent of physical layout and can be designed purely for logical organization (departments, functions, permissions).

    ​

    Routers use the subnet mask to route packets to the correct subnet.

Subnet Classes

    Traditionally, IP addresses were grouped into class A, B, and C for basic subnet/host allocation. Modern networks use flexible masks (CIDR) for more precise subnetting.

    ​

Practical Benefits

    Address Management: Avoids wasting addresses by allocating only as many as needed.

    Network Isolation: Limits broadcast traffic and improves security by separating subnetted areas.

    Scalability: Makes it easier to grow the network organization over time.

Summary Table: Subnetting Components
Type	Purpose	Example
Network Address	Identifies the subnet	192.168.1.0
Host Address	Device inside the subnet	192.168.1.100
Gateway Address	Route to external networks	192.168.1.254

In summary: Subnetting lets network administrators divide networks logically, optimize resource allocation, reduce congestion, and improve control. The subnet mask is essential for distinguishing network/host portions of IPs, and correct configuration ensures proper communication and routing between devices.
​
​
ARP (Address Resolution Protocol) is a fundamental protocol that I learned about through hands-on labs in TryHackMe. This protocol is essential for local network communication in modern networks.
What is ARP and Why Is It Important?

    ARP works at the network level to link a known IP address to its corresponding physical MAC address within the same LAN. This is necessary because, while IP addresses are used for logical addressing and routing, frames on Ethernet-based networks must be delivered via MAC addresses.

​

Every networked device keeps an ARP cache/table, a temporary memory area that stores recent associations between IP addresses and MAC addresses.
How Does ARP Work?

    ARP Request: When a device wants to communicate within its network and knows the destination IP but not its MAC address, it broadcasts an ARP Request to all devices in the LAN. The message is like: "Who has IP address X.X.X.X? Tell me your MAC address."

    ARP Reply: Only the device with that requested IP address replies, sending back its MAC address in a unicast ARP Reply directly to the requester.

    Caching: Once a mapping is learned, it is saved in the ARP cache for a short period (typically a few minutes), speeding up future communications.

Types of ARP Entries

    Dynamic: Added and removed automatically as ARP Requests/Replies happen; these entries expire after some time or when the device is shutdown.

    ​

    Static: Manually set and persist until the device is rebooted or the entry is deleted; used for special addresses, multicast, or configurations.

Key Scenarios

    Devices on the same LAN: ARP resolves the MAC address of the target IP so frames can be delivered directly at layer 2.

    Devices on different LANs/subnets: The sender's device uses ARP to find the MAC address of the gateway/router to forward the packet; routers use ARP for the next hop on each network segment.

    ​

Why ARP Matters

    It enables all Ethernet-based (and similar) networks to deliver packets by linking logical IP-level addressing and physical network delivery.

    Without ARP, a device could not send a frame to another unless it already knew the MAC address in advance.

Practical Skills Developed in TryHackMe Labs

    Viewing and analyzing the ARP cache with commands like arp -a.

    Interpreting how dynamic and static entries appear and expire.

    Understanding ARP's role in troubleshooting local network communication issues.
What is DHCP?

DHCP (Dynamic Host Configuration Protocol) is a network protocol that automatically assigns IP addresses and other network configuration parameters (like subnet mask, gateway, and DNS server) to devices joining a network. This makes device setup simpler and reduces manual configuration errors.
DHCP Operation: The DORA Process

When a new device connects to a network, if it has not been manually configured with an IP address, it uses DHCP to obtain its network settings. The DHCP procedure follows four key steps, commonly called the DORA process:

    Discover

        The device broadcasts a DHCP Discover message across the local network to find available DHCP servers.

        This message uses special addresses: source IP 0.0.0.0 (no IP yet) and destination IP 255.255.255.255 (broadcast to all).

    Offer

        Any DHCP server on the network responds with a DHCP Offer, providing an available IP address and other network details.

        The offer may include lease time, subnet mask, gateway, and DNS parameters.

    Request

        The client replies with a DHCP Request message, indicating it wants to accept the offered IP address and configuration.

        This is usually broadcast so all servers know which offer was accepted.

    Acknowledge (ACK)

        The server sends an Acknowledgment (DHCP ACK) message confirming the assignment.

        The client receives the network settings and can begin using the assigned IP address.

This process is automatic and typically takes just a few seconds whenever you connect a device to a managed network.
Key Points

    Dynamic assignment: IP addresses are leased for a limited period; they may change when the lease expires or the device disconnects.

    Efficiency: DHCP avoids IP conflicts, streamlines network setup, and centralizes address management.

    Parameters assigned: Along with the IP address, DHCP can distribute subnet mask, default gateway, DNS server addresses, and more.

Example Timeline

    Laptop joins network, sends DHCP Discover.

    DHCP server responds with DHCP Offer.

    Laptop accepts via DHCP Request.

    DHCP server confirms via DHCP ACK. Laptop now has an IP, gateway, DNS, etc., ready to use.
## Network Topologies & IP Configuration Lab
Bus, Ring, and Star Topologies

During this lab session, I explored fundamental network topologies—bus, ring, and star—and documented the main features and weaknesses of each.
Bus Topology

    All devices are connected to a single backbone cable.

    Data is sent in both directions down the backbone until it reaches its destination.

    If the backbone cable fails, all network communication stops.

Ring Topology

    Each device connects to two other devices, forming a circle.

    Packets travel from one device to the next until reaching the target.

    A major flaw: if any device or cable breaks, data can no longer circulate in the network.

Star Topology

    All devices connect individually to a central hub or switch.

    Every packet travels through the switch/hub.

    If the central device fails, the entire network goes offline, but if a single cable/device fails, only that device is affected.

Topology Flaws

During the simulation I discovered the weaknesses of each topology:

    In a bus topology, any break in the backbone brings down the whole network.

    In a ring topology, a single failure stops all communication.

    In a star topology, the central switch/hub is the critical point—if it goes down, so does the whole network.

IP Configuration and Interfaces
VMware Adapters

    VMware installs virtual network interfaces (such as VMnet1, VMnet8) that appear in the ipconfig output only if VMware is present on the PC.

    These adapters handle communication with virtual machines, not with the physical network.

Wi-Fi Adapter

    The "Wireless LAN adapter Wi-Fi" is my real network card, connected to the home or office router for Internet and LAN access.

Disconnected Adapters

    Physical Ethernet and Bluetooth adapters may appear as "Media disconnected" when not in use.

Private vs. Public IPs

    Private IP: Used inside a local network. It allows devices to communicate on LAN, but is not accessible directly from the Internet (e.g., 192.168.x.x).

    Public IP: Assigned by the Internet Service Provider (ISP) and is visible on the Internet. It allows communication with devices outside the local network.

Switch vs. Router

    Switch: Connects devices within the same network segment; it forwards packets only between the connected local devices.

    Router: Connects different networks (for example, a local network to the Internet), routes data between them, and assigns devices their local IP.

Summary

This project helped me visualize how different topologies work, analyze ipconfig output, understand virtual vs. physical network adapters, and clarify the differences between private/public IP addresses and switch vs router roles.
# Network Basics Lab: Using Ping & ICMP

## Scenario
In this lab, I used the `ping` command to test connectivity and latency between my system and various target IP addresses, both private and public.

## Steps Performed
1. Used `ping` to test the private IP address `192.168.1.254` and analyzed the response times and packet loss.
2. Repeated the test using the public address `8.8.8.8`, retrieving a lab-specific flag upon success.
3. Examined statistics such as round-trip time (RTT), packet transmission, and reception.

## Key Concepts Learned
- ICMP (Internet Control Message Protocol) basics
- Using the `ping` command for network diagnostics
- Interpreting timing statistics and packet loss to assess connectivity

## Screenshots
<img width="777" height="202" alt="immagine" src="https://github.com/user-attachments/assets/d11a78fa-ce11-4cb1-b86f-f0582c1d926c" />


---

*Lab completed in a TryHackMe simulated environment for practical training in basic network diagnostics.*
OSI Model — TryHackMe Learning Report

This document summarizes the core concepts of the OSI (Open Systems Interconnection) model as learned through the TryHackMe networking labs.
What is the OSI Model?

The OSI model is a conceptual framework that divides network communication into seven layers, each responsible for specific functions. This layered approach standardizes communication across diverse devices and protocols, enabling interoperability, ease of troubleshooting, and modular network design.
The 7 OSI Layers and Their Functions:
| Layer | Name             | Main Role                                                                 |
|-------|------------------|---------------------------------------------------------------------------|
| 7     | Application      | Interface for end-user applications, manages network services             |
| 6     | Presentation     | Data formatting, encryption, compression                                  |
| 5     | Session          | Manages sessions and connections between applications                    |
| 4     | Transport        | Provides reliable data transfer, error recovery                          |
| 3     | Network          | Logical addressing and routing (e.g., IP addressing and path selection)  |
| 2     | Data Link        | Physical addressing (MAC), framing, error detection on the local network  |
| 1     | Physical         | Transmission of raw bits over physical media                             |


    Encapsulation: Data moves down the layers at the sender, with each layer adding its own header. The reverse happens at the receiver.

    Layer-specific devices:

        Routers operate mainly at Layer 3 (Network layer), managing packet routing between different networks.

        Switches and NICs operate mainly at Layer 2 (Data Link layer), handling MAC addresses and local delivery.

    Addressing:

        IP addresses are used at Layer 3 for routing.

        MAC addresses are used at Layer 2 for local transmission.

Benefits of the OSI Model

    Enables devices and software from different vendors to communicate.

    Simplifies network design, development, and troubleshooting by isolating functions.

    Supports network scalability and flexibility through modular layer design.

Practical Insights from TryHackMe

    Observed real data encapsulation and decapsulation.

    Studied routing protocols and decision-making at Network layer.

    Learned the role of various network devices within the 7-layer structure.

This report serves as a foundational reference for understanding network communications through the OSI model, a critical concept in modern networking.
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

## Screenshots
Gobuster Output:
<img width="1622" height="646" alt="immagine" src="https://github.com/user-attachments/assets/6ba7ed22-45f6-43ee-9ebf-34d9ee4b7a5a" />


## Security Consideration (Write-up)
Finding hidden endpoints is crucial for penetration testing. In this case, the `/bank-transfer` endpoint was exposed, potentially allowing unauthorized money transfers. It's important that sensitive pages are properly secured and not discoverable via brute force or wordlists.

---

*This report is for educational purposes only. All activities were performed in a safe, simulated environment with permission, as provided by TryHackMe.*
Packets and Frames — TryHackMe Room Report

This report summarizes the concepts and practical labs completed in the TryHackMe room "Packets and Frames".
What Are Packets and Frames?

    Packets are units of data at Layer 3 (Network Layer) of the OSI model. 
    Each packet contains a payload and network information, such as source and destination IP addresses in its header. 
    Packets are used for routing data across multiple networks and can be reconstructed as part of a larger message.

    Frames operate at Layer 2 (Data Link Layer). 
    A frame encapsulates a packet and includes physical addressing (MAC addresses) required to deliver data within the local network. 
    Frames add reliability and formatting for transmission over network hardware like switches and NICs.

    Encapsulation: When sending data, each OSI layer adds its own information to the original message. 
    The packet is wrapped inside a frame for physical delivery. On arrival, layers strip away added headers until only the original data remains.

TCP/IP and UDP/IP

    TCP/IP (Transmission Control Protocol/Internet Protocol): Provides reliable, ordered, and error-checked delivery of data packets between devices. 
    TCP uses sequence numbers, acknowledgements (ACK), and a Three-way Handshake (SYN, SYN/ACK, ACK) for connection setup and integrity (using a checksum header).

        Connection-oriented: Ensures all packets arrive and are assembled in order.

    UDP/IP (User Datagram Protocol/Internet Protocol): Offers fast, connectionless transmission of packets. 
    UDP does not guarantee order, delivery, or error correction, making it suitable for real-time applications, streaming, or games.

Ports 101

    Ports are numerical endpoints between 0 and 65535 that allow devices to separate and manage different network services and applications. 
    Commonly used ports include:

        21: FTP (File Transfer Protocol)

        22: SSH (Secure Shell)

        80: HTTP (Web traffic)

        443: HTTPS (Secure web traffic)

        445: SMB (Server Message Block)

        3389: RDP (Remote Desktop Protocol)

    Ports between 0–1024 are the well-known standard ports for key protocols. 
    By convention, applications use these standard ports (e.g., HTTP on 80), but ports can be changed as required.

Practical Lab: Using Netcat (nc)

As part of the room, I completed a hands-on exercise using the command line tool netcat (nc) to connect to IP address 8.8.8.8 on port 1234. The command:

bash
nc 8.8.8.8 1234

successfully established a connection and returned the flag:

text
THM{YOU_CONNECTED_TO_A_PORT}

This lab demonstrated how to open a network connection to a specific port, reinforcing understanding of ports and basic client-server communication.

This report documents all key theory and practical steps completed in the "Packets and Frames" TryHackMe room and serves as a reference for concepts essential to networking and cybersecurity.
