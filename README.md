# Soc-Sandbox-labs
Welcome to my cybersecurity lab portfolio! This repository contains hands-on documentation, network analysis, and vulnerability assessments conducted in my dedicated home laboratory and through my professional training. 
----
## Lab architecture:
*** Attacker Machine:** Kali Linux (Debian-based penetration testing platform)
*** Target Machine:** Metasploitable 2 (Intentionally vulnerable Linux virtual machine)
*** Network Design:** Host-Only Private Virtual Network (IP Range: '192.168.56.0/24') for secure, isolated testing.
----
## Lab Index
### Practical Sandbox Labs (Hands-on Range)
1.[Network Discovery & Port Scanning (Nmap)](#-lab-1-network-discovery--port-scanning-nmap)
2.* Vulnerability Exploitation % Analysis (Coming Soon!)*
### Google Cybersecurity Certificate Labs (Coursera)
3. [Linux File Permissions Audit](linux-permissions-audit.md)
4. [SQL Querying for Security Incident Investigation](sql-incident-investigation.md)
5. [Wireshark Network Traffic Analysis](wireshark-traffic-analysis.md)
----
## Lab 1: Network Discovery & Port Scanning (Nmap)
### Objective:
Establish a secure, isolated connection between the attacker and target instances, and perform basic host discovery and service identificatio using Nmap.
### Methodology & Troubleshooting:
** Network Isolation:** Originally configured with conflicting viertual adaptors (Bridged and NAT). Reconfigured both VMs to run on a private ***Host-Only Adater** ('VirtualBox Host-Only Ethernet Adapter') to isolate the vulnerable target from the public internet and home LAN.
**Host Verification:** Confirmed IP assignments via CLI ('ifconfig'):
***Kali Linux:** '192.168.56.101' (example)
***Metasploitable 2:** '192.168.56.102'
**Active Scanning:** Conducted a fast port scan targeting the top 100 most common ports to verify open attack surfaces.
### Command Executed
'''bash
nmap -F 192.168.56.102
Scan Results & Analysis 
The scan successfully identified several critical, unpatched services running on the target machine: 
.Port 21(FTP): Open (often vulnerable to anonymous login or version exploits).
.Port 22(SSH): Open (remote terminal management).
.Port 23(Telnet):Open (unencrypted, insecure remote access protocol).
.Port 80(HTTP): Open (web server host).
.Port 3306(MySQL): Open (database server access).

Lab 3: Linux File Permissions Audit (Google Cybersecurity Course 4):
Objective:
Examine file system permisisions via the Linux CLI to ensure that organization security policies are enforced and user access levels resttict unathorized file modifivcations.
Key Skills Demonstrated
.Utilizing commands like ls-la to inspect user, group, and other permissions.
.Utilizing chmod to modify permissions on critical system directories.
.Enhancing organizational directory structure and security baseline configurations.

Lab 4: SQL Querrying for Security Incident Investigation (Google Cybersecurity Course 5):
Objective:
Query transactional and securitty log databases using SQL to track down unathorized login attempts and potential brute force security incidents.
Key Skills Demonstrated
.Constructing basic database queries SELECT, FROM, and WHERE.
.Filtering log entries utilizing wildcard card characters(LIKE) and logical operators(AND,OR).
Generating audit trails of dynamic IP activity matching suspicious user patterns.

Lab 5:Wireshark Network Traffic Analysis(Google Cybersecurity Course 6)
Objective:
Inspect network packet capture (PCAP) files inside wireshark to locate unusual network behaviours, identify rogue protocol interactions, and monitor packet headers.
Key Skills Demonstrated
.Applying Wireshark display filters(e.g., tcp.flags.syn == 1, ip.addr == ...) to sort bulk packet data.
.Inspecting TCP handshake metrics to identify anomalies.
.Analyzing payload logs to pinpoint source-target communication paths.
This lab environment was designed and verified on July 14,2026.
