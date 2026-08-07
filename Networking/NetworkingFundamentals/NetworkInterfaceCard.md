# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is a Network Interface Card(NIC)?

A Network Interface Card is a hardware device that allows a device to communicate over and connect to a network. Every device whether wired or wireless requires a NIC to connect to a network. In modern computers and laptops, the NIC is usually integrated into the motherboard, so a separate NIC is often unnecessary.
Every NIC has a unique MAC Address assigned by the manfacturer.
It primarily operates at the physical and data link layer of the OSI model.

# Functions of a Network Interface Card

The major functions of a Network Interface Card are as follows:
    1. Connect a device to a network
    2. Send and receive data
    3. Provide a MAC address to the device
    4. Perform error detection on incoming frames
    5. Manage transmission speed and perform flow control

# Types of Network Interface Cards

There are two major types of NICs:
    1. Wired
    2. Wireless

Wired NICs use an Ethernet port and cable to connect to a network. They are fast, reliable, provide low latency and are more secure.
Wireless NICs use radio waves(Wi-Fi) instead of cables. They are mobile, hassle-free due to lack of cables and are easy to install.

# Working of a Network Interface Card

When a user requests a webpage, the web browser creates the request and passes it to the operating system. The operating system takes this data and passes it on to the NIC. The NIC encapsulates this data, adds necessary information(like MAC addresses), converts the data into signals and sends it through the network media. The destination NIC receives the signals, converts them back into data and passes it to the destination operating system. The process happens in reverse for a reply.