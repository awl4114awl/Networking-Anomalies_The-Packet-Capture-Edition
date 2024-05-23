<p align="center">
<img src="https://i.imgur.com/ARGlAoP.png" height="100%" width="100%" alt="install kali"/>
</p>
<h1>Networking Anomalies: The Packet Capture Edition [NG]</h1>

    Our security analyst has been busy with other work recently and is in need of some assistance. She needs help viewing some network traffic logs to identify if any weird activity is going on that could potentially be threats to our network resources. We need you to review a set of logs and identify malicious activity if it exists.

## Tools Used

    Wireshark
    Security-Desk VM
    Web Browser
    Statistical Analysis Tools
    Protocol-specific Analysis Tools
    SSH Client

## Network Map
<img src="https://i.imgur.com/Ms9L7zn.jpeg" height="80%" width="80%" alt="NICE Challenge"/>

## Approach

    When addressing the challenge, the task entails examining suspicious packet captures of network traffic and subsequently marking them on their website for later review.

    Initiating the process, I accessed the Security-Desk VM, where I observed three files, namely 10.7, 20.0, and 30.21, all of which were in pcap format. These files were viewed using Wireshark. The website in question was "webforms.daswebs.com," where I intended to document my analysis.

## Analysis of 10.7.pcap

    Upon opening the file, I conducted a preliminary scan to assess various aspects such as packet count, color differences, protocols, and recurring IP addresses. Additionally, I utilized Wireshark's features, including Expert Information under the Analyze tab and Protocol Hierarchy, Conversations, and Endpoints under the Statistics tab. Notably, IP addresses 172.16.30.109 and 172.16.10.7 exhibited significant communication, comprising 2,035 packets between them. This prompted a closer examination, revealing anomalous behavior from IP 172.16.30.109, which was sending a large volume of ARP requests across the network. Furthermore, it initiated a TCP-3-way handshake with 172.16.10.7 on port 80. The packet analysis concluded with the identification of the suspect IP (172.16.30.109) within packets 41-6670.

## Analysis of 20.0.pcap

    Similarly, upon opening this file, I employed a comparable analytical approach. Noteworthy observations included an abnormal influx of SYN connection requests and corresponding RST/ACK connection reset packets. Further investigation revealed a pattern where IP 172.16.30.109 instigated SYN connection requests to IPs 172.16.20.2 and 172.16.20.4, eliciting SYN/ACK responses from the latter, followed by a barrage of SYN requests from 172.16.30.109 and continuous RST/ACK responses from 172.16.20.4. This behavior suggested a potential SYN flooding attempt aimed at disrupting server functionality. Consequently, IP 172.16.30.109 was flagged as malicious, with its activity spanning from packet 77 to 8801.

## Analysis of 30.21.pcap

    The examination of the third file followed a similar pattern. Notably, IP 172.16.30.109 exhibited a recurrent behavior of sending numerous ARP requests akin to the observations in 10.7.pcap. Additionally, it established a connection with IP 172.6.30.21, engaging in SSH communication. However, at packet 8186, the connection was terminated. Similar to the previous analyses, IP 172.16.30.109's activity was deemed malicious, spanning from packet 963 to 8186.

    Overall, the analysis of these pcap files revealed a consistent pattern of suspicious behavior associated with IP address 172.16.30.109, warranting further investigation and mitigation measures.

## General Steps:

    Logged onto Security-Desk VM
    Located 3 pcap files on Desktop: 10.7, 20.0, 30.21
    Used Wireshark for analysis
    Reporting to: webforms.daswebs.com

## 10.7.pcap:

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

## 20.0.pcap:

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

## 30.21.pcap:

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
