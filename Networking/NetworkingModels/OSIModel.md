# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is the OSI Model?

The OSI Model (Open Systems Interconnection Model) is a framework that explains the travel of data from one device to another.
The OSI Model divides communication over 7 layers where each layer has a specific responsibility which also rely on the layers above and below it. 

The seven layers of the OSI model are as follows:
    7. Application Layer
    6. Presentation Layer
    5. Session Layer
    4. Transport Layer
    3. Network Layer
    2. Data Link Layer
    1. Physical Layer

When sending data, it moves down the layers and when receiving the data, it moves up the layers.

# Application Layer

This layer provides network services for applications. A few examples would be HTTPS, HTTP, FTP, SMTP etc.

# Presentation Layer

This layer is responsible for converting the data is such a way that both devices - the sender and the receiver can understand it. It encrypts, decrypts, compresses and formats the data. For example, TLS performs encryption for HTTPS connections.

# Session Layer

This layer is responsible for establishing, maintaining and terminating communication sessions between devices.

# Transport Layer

This layer is responsible for the transport of data. There are two major protocols used: TCP and UDP. TCP ensures reliable deivery, error checking and flow control wheareas UDP offers speed in exchange for reliable delivery.

# Network Layer

This layer is responsible for routing data between different networks using logical addresses like the IP Address. It handles the routing of packets and selecting their path.

# Data Link Layer

This layer is responsible for communication of data within the same local network. It uses MAC Addresses to deliver data frames between devices on the same local network.

# Physical Layer

This layer is responsible for transmitting raw bits of data over physical media like ethernet cables, fiber-optic cables and wireless radio signals.