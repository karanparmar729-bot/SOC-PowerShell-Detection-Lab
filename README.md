# Endpoint Detection & Log Pipeline Analysis (Sysmon -> Splunk)

## 📌 Project Overview
This project demonstrates the end-to-end implementation of an enterprise-grade log ingestion pipeline and the subsequent security analysis of a simulated endpoint attack. 

By configuring **Microsoft Sysmon** on a target Windows 10 VM, forwarding logs to a centralized **Splunk Enterprise** SIEM on my host Windows 11 machine, and executing a simulated PowerShell execution policy bypass technique, I successfully captured, analyzed, and documented critical forensic artifacts.

---

## 📂 Access the Full Project Report
📄 **[Download / View the Complete Project Report](./Ranjit_Singh_SOC_Project.doc)** 

---

## 🛠️ Technologies & Tools Used
* **SIEM:** Splunk Enterprise (Indexer/Search Head)
* **Endpoint Telemetry:** Microsoft Sysmon (SwiftOnSecurity Configuration)
* **Log Transport:** Splunk Universal Forwarder (Port 9997 TCP)
* **Hypervisor:** Oracle VM VirtualBox
* **Target OS:** Windows 10 Pro (VM)
* **Host/Collector OS:** Windows 11 Pro (Physical Host)
* **Simulated Threat:** PowerShell Execution Policy Bypass (MITRE ATT&CK T1059.001)

---

## 📐 Lab Architecture & Log Flow
```text
+------------------------------------------+        Port 9997 (TCP)       +-----------------------------------------+
|             WINDOWS 10 VM                |                              |            WINDOWS 11 HOST              |
|        (Target Host / Forwarder)         |  ------------------------->  |          (SIEM Host / Collector)        |
|                                          |                              |                                         |
|  [Sysmon Operational Event Logs]         |                              |  [Splunk Enterprise Instance]           |
|  [Splunk Universal Forwarder Service]    |                              |  [Centralized Indexer & Search Head]    |
+------------------------------------------+                              +-----------------------------------------+

⚡ The Simulation (PowerShell Evasion)
To simulate a defense evasion technique commonly used in phishing campaigns, I ran an elevated command on the Windows 10 VM designed to bypass execution policies (-ep bypass) and silently download an external payload (google_test.txt) to a public directory:
powershell.exe -nop -ep bypass -c "Invoke-WebRequest -Uri 'https://www.google.com' -OutFile 'C:\Users\Public\google_test.txt'"

🔍 SOC Analyst Investigation in Splunk
As a SOC Analyst, I built a high-fidelity search query in Splunk to hunt for the download artifact:
index=* "google_test.txt"
Key Forensic Artifacts Extracted:
Target Endpoint Name: DESKTOP-S7EO8FC
Source Log Pathway: WinEventLog:Microsoft-Windows-Sysmon/Operational (Sysmon Event ID 1: Process Creation)
Initiating Executable: PowerShell.EXE
Evasion Parameter Detected: -ep bypass (Execution Policy Bypass)
Parent Process: cmd.exe (Command Prompt)
🛡️ Incident Response Playbook (Highlights)
If detected in production, the L1 SOC Analyst workflow includes:

Network Isolation: Isolate host DESKTOP-S7EO8FC immediately to prevent lateral movement.
Process Termination: Kill the specific active process IDs (PIDs) associated with the malicious string.
Eradication: Locate, analyze in a sandbox, and safely delete the downloaded google_test.txt file.
Host Hardening: Enforce AppLocker/WDAC policy controls to block non-admin execution of PowerShell.
🎓 Key Takeaways & Skills Proven
SIEM Engineering: Successfully configured and validated a functional remote log ingestion pipeline.
Forensic Auditing: Demonstrated advanced understanding of Sysmon Event ID 1 (Process Creation) and forensic telemetry.
Threat Mitigation: Capable of translating raw alerts into structured containment and eradication steps.

---
### 👤 Connect with Me
* **Author:** Ranjit Singh
* **LinkedIn:** [Ranjit Singh on LinkedIn](https://www.linkedin.com/in/ranjit-singh-a878a93b9)
