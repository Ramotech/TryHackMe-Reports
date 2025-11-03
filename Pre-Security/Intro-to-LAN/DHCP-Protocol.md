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
