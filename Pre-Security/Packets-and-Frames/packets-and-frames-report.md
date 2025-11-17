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
