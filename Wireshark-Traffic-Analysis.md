#Wireshark-Traffic-Analysis
##Objective
The objective of this lab was to inspect a packet capture (PCAP) file using Wireshark to investigate anomalous network behaviour. By analyzing packet headers, protocols, and data payloads, I identified malicious traffic signatures, mapped source-to-destination communication paths, and isolated an active security incident.
----
## Scenario Overview
An organization's instrusion detection system(IDS) triggered an alert indicating unusual outbound traffic patterns.
As a security analyst, my task was to analyze the network traffic log to:
1. Filter bulk network traffic to isolate relevant protocols.
2. Inspect the TCP three-way handshake for anomalies.
3. Track down unencrypted payload data to assess potential data exfiltration.
----
## Packet Analysis & Methodology
### 1. Filtering bulk Protocol Traffic
To cut through background network noise (like broadcast traffic and standard DNS queries), I utilized Wireshark display filters to isolate specific protocol types:
'''text
//Display filter to view only HTTP web traffic
http
//Display filter to isolate traffic from a suspected malicious host IP 
ip.addr == 192.168.1.105
Applying these filters allowed me to narrow down thousands of bulk packets into a clean sequence of web requests originating from a single internal asset.
2. Inspecting the TCP Handshake Metrics
I analyzed the transmission control protocol (TCP) flags within the packet headers to ensure connections were establishing normally.
. Normal Sequence Verified: I located the expected sequential packet flows: SYN-ACK-ACK.
. Anomaly Detection: By applying the filter
tcp.flags.syn == 1 and tcp.flags.ack == 0, I adited the tcp.flags.syn == 1 and tcp.flags.ack == 0, I audited the network for high volume of unanswered SYN packets, which would indicate a SYN-flood Denial of service(DoS) attack or an active port scan.
3. Analyzing Payloads for Data Exfiltration
Because HTTP traffic is entirely unencrypted(plaintext), I inspected the packet bytes and packet details panes for specific POST request.
. By right-clicking a packet and selecting Follow TCP Stream, Wireshark reassembled the fragmented packets into readable, sequential transcript of the communication.
. Findings: The stream analysis revealed a plain-text payload containing unauthorized administrative credentials being sent to an external, untrusted server, confirming a successful credential harvesting incident.
Investigation Findngs & Security Impact
Analyzing the PCAP file provided definitive evidence of the security event:
. Root Cause Isolated: An internal workstation was compromised via an unencrypted web form, allowing an attacker to intercept administrative data.
. Incident Containment: By identifying the exact malicious external IP address and port from the packet headers, I provided the firewall team with the necessary parameters to immediately block all outbound traffic to that destination.
. Defensive Hardening: Recommended enforcing strict HTTPS-only transport policies accross all internal corporate web properties to encrypt payloads and mitigate future packet sniffing risks.
