# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is Network Architecture?

Network architecture defines how different devices communicate, how a network is managed and how data is transmitted. It explains the overall structure and design of a computer network.

The two most common network architectures are:
Client-Server Architecture 
Peer to Peer Architecture 

# Client-Server Architecture 

The client-server architecture is a network model in which one or more servers provide services or data to multiple clients. The devices, instead of communicating directly, communicate through the server.
The server stores and manages data and resources and clients can access these resources whenever required.

The communication between a client and a server happens like so:
The client sends a request. The server receives the request and processes it. The server sends back the requested data. The client receives the data and uses it.

This is a preferred architecture type due to its centralised management, better security and scalability. The only disadvantages are a single point of failure - the server, higher cost and it requires skilled administration.

# Peer to Peer Architecture 

The peer to peer architecture is a network model where all devices are equal. There is no dedicated central server. Each device can both request and provide resources directly to other devices.

The communication occurs as follows: 
A computer sends a request to another computer. The other computer receives the request and sends data directly to the computer who asked. There is no central communication involved.

Its advantages lie in low cost, easy setup and direct connection. On the other hand it also faces lower security, poor scalability and backup challenges.

# Hybrid Architecture

Most modern networks combine the advantages of both both client server architecture and peer to peer architecture. This approach aims to balance performance, scalability and flexibility.

A good example would be a company that has both a centralized server for requests, but its employees can still share data and resources with each other directly using their computers.
