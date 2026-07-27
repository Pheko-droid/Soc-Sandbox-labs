markdown
# Network Traffic Analysis: PCAP Inspection with Wireshark
## Objective
The goal of this investigation was to perform offline packet capture (PCAP) analysis on captured network traffic ('ftp_bruteforce.pcapng') to identify authentication anomalies, inspect unencrypted protocol commaands, and isolate potential brute-force activity.
----
## Scenario & Investigation Overview
As a SOC Analyst, reviewing packet captures is critical for verifying alerts and determining whether authentication failures represent routine user error or an automated brute-force attempt.
Durin this investiation, I focused on:
1. Inspecting cleartext credentials transmitted via FTP control protocol.
2. Filtering for repeated authentication failure status code.
3. Isolating the specific packet frame where successful authentication occured.
----
## Key Wireshark Display Filters Applied
Wireshark Display Filter | Purpose & Analysis Outcome |
:--- | :---|
'ftp.request.command == "PASS"' | Extracted cleartext password attempts submitted by client host ('PASS password123', 'PASS 12345', 'PASS msfadmin', etc.). |
'ftp.response.code == 530' | Filtered for server responses indicating **Login incorrect**, identifying repeated failed authentication attempts in rapid succession. |
'ftp.response.code == 230' | Isolated the specific response frame indicating ** Login successful**, confirming the exact moment access was granted. |
----
Key Findings
1. **Cleartext Credential Exposure:** FTP transmits commands and passwords in cleartext. Inspecting the payload revealed all login attempts without needing decryption.
2. **Brute-Force Pattern Identified:** Multiple rapid sequencial password attempts resulted in '530' status codes, indicating automated dictionary/brute-force activity.
3. **Successful Account Compromise:** A subsequent '230' status resoponse confirmed a sucessful login using valid credentials ('msfadmin').
----
## Defensive Reccommendations & Hardening
*** Enforce Transport Layer Encryption:** Disable cleartext FTP and enforce encrypted file protocols (**SFTP** or **FTPS**).
*** Account Lockout Policies:** Implement rate limiting and temporary account lockout thresholds after consecutive failed login attempts (e.g., 5failed attempts within 5minutes).
*** SIEM Alet Integration:** Configure network intrusion detection systems (NIDS) to trigger alerts when multiple FTP status code '530' events originate from a single source IP.
