# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is Nmap?

Nmap (Network Mapper) is an open-source tool used for network discovery and security auditing. It sends specially crafted packets to target hosts and analyses the responses to find out:
    1. Which hosts are up on a network
    2. Which ports are open on those hosts
    3. Which services (and their versions) are running on those ports
    4. Which operating system a host is likely running
Both attackers (during reconnaissance) and defenders (during audits) use it, which makes it one of the most important tools to learn early.
The official practice target 'scanme.nmap.org' is provided by the Nmap project specifically so people can test their scans against it.

# Installation

On Linux:
    sudo apt install nmap
On Windows, Nmap has an official installer which also includes Zenmap (a GUI for Nmap) and Npcap (the packet capture driver).
Many scan types need raw packet access, so they require sudo on Linux or an Administrator terminal on Windows.

# Basic Scan

    nmap 192.168.1.10
With no options, Nmap:
    1. Checks whether the host is up
    2. Scans the 1000 most common TCP ports
    3. Uses a SYN scan if run as root, or a TCP Connect scan if run as a normal user
    4. Prints the open ports along with the service name usually associated with each port

# Specifying Targets

    nmap 192.168.1.10                  : Single host
    nmap 192.168.1.10 192.168.1.20     : Multiple hosts
    nmap 192.168.1.1-50                : A range
    nmap 192.168.1.0/24                : A whole subnet (CIDR notation)
    nmap scanme.nmap.org               : A hostname
    nmap -iL targets.txt               : Read targets from a file
    nmap 192.168.1.0/24 --exclude 192.168.1.1   : Skip certain hosts

# Port States

Nmap reports each port in one of these states:
    open            : An application is actively accepting connections on this port
    closed          : The port is reachable but no application is listening
    filtered        : Nmap cannot tell whether it is open because a firewall or filter is blocking the probes
    unfiltered      : The port is reachable but Nmap cannot tell if it is open or closed (seen in ACK scans)
    open|filtered   : Nmap cannot decide between open and filtered (common in UDP scans)
    closed|filtered : Nmap cannot decide between closed and filtered (rare)

# Host Discovery

Host discovery (also called a ping scan) finds which hosts are alive, without scanning ports.
    sudo nmap -sn 192.168.1.0/24
On a local network, Nmap uses ARP requests for this, which is fast and very reliable. On remote networks, it uses ICMP echo, TCP SYN to port 443, TCP ACK to port 80 and ICMP timestamp requests.
Other discovery options:
    -Pn             : Skip discovery and treat every host as up (useful when hosts block pings)
    -PS<ports>      : TCP SYN ping
    -PA<ports>      : TCP ACK ping
    -PU<ports>      : UDP ping
    -PE             : ICMP echo ping

# Scan Types

TCP SYN Scan (-sS):
    sudo nmap -sS 192.168.1.10
Also called a half-open or stealth scan. Nmap sends a SYN; if it gets a SYN-ACK the port is open, and Nmap immediately sends an RST instead of completing the handshake. It is fast and is the default when run as root.

TCP Connect Scan (-sT):
    nmap -sT 192.168.1.10
Completes the full three-way handshake using the operating system's connect() call. It does not need root, but it is slower and more likely to be logged by the target application.

UDP Scan (-sU):
    sudo nmap -sU 192.168.1.10
Scans UDP ports such as DNS (53), SNMP (161) and DHCP (67). It is much slower since UDP has no handshake; Nmap mostly relies on ICMP 'port unreachable' replies to mark a port closed.

TCP ACK Scan (-sA):
    sudo nmap -sA 192.168.1.10
Does not find open ports. It is used to map firewall rules, showing which ports are filtered and which are unfiltered.

Null, FIN and Xmas Scans (-sN, -sF, -sX):
These send packets with no flags, only FIN, or FIN+PSH+URG flags respectively. Closed ports reply with RST while open ports usually stay silent. They can sometimes slip past simple non-stateful firewalls but do not work reliably against Windows hosts.

# Specifying Ports

    -p 22                : A single port
    -p 22,80,443         : A list of ports
    -p 1-1000            : A range
    -p-                  : All 65535 ports
    -F                   : Fast mode, top 100 ports
    --top-ports 20       : The top N most common ports
    -p U:53,T:80         : Specific UDP and TCP ports (with -sU and a TCP scan together)

# Service and OS Detection

    nmap -sV 192.168.1.10
Version detection. Nmap talks to each open port to find out the actual service and its version (e.g. 'OpenSSH 8.9p1' instead of just 'ssh'). Knowing exact versions is what lets you look up known vulnerabilities.
    sudo nmap -O 192.168.1.10
OS detection. Nmap compares how the host's TCP/IP stack responds against a database of known fingerprints to guess the operating system.
    sudo nmap -A 192.168.1.10
Aggressive scan. Enables OS detection, version detection, default scripts and traceroute all together. Very informative but also noisy.

# Nmap Scripting Engine (NSE)

NSE lets Nmap run Lua scripts to perform extra tasks like deeper enumeration, brute forcing and vulnerability checks.
    nmap -sC 192.168.1.10
Runs the default set of scripts (same as --script=default).
    nmap --script=http-title 192.168.1.10
Runs a specific script.
    nmap --script=vuln 192.168.1.10
Runs all scripts in the 'vuln' category.
Scripts are grouped into categories such as default, safe, discovery, auth, brute, vuln, exploit and intrusive.
On Linux, scripts are usually stored in /usr/share/nmap/scripts/.

# Timing Templates

Timing templates control how fast Nmap scans, from -T0 (slowest) to -T5 (fastest):
    -T0 : Paranoid
    -T1 : Sneaky
    -T2 : Polite
    -T3 : Normal (default)
    -T4 : Aggressive (common choice on reliable networks)
    -T5 : Insane
Slower scans are harder for an IDS to notice but can take hours. Faster scans may miss ports on slow or lossy networks.

# Output Options

    -oN scan.txt    : Normal output, human readable
    -oX scan.xml    : XML output, useful for other tools
    -oG scan.gnmap  : Grepable output, one host per line
    -oA scan        : All three formats at once
    -v / -vv        : Increase verbosity (shows results while the scan runs)
    --open          : Only show open ports
    --reason        : Show why a port was given its state (e.g. syn-ack, reset)

Example of grepable output with pipelining:
    grep "22/open" scan.gnmap | awk '{print $2}'
This prints only the IP addresses that have SSH open.

# Example Workflow

    1. sudo nmap -sn 192.168.1.0/24                     : Find live hosts
    2. sudo nmap -sS -p- -T4 192.168.1.10              : Find all open TCP ports on one host
    3. sudo nmap -sV -sC -p 22,80,443 192.168.1.10     : Enumerate only the open ports in detail
    4. sudo nmap -sU --top-ports 20 192.168.1.10       : Check the most common UDP ports
    5. Add -oA <name> to each step to keep a record of the results
Scanning all ports first and enumerating only the open ones is much faster than running -A against all 65535 ports.

# Why does this matter in Cybersecurity?

Offensive side:
    1. Reconnaissance is the first stage of most attacks and penetration tests.
    2. Finding outdated service versions points directly to possible exploits.
Defensive side:
    1. Auditing your own network for unknown hosts and unnecessary open ports.
    2. Verifying that firewall rules are actually doing what they should.
    3. Recognising scans in logs and captures. For example, a SYN scan shows up in tcpdump as many [S] packets from one source to many different ports, each answered with [S.] or [R.] and followed by an [R] from the scanner:
    sudo tcpdump -i eth0 -nn 'tcp[tcpflags] == tcp-syn'
