# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What do you mean by Networking?

In the domain of Cybersecurty, learning about networking is very important. Networking refers to understanding how computers communicate with each other to exchange data. Almost every attack involves the travel of data over a network. Using this understanding allows you to identify possible malicious attacks.

# What is an IP Address?

In the real world, to uniquely identify any particular idividuals, we have fingerprints and iris scanners. Similarly, in the digital world, to uniquely identify network interfaces, we have IP Addresses.
For a computer to know where to send packets, it needs an IP Address. IP Address stands for Internet Protocol Address and there are two different types: IPv4 and IPv6.
The default format of an IPv4 address is x.x.x.x where x is any number between 0 and 255. These numbers are actually a representation of a binary octet.

For example: 10101010 represents 170
             170.81.22.1 represents 10101010 01010001 00010110 00000001

The numbers are for human representation. Computers read the binary digits.
An IPv6 Address uses 128 bits instead of 32 (in the case of IPv4) which allows a much bigger variety of nique addresses.
More about IP Addresses will be mentioned later.

# What is a MAC Address?

A MAC (Media Access Control) Address is used to differentiate network interfaces on a local level, like a LAN (Local Area Network). It is the physical address of a device given to it by the manufacturer.
It consists of six pairs of hexadecimal digits in the format xx:xx:xx:xx:xx:xx

The first three pairs represent the identity of the manufacturer (known as an 'Organisationally Unique Identifier - OUI) and the last three specify the device network interface.

An application usually knows only the IP Address of its destination and not the MAC Address. To resolve this, a protocol has been defined by the name of ARP (Address Resolution Protocol). More infoprmation about this will be given later.

# What is DNS?

DNS stands for Domain Name System. As discussed earlier, for a computer to communicate, it needs the IP Address of the recipient. It is obviously improbable to note down or remember the IP Addresses of every site one wants top visit.
The DNS allows us to forget about the IP Addresses and instead remember human readable text - known as domain names. By the DNS, domain names are converted into publically known IP Addresses.

For example: The IP Address of Google is 142.250.72.14 but it can also be reached using google.com (The IP of google.com changes depending on time and location).

Here, google.com is the domain name for 142.250.72.14.
