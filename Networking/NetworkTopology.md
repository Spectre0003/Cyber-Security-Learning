# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What do you mean by network topology?

Network topology refers to the arrangement of devices and the connections between them in a computer network. It describes how data flows in network. In topology, the devices are referred to as nodes and the connections between them are referred to as links.

Types of Network Topologies:
    -Bus Topology
    -Star Topology
    -Ring Topology
    -Mesh Topology
    -Tree Topology
    -Hybrid Topology

# Bus Topology

All nodes share a single communication link called the backbone. That is, All the devices on a bus topology share a single communication line. This is very easy to install and cheap, but has major disadvantages.
The failure of the link - the backbone, causes the entire network to go down. Data collisions are very common as all devices share a common communication bandwidth and the performance of this network decreases with the addition of devices.

# Star Topology

Every device on the network connects to a central hub or switch. It has high performance, is easy to install and troubleshoot and adding and removing devices is simple. On the other hand, more cabling is required and it is slightly more expensive than bus topology. Of course, If the switch fails, the communication network goes down once again.

# Ring Topology

Each device connects to exactly two other devices, forming a ring overall. Data travels usually in a single direction(both directions in dual-ring networks). This topology prevents collisions, but a single broken cable can still interrupt the network. Once established, it is also harder to expand with difficult troubleshooting.

# Mesh Topology

Every node (device) connects to every other node(Full Mesh), or every important/selective devices are connected(Partial Mesh). This topology is extremely reliable, even if one link breaks, it will not hinder the network as it has multiple paths for traversal of data. It also has excellent fault tolerance. But it has the highest cost out of all other topologies and is extremely complex to manage.

# Tree Topology

This topology is a combination of multiple star topologies connected in a hierarchy. It is highly scalable and very easy to manage. It is suitable for large organizations (like enterprises, companies and campuses) because of its scalability and easy fault isolation. Its downsides include complex configuration and more cabling.

# Hybrid Topology

This topology is a combination of more than one topology. It is flexible, scalable and can be optimised based on requirements. But it is expensive and very hard to maintain. Hybrid topologies are amongst the most commonly used topologies becuase it can combine the advantages of multiple different topologies.