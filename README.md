<p align="center">
<img src="https://i.imgur.com/ARGlAoP.png" height="100%" width="100%" alt="install kali"/>
</p>
<h1>Networking Anomalies: The Packet Capture Edition [NG]</h1>

    Our security analyst has been busy with other work recently and is in need of some assistance. She needs help viewing some network traffic logs to identify if any weird activity is going on that could potentially be threats to our network resources. We need you to review a set of logs and identify malicious activity if it exists.

<h2>Solution</h2>

    The challenge is asking me to look at suspicious packet captures of traffic over the network. Then to flag them on their website for further review later.

    First I logged onto the Security-Desk VM and on the Desktop I saw 3 files. 10.7, 20.0, and 30.21 which were all pcap files. They all opened with Wireshark. The website was called "webforms.daswebs.com" which is where I will report my analysis too. 

    10.7.pcap  
    After opening it, I briefly scrolled through and just kind of looked at how many packets there were, the difference in colors, protocols, re-occurring IP's, etc. Then I clicked on Expert Information under the Analyze tab and Protocol Hierarchy, Conversations, and Endpoints under the Statistics tab. I mostly noticed that IP's 172.16.30.109 and 172.16.10.7 were talking to each other the most with 2,035 packets between them. I decided to check those out a little more closely. Then I noticed that IP 172.16.30.109 was sending mass ARP requests to a wide range of IPs across a network which seemed odd. The first ARP request was at packet #41 and at packet #3088, 172.16.30.109 initiated the TCP-3-way handshake with 172.16.10.7 by sending a SYN connection establish request over port 80. I scrolled to the very bottom of the packet capture file and last saw the suspect IP in packet #6670. The rogue IP is 172.16.30.109 and the range is packets 41-6670.

    20.0.pcap
    Upon opening this new pcap file I kind of followed a similar thought process in the previous pcap file. Looked at the length, reoccurring IPs, color coding, the Expert Information, etc. Looking at the Expert Information I saw over 2000 SYN connection requests and 2000+ RST/ACK connection reset packets. This definitely seemed odd. IPs 172.16.30.9, 172.16.20.2, and 172.16.20.4 all had have some relationship. 172.16.30.109 initiated SYN connection request with 172.16.20.2 and 172.16.20.4 replied with SYN/ACK. Then 172.16.30.109 proceeded to mass spam 172.16.20.2 with SYN requests and 172.16.20.4 kept sending RST/ACK. This to me seemed like IP 172.16.30.109 was trying to bring down a server with SYN flooding? I flagged IP 172.16.30.109 as malicious, and I first saw that IP at packet 77 and last saw it at 8801.

    30.21
    Again, same process went into this one. I saw IP 172.16.30.109 was sending thousands of ARP requests similar to 10.7.pcap. Then I noticed 172.16.30.109 connected with 172.6.30.21 at packet 7601. They began communicating over SSH for a while and eventually at packet 8186 the connection was finished between them. I flagged IP 172.16.30.109 as malicious starting from packet 963-8186.

General Steps:

    Logged onto Security-Desk VM
    Located 3 pcap files on Desktop: 10.7, 20.0, 30.21
    Used Wireshark for analysis
    Reporting to: webforms.daswebs.com

10.7.pcap:

    Initial Overview
        Scanned packets, checked colors, protocols, recurring IPs

    In-Depth Analysis
        Checked "Expert Information" and "Statistics" tabs
        Noticed heavy interaction between 172.16.30.109 and 172.16.10.7 (2,035 packets)

    Findings
        IP 172.16.30.109 sending mass ARP requests
        TCP-3-way handshake initiated at packet #3088 over port 80

    Malicious Activity
        Rogue IP: 172.16.30.109
        Packet range: 41-6670

20.0.pcap:

    Initial Overview
        Similar process as 10.7.pcap

    In-Depth Analysis
        Over 2000 SYN requests and 2000+ RST/ACK packets
        IPs involved: 172.16.30.9, 172.16.20.2, 172.16.20.4

    Findings
        172.16.30.109 spamming 172.16.20.2 with SYN requests

    Malicious Activity
        Suspected SYN flooding by 172.16.30.109
        Packet range: 77-8801

30.21.pcap:

    Initial Overview
        Similar process as before

    In-Depth Analysis
        172.16.30.109 sending thousands of ARP requests

    Findings
        SSH communication between 172.16.30.109 and 172.6.30.21 starting at packet 7601

    Malicious Activity
        Rogue IP: 172.16.30.109
        Packet range: 963-8186

_____________________________
FIN

<h2>Report</h2>
<br />
<br />
<img src="https://i.imgur.com/jRtYIrB.png" height="80%" width="80%" alt="NICE Challenge"/>
<br />
<br />
<img src="https://i.imgur.com/HUN1PZp.png" height="80%" width="80%" alt="NICE Challenge"/>
<br />
<br />
<img src="https://i.imgur.com/DFPzDhD.png" height="80%" width="80%" alt="NICE Challenge"/>
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
