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
