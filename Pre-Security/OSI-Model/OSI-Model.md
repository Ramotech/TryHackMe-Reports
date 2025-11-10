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
