# Soc-Sandbox-labs
Welcome to my practical cybersecurity home lab repository. This portfolio documents hands-on security exercises, network analysis, vulnerability assessments, SQL investigations, and defensive controls within an isolated virtual lab environment.
----
## Home Lab Environment & Architecture:
*** Hypervisor:**VMware Workstation
***Attacker/Analysis Workstation:**Kali Linux (Debian-based penetration testing platform)
*** Target Machine:** Metasploitable 2 (Intentionally vulnerable Linux virtual machine)
*** Network Design:** Host-Only Private Virtual Network (IP Range: '192.168.56.0/24') for secure, isolated testing.
----
##Core Tools & Technologies
| Category | Tools & Technologies |
| :--- | :--- |
| **Network Discovery & Scanning** | Nmap, Net cast |
| **Protocol Analysis & Forensics** | Wireshark, Tshark |
| **Operating Systems & Administration** | Linux (Kali, Ubuntu), Metasploitable, Bash, File Permissions |
| **Database Security & Analysis** | SQL, Relational Database Querying |
| **Protocols Inspected** | FTP, TCP/IP, ARP, DHCP, DNS |

## Lab Index & Case Studies
### Practical Sandbox Labs (Hands-on Range)
1.[Network Discovery & Port Scanning (Nmap)](#-lab-1-network-discovery--port-scanning-nmap)
2. [Vulnerability Analysis & Service Hardening](Vulnerability-analysis.md)
**Focus: ** Access control auditing, default credential risks ('msfadmin'), and service configuration hardening.
3.[Network Traffic Analysis: PCAP Inspection with Wireshark](wireshark-pcap-analysis.md)
 ** Focus: ** Offline packet forensics, cleartext credential extraction, and brute-force signature analysis ('530' vs '230' response codes).
### Google Cybersecurity Certificate Labs (Coursera)
4. [Linux File Permissions Audit](linux-permissions-audit.md)
 ** Focus: ** System security, directory access control, and auditing file permissions on Linux assets. 
5. [SQL Querying for Security Incident Investigation](sql-incident-investigation.md)
** Focus: ** Database querying, log inspection, and tracking security incidents using SQL commands.
6. [Wireshark Network Traffic Analysis](wireshark-traffic-analysis.md)
** Focus: ** Live packet capturing, protocol analysis, and inspecting network traffic streams.
----
## Lab 1: Network Discovery & Port Scanning (Nmap)
### Objective:
Establish a secure, isolated connection between the attacker and target instances, and perform basic host discovery and service identification using Nmap.
### Methodology & Troubleshooting:
** Network Isolation:** Originally configured with conflicting virtual adaptors (Bridged and NAT). Reconfigured both VMs to run on a private ***Host-Only Adapter** ('VirtualBox Host-Only Ethernet Adapter') to isolate the vulnerable target from the public internet and home LAN.
**Host Verification:** Confirmed IP assignments via CLI ('ifconfig'):
***Kali Linux:** '192.168.56.101' (example)
***Metasploitable 2:** '192.168.56.102'
**Active Scanning:** Conducted a fast port scan targeting the top 100 most common ports to verify open attack surfaces.
### Command Executed
'''bash
Nmap -F 192.168.56.102
Scan Results & Analysis 
The scan successfully identified several critical, unpatched services running on the target machine: 
.Port 21(FTP): Open (often vulnerable to anonymous login or version exploits).
.Port 22(SSH): Open (remote terminal management).
.Port 23(Telnet):Open (unencrypted, insecure remote access protocol).
.Port 80(HTTP): Open (web server host).
.Port 3306(MySQL): Open (database server access).

Lab 3: Linux File Permissions Audit (Google Cybersecurity Course 4):
Objective:
Examine file system permissions' via the Linux CLI to ensure that organization security policies are enforced and user access levels restrict unauthorized file modification.
Key Skills Demonstrated
.Utilizing commands like ls-la to inspect user, group, and other permissions.
.Utilizing chmod to modify permissions on critical system directories.
.Enhancing organizational directory structure and security baseline configurations.

Lab 4: SQL Querying for Security Incident Investigation (Google Cybersecurity Course 5):
Objective:
Query transactional and security log databases using SQL to track down unauthorized login attempts and potential brute force security incidents.
Key Skills Demonstrated
.Constructing basic database queries SELECT, FROM, and WHERE.
.Filtering log entries utilizing wildcard card characters(LIKE) and logical operators(AND,OR).
Generating audit trails of dynamic IP activity matching suspicious user patterns.

Lab 5:Wireshark Network Traffic Analysis(Google Cybersecurity Course 6)
Objective:
Inspect network packet capture (PCAP) files inside Wireshark to locate unusual network behaviors, identify rogue protocol interactions, and monitor packet headers.
Key Skills Demonstrated
.Applying Wireshark display filters(e.g., tcp.flags.syn == 1, ip.addr == ...) to sort bulk packet data.
.Inspecting TCP handshake metrics to identify anomalies.
.Analyzing payload logs to pinpoint source-target communication paths.
This lab environment was designed and verified on July 14,2026.
----
## Career Focus & Objective
Aspiring **Junior SOC Analyst / Security Engineer ** actively completing hands-on certifications and cloud architecture coursework (Google Cybersecurity, CompTIA Security+, Microsoft Azure). Documenting real-world threat detection scenarios to build defensive operations readiness.
