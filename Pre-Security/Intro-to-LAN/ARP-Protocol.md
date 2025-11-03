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
