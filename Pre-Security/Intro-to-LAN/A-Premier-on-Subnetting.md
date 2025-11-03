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
