# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is the TCP/IP Model?

Unlike the OSI Model, which is a conceptual framework, the Internet actually uses TCP/IP model for communication. TCP/IP stands for Transmission Control Protocol/Internet Protocol as its core lays in the TCP and IP protocols.

It divides communication into four major layers:
    4. Application Layer
    3. Transport Layer
    2. Internet Layer
    1. Network Access/Link Layer

Some sources separate the Physical layer from the Link layer, making it a five layer model.

# Application Layer

This layer provides network services to applications. It handles things like file transfer, web browsing and name resolution.

# Transport Layer

This layer handles the delivery of data between application on different devices.
The two main protocols are the TCP and UDP as discussed in the OSI Model.

# Internet Layer

This layer is responsible for moving packets between different networks using IP Addresses. It handles logical addressing, routing and path selection based on algorithms.

# Link Layer

This layer is responsible for communication of data within a physical medium. It handles MAC Addressing, packet framing and physical transmission.

# Relation between the OSI model and the TCP/IP model

The layers correspond to each other as follows:
    [OSI Model]                    [TCP/IP Model]
    7. Application Layer          - Application Layer
    6. Presentation Layer         - Application Layer
    5. Session Layer              - Application Layer
    4. Transport Layer            - Transport Layer
    3. Network Layer              - Internet Layer
    2. Data Link Layer            - Network Access Layer
    1. Physical Layer             - Physical Layer (if considered)