# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# Network Communication Methods

Communication methods describe how data is delivered from a sender to one or more receivers, and in which direction it can flow. They are covered here under three headings:
    1. Delivery Methods (Unicast, Broadcast, Multicast, Anycast)
    2. Transmission Modes (Simplex, Half Duplex, Full Duplex)
    3. Switching Methods (Circuit Switching, Packet Switching)

# Unicast

Unicast is one-to-one communication. A single sender transmits data to a single, specific receiver identified by its IP or MAC address.
Most everyday traffic is unicast: browsing a website, an SSH session, downloading a file.
Example: Your laptop (192.168.1.10) sending a request to a web server (93.184.216.34).

# Broadcast

Broadcast is one-to-all communication. A sender transmits data to every device on the local network segment.
In IPv4, the broadcast address is the last address of a subnet (e.g. 192.168.1.255 for 192.168.1.0/24), and 255.255.255.255 is the limited broadcast. At the MAC level the broadcast address is FF:FF:FF:FF:FF:FF.
Routers do not forward broadcasts, so a broadcast stays inside its own network. This boundary is called a broadcast domain.
Examples: ARP requests ("Who has 192.168.1.1?") and DHCP Discover messages.
IPv6 does not use broadcast at all; it relies on multicast instead.

# Multicast

Multicast is one-to-many communication. Data is sent only to a group of devices that have chosen to receive it, rather than to everyone.
IPv4 multicast uses the range 224.0.0.0 to 239.255.255.255. Devices join a group using IGMP (Internet Group Management Protocol).
It saves bandwidth because the sender transmits one copy and the network duplicates it only where needed.
Examples: IPTV streaming, routing protocols like OSPF (224.0.0.5), and mDNS (224.0.0.251).

# Anycast

Anycast is one-to-nearest communication. The same IP address is assigned to multiple servers in different locations, and the network routes each request to the closest (in routing terms) one.
Examples: Public DNS resolvers like 8.8.8.8 and 1.1.1.1, and CDNs (Content Delivery Networks).
Anycast also helps absorb DDoS attacks since the attack traffic is spread across many locations.

# Simplex

In simplex mode, data flows in only one direction. One device always sends and the other always receives.
Examples: Keyboard to computer, TV broadcasting, a computer sending data to a monitor.

# Half Duplex

In half duplex mode, data can flow in both directions, but only one direction at a time. Devices take turns.
Examples: Walkie-talkies, old hubs and early Ethernet. Because both sides share the channel, collisions can occur, which is handled using CSMA/CD (Carrier Sense Multiple Access with Collision Detection).

# Full Duplex

In full duplex mode, data flows in both directions at the same time.
Examples: Telephone calls, modern switched Ethernet. Since each direction has its own path, there are no collisions.

# Circuit Switching

In circuit switching, a dedicated path is set up between sender and receiver before communication starts, and it stays reserved for the entire session.
It provides constant speed and order, but wastes resources when the line is idle.
Example: Traditional landline telephone networks.

# Packet Switching

In packet switching, data is broken into small packets. Each packet carries the destination address and may take a different route; they are reassembled at the destination.
It uses the network much more efficiently and is how the Internet works. The downside is variable delay (latency and jitter) and possible packet loss.

# Why does this matter in Cybersecurity?

Many attacks abuse these methods:
    1. ARP Spoofing abuses broadcast ARP requests to poison a victim's ARP table.
    2. Smurf Attacks send ICMP requests to a broadcast address with a spoofed source IP, flooding the victim with replies.
    3. LLMNR/NBT-NS Poisoning abuses multicast/broadcast name resolution on Windows networks to capture credentials.
    4. DHCP Starvation and Rogue DHCP abuse broadcast DHCP messages.
Knowing what normal broadcast and multicast traffic looks like helps an analyst spot anomalies in tools like Wireshark. For example, the display filter
    eth.dst == ff:ff:ff:ff:ff:ff
shows all broadcast frames in a capture.
