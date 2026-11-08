# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is tcpdump?

tcpdump is a command-line packet analyser. Like Wireshark, it captures packets travelling on a network interface, but instead of a graphical window it prints the packets directly in the terminal.
It is built on the libpcap library (the same library Wireshark uses for capturing), so files saved by tcpdump can be opened in Wireshark and vice versa.
Since it needs no GUI, it is extremely useful on servers, remote machines accessed over SSH, and lightweight Linux systems where Wireshark is not available.

# Installation and Privileges

On most Linux distributions it is either pre-installed or can be installed with:
    sudo apt install tcpdump
Capturing live traffic requires root privileges, so commands are usually run with sudo. Reading an already saved capture file does not require root.

# Listing Interfaces

Before capturing, you need to know which interface to listen on.
    sudo tcpdump -D
This lists all available interfaces (e.g. eth0, wlan0, lo). You can also use 'ip a' to see them.
The special interface 'any' captures on all interfaces at once.

# Basic Capturing

    sudo tcpdump -i eth0
This starts capturing on eth0 and prints every packet until you stop it with Ctrl+C. On a busy network this scrolls very fast, so it is usually combined with options and filters.

# Commonly Used Options

    -i <interface>  : Interface to capture on
    -c <number>     : Stop after capturing the given number of packets
    -n              : Do not resolve IP addresses to hostnames
    -nn             : Do not resolve hostnames or port numbers (shows 443 instead of https)
    -v, -vv, -vvv   : Increase verbosity (TTL, IP ID, checksums, etc.)
    -A              : Print the packet contents in ASCII
    -X              : Print the packet contents in both hex and ASCII
    -e              : Show the link-layer header (MAC addresses)
    -q              : Quick, less verbose output
    -tttt           : Show full date and time for each packet
    -w <file>       : Write raw packets to a file instead of printing them
    -r <file>       : Read packets from a saved file

Example:
    sudo tcpdump -i eth0 -nn -c 20
This captures 20 packets on eth0 without any name resolution. Using -n or -nn is a good habit, as DNS lookups slow tcpdump down and can themselves generate extra traffic.

# Saving and Reading Captures

    sudo tcpdump -i eth0 -w capture.pcap
This saves all captured packets to capture.pcap. Nothing is printed while writing to a file except the final packet count.
    tcpdump -r capture.pcap -nn
This reads the saved file and prints its packets. The file can also be opened in Wireshark for deeper analysis.
A common workflow is: capture with tcpdump on a remote server, copy the pcap to your machine, and analyse it in Wireshark.

# Understanding the Output

A typical line of tcpdump output looks like this:
    10:15:32.123456 IP 192.168.1.10.54321 > 93.184.216.34.443: Flags [S], seq 1234567, win 64240, length 0
Broken down:
    1. 10:15:32.123456      : Timestamp of the packet
    2. IP                   : Network layer protocol
    3. 192.168.1.10.54321   : Source IP and source port (the last number is the port)
    4. 93.184.216.34.443    : Destination IP and destination port
    5. Flags [S]            : TCP flags
    6. seq, win, length     : Sequence number, window size and payload length

The common TCP flags shown are:
    [S]  : SYN (connection request)
    [S.] : SYN-ACK (the '.' means ACK)
    [.]  : ACK
    [P.] : PUSH-ACK (data being sent)
    [F.] : FIN-ACK (connection closing)
    [R]  : RST (connection reset)
So the three-way handshake appears as [S], then [S.], then [.].

# Filtering Traffic

tcpdump uses BPF (Berkeley Packet Filter) syntax for filters, the same syntax as Wireshark's capture filters. The filter is written at the end of the command.

By host:
    sudo tcpdump -i eth0 host 192.168.1.10
    sudo tcpdump -i eth0 src host 192.168.1.10
    sudo tcpdump -i eth0 dst host 192.168.1.10
By network:
    sudo tcpdump -i eth0 net 192.168.1.0/24
By port:
    sudo tcpdump -i eth0 port 22
    sudo tcpdump -i eth0 portrange 20-25
By protocol:
    sudo tcpdump -i eth0 icmp
    sudo tcpdump -i eth0 udp
    sudo tcpdump -i eth0 arp

# Combining Filters

Filters can be combined using 'and', 'or' and 'not' (or &&, || and !).
    sudo tcpdump -i eth0 host 192.168.1.10 and port 80
    sudo tcpdump -i eth0 port 80 or port 443
    sudo tcpdump -i eth0 not port 22
When using brackets, wrap the filter in single quotes so the shell does not interpret them:
    sudo tcpdump -i eth0 'host 192.168.1.10 and (port 80 or port 443)'
The 'not port 22' filter is especially handy when you are connected over SSH, otherwise tcpdump also captures your own SSH session's traffic.

# Filtering by TCP Flags

tcpdump can match specific TCP flags:
    sudo tcpdump -i eth0 'tcp[tcpflags] & tcp-syn != 0'
This shows all packets with the SYN flag set.
    sudo tcpdump -i eth0 'tcp[tcpflags] == tcp-syn'
This shows packets with only SYN set, i.e. new connection attempts. A large number of these from one source to many ports is a typical sign of a port scan.

# Using tcpdump with Pipelining

Since tcpdump prints text, it can be piped into other commands. The -l option makes the output line-buffered so it passes through the pipe immediately.
    sudo tcpdump -i eth0 -nn -l port 53 | grep -i "A?"
This shows DNS queries being made on the network.
    tcpdump -r capture.pcap -nn | awk '{print $3}' | sort | uniq -c | sort -nr | head
This counts which source IP.port combinations appear the most in a capture file.

# tcpdump vs Wireshark

    tcpdump                          | Wireshark
    Command line                     | Graphical interface
    Very lightweight                 | Heavier on resources
    Works over SSH / on servers      | Needs a desktop environment
    Uses BPF filters only            | Has capture and display filters
    Basic protocol decoding          | Deep protocol dissection
Both are complementary. tcpdump is usually used to capture, and Wireshark to analyse in detail.

# Why does this matter in Cybersecurity?

    1. Incident Response: Quickly capturing traffic on a compromised server without installing anything heavy.
    2. Detecting Scans: Spotting floods of SYN packets or ICMP sweeps.
    3. Spotting Cleartext Credentials: Using -A on protocols like HTTP, FTP or Telnet shows data sent unencrypted.
    4. Verifying Firewall Rules: Checking whether traffic is actually reaching or leaving a host.
    5. Evidence Collection: Saving pcap files for later forensic analysis.
