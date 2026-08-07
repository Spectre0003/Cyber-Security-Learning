# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.


# List of common network devices
    Hub
    Switch
    Router
    Bridge
    Repeater
    Gateway
    Modem
    Wireless access point
    Firewall
    Load balancer

# Hub

A hub connects multiple devices together in a LAN network. It behaves as a centralized connection point. It receives data from one device and broadcasts it to every device on the network. It operates at the physical layer of the OSI model.
Only the device that needs to receive the data keeps it while the others drop the data that was not addressed to them. It is very inexpensive, easy to install and suitable for small networks. But it also lacks security and has poor performance due to network congestion from constant broadcasts.

# Switch

A switch also connects multiple devices together in a LAN but it forwards the message only to the intended receiver instead of broadcasting it. A switch operates in the data link layer.
The switch builds a MAC address table and uses it to provide a low collision rate with better performance. 
The switch supports full duplex connection with dedicated bandwidth per port with high speed and scalability. But it is also more expensive as compared to hubs and may require configuration.

# Router

A router connects different networks and sends/forwards packets based on their IP addresses. It determines the best path for the travel of packets. Unlike the above two which work in the physical and link layer, a router works in the network layer of the OSI model.
It maintains a routing table and enables LANs to connect to the internet. It supports multiple routing protocols and improves security. On the other hand it is also more expensive and requires configuration to work.

# Bridge

A Bridge connects two or more LAN segments while filtering traffic based on their MAC addresses. It reduces traffic by forwarding frames only when required. It acts in the Data link layer.
Its goal is to reduce network congestion and improve performance, but it is slower than modern switches and rarely used today.

# Repeater

A repeater is a network device used to regenerate and amplify weak network signals. Due to attenuation, signals weaken over long distances, A repeater's job is to receive the weakened signal, regenerate it and forward a clean copy
A repeater is not expensive. It works in the physical layer. It does not filter or reduce traffic.

# Gateway

A Gateway is a device that acts like a translator which allows incompatible networks to communicate. It connects different types of networks or systems using different communication protocols.
A gateway can enable communication between networks that use different protocols or data formats.

# Modem

A modem (short for Modulator-Demodulator) is a device meant to enable communication between a local network and an Internet Service Provider. Its function is to convert digital signals into signals suitable for transmission and vice versa.
There are many types of modems, of which some are: DSL Modem, Cable Modem and Cellular Modem.

# Wireless Access Point

A Wireless Access Point enables wireless devices to connect to a wired network using Wi-Fi. It does not perform IP addressing nor does it provide internet access. It usually relies on a router for these functions.
it is used extensively due to its ability to expand Wi-Fi coverage and support multiple wireless devices.

# Firewall

A firewall is used to prevent unauthorized access while allowing legitimate communication. It is a security device that monitors, filters, and controls incoming and outgoing network traffic based on predefined security rules. 
Firewalls can be hardware devices, software applications or both.
It protects against unauthorized access, blocks malicious traffic and improves network security