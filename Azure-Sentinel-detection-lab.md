Azure sentinel Cloud SOC Lab: Brute-Force Detection

🎯 Objective

To build a cloud-based Security Operations Center (SOC) using Microsoft Azure Sentinel, configure log ingestion from a Windows VM, and create a detection rule to identify brute-force login attempts — simulating a real-world SOC analyst workflow.

🛠️ Tools & Technologies Used

Tool Purpose
Microsoft Azure Cloud infrastructure platform
Azure Virtual Machines Windows Server 2025 VM (SOC-Windows-VM)
Azure Log Analytics Workspace Centralized log storage and querying
Azure Sentinel Cloud-native SIEM / Security Orchestration, Automation, and Response (SOAR)
Azure Monitor Agent (AMA) Log collection agent for Windows VMs
Kusto Query Language (KQL) Query language for log analysis
Remote Desktop Protocol (RDP) Remote access to Windows VM
Kali Linux Attack simulation platform

---

🏗️ Architecture Overview

```
Attacker (Kali Linux)
        │
        │ Hydra Brute-Force
        ▼
+----------------------------+
|     Windows Server VM      |
|   (SOC-Windows-VM)         |
|   - Event ID 4625 (failed) |
|   - Event ID 4624 (success)|
+----------------------------+
        │
        │ Azure Monitor Agent (AMA)
        │ Data Collection Rule (DCR)
        ▼
+----------------------------+
|  Log Analytics Workspace   |
|  (SOC Analytics Workspace)   |
|  - Security Event table     |
+----------------------------+
        │
        │ KQL Query (Scheduled)
        ▼
+----------------------------+
|     Azure Sentinel         |
|   - Detection Rule         |
|   - Incident Generation    |
|   - Alert Dashboard        |
+----------------------------+
        │
        ▼
   SOC Analyst Investigation
```

---

📊 Lab Setup Summary

Component Configuration
Virtual Machine Windows Server 2025 Datacenter, Standard_B2as_v2, East US
VM Credentials Username: socadmin
Log Analytics Workspace SOCAnalyticsWorkspace
Sentinel Status Enabled
Data Connector Windows Security Events via AMA
Data Collection Rule DCR-SOC-Windows-VM (collects all security events)
Network Security Group Inbound RDP (3389) allowed for lab testing

---

🔍 Detection Rule: Brute-Force Login Attempts

Rule Configuration

Setting Value
Rule Name Detect Brute-Force Login Attempts
Rule Type Scheduled Query Rule
Severity Medium
Tactics Brute Force, Credential Access
Query Interval 5 minutes
Incident Creation Enabled

KQL Query Used

```kql
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts = count() by Account, Computer, bin(TimeGenerated, 5m)
| where FailedAttempts >= 5
```

What This Query Does

Component Explanation
SecurityEvent Queries the Windows security event logs
where EventID == 4625 Filters for failed login attempts
summarize FailedAttempts = count() Counts failures per account, per computer, per 5-minute window
where FailedAttempts >= 5 Triggers only if 5+ failures occur in 5 minutes

---

🧪 Attack Simulation

Step 1: Generate Failed Logins

From the Windows VM, locked the screen (Windows + L) and entered incorrect passwords 10-15 times, followed by one successful login.

Step 2: Incident Triggered

Within 5-10 minutes of the failed login attempts, Azure Sentinel generated an incident.

Incident Details

Field Value
Incident ID c383f296-a82c-4ea9-b1d0-c95a23f43dcc
Alert Name Multiple Failed Logins Detected
Description Identifies accounts with multiple failed logins followed by a success
Attack Tactic Credential Access (MITRE ATT&CK)
Affected Account socadmin
Source SOC-Windows-VM

---

🧰 Key Skills Demonstrated

Skill How It Was Demonstrated
Cloud Infrastructure Deployed Azure VM, configured networking, managed resource groups
SIEM Configuration Enabled Azure Sentinel, connected data sources
Log Ingestion Installed Azure Monitor Agent, created Data Collection Rules
KQL Query Writing Wrote detection logic to identify brute-force patterns
Incident Response Investigated generated incidents, documented findings
Attack Simulation Used RDP lock screen to simulate brute-force attempts
Troubleshooting Debugged agent connectivity and Data Collection Rules

---

📈 Business Impact & Learnings

What I Learned

· How to build a cloud-native SOC from scratch
· How to write KQL queries for security detection
· How data collection rules (DCRs) work in Azure Monitor
· How to simulate attacks and detect them in real-time
· The importance of logging and monitoring in incident detection

Real-World Application

This lab simulates a common SOC analyst workflow: setting up monitoring, writing detection rules, investigating incidents, and documenting findings — exactly what Tier 1 SOC analysts do daily.

---

🔗 Lab Resources

Resource Link
Lab Environment Microsoft Azure (Free Trial)
SIEM Platform Microsoft Sentinel
Attack Simulation Manual RDP lock screen brute-force
Portfolio Repo GitHub - SOC-Sandbox-Labs

---

📸 Screenshots

Note: Screenshots are stored in the screenshots/ folder of this repository.

Screenshot Description
sentinel-connector.png Windows Security Events via AMA connector status
sentinel-rule.png Active detection rule in Sentinel Analytics
sentinel-incident.png Incident generated by brute-force detection
sentinel-query.png KQL query results showing failed logins
azure-vm.png SOC-Windows-VM running in Azure
log-analytics.png Log Analytics workspace with ingested data
