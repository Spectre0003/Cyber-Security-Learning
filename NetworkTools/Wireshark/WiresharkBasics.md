# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is Wireshark?

Wireshark is an open-source, cross-platform network protocol analyser capable of investigating live packet traffic and inspecting captures. It has the ability to capture packets travelling on the network and investigate them with appropriate tools.
It assists in identifying suspicious or anomalous traffic. It can also investigate protocol details.

# Default Wireshark Window

There are five parts of the window that opens when launching wireshark:
    1. The Toolbar: Contains menus and tools for packet sniffing and processing.
    2. The Display Filter Bar: Its the main filtering section
    3. Recent Files: True to its name, it shows the recently accessed files.
    4. Capture Filter and Interfaces: Shows the available network interfaces and provides options to select an interface for packet capture.
    5. Status Bar: It displays information about the current capture or file, including the number of packets, packet statistics, and other status information.

# Main Wireshark Window

The main Wireshark window is alsodivided into several major components:
    1. Menu: Provides access to Wireshark's functions and configuration options.
    2. Main Toolbar: Provides shortcuts to commonly used operations.
    3. Filter Toolbar: Allows display filters to be entered and applied.
    4. Packet List Pane: Displays a summary of each captured packet.
    5. Packet Details Pane: Displays the protocols and fields contained in the selected packet.
    6. Packet Bytes Pane: Displays the raw packet data as a hexadecimal dump alongside its ASCII representation.
    7. Packet Diagram Pane: Displays the selected packet as a diagram.
    8. Statusbar: Displays information about the current capture and program state.

# Loading a Pcap file (pcapng)

You can load a pcap file by either double clicking it through recent files or browse to it via File and Open.
Once it opens, there are three main components of the window.
The centre is the list of packets captured in the file.
The bottom left part is the packet details pane. It shows the protocol breakdown of a selected packet.
The bottom right is the Packet Bytes pane. It contains a hex dump, with the corresponding ASCII representation alongside it.

# Packet Capturing

Wireshark can capture live network traffic from a selected network interface. Selecting the appropriate interface and starting the capture causes Wireshark to begin collecting packets. The capture can be stopped using the Stop button. A stopped capture can also be restarted using the Restart function.
During a live capture, packets are displayed in the Packet List pane as they are received. The statusbar provides information such as the number of captured and displayed packets.

# Packet Dissection

Packet Dissection refers to investigating packert details by decoding available protocols and fields. 
You can click(or double click) any packet to open its details which will show up in the Packet Details pane.
Selecting a specific field in the Packet Details pane highlights the corresponding bytes in the Packet Bytes pane, allowing you to relate decoded protocol fields to their raw packet data.

# Packet Navigation

Wireshark numerically arranges packe4ts based on the time at which they were captured and assigns each packet a unique number. This makes it easier for one to investigate/reinvestigate certain packets.
You can also directly go to a certain packet by typing its number on the Go dropdown and selecting Go to packet.
Apart from using packet numbers, you can also use Find Packet under the Edit dropdown to search for specific texts, bytes, etc.
You can mark packets. Marked packets are displayed with a black background. This is helpful to note down suspicious packets. Keep in mind that martked packets reset after closing the file.
You can also add Packet comments by right clicking a certain packet and the navigating to Packet Comments. Packet comments do not reset after closing a file.
An important thing to note is pcapng capture format can store comments whereas a pcap cannot.
The time display can also be switched to show the acctual time and date of the packet instead of showing its capture time by choosing the right Time Display Format in the View dropdown.

# Packet Filtering

Wireshark has a powerful filter engine that helps analysts to narrow down the traffic. Wireshark has two types of filters: capture and display. Capture filters are used for capturing only the packets valid for the used filter. Display filters are used for viewing the packets valid for the used filter. 
The most basic way to filter traffic is right click on a packet field or select Apply as Filter under Analyze. 
You can also use Conversation Filter under Analyze to view only the packets related to a specific packet number.
Instead of hiding the unrelated packets, you can also display all of them and colour the ones related by using Colourise Conversation under the View dropdown.
You can also right click on a value under the packet details and click Apply as Coloumn to add the given column to the packet list panel.
By using the right click menu(or under Analyze dropdown) you can follow a particular stream (like TCP or HTTP). Packets from server are shown in blue and packets from the client are shown in red (For Follow Stream in particular).
Finally, you can also use the 'Apply a display filter' search bar to filter the network packets. You can filter the packets by protocol name, port, ip addresses and much more.