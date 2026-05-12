# 🛡️ SMB Lateral Movement Detection & PCAP Investigation

## 📌 Project Overview

This project demonstrates the detection and analysis of **SMB-based lateral movement activity** inside an internal network using **PCAP forensic analysis**. The investigation follows a SOC workflow to identify suspicious authentication attempts, internal host communication, and potential credential misuse.

The goal is to simulate how a SOC analyst investigates SMB traffic to detect brute force attempts, credential abuse, and lateral movement behavior.

---

## 🎯 Objectives

- Identify SMB traffic in network captures  
- Detect internal systems involved in SMB communication  
- Analyze NTLM authentication attempts  
- Identify failed login attempts (`STATUS_LOGON_FAILURE`)  
- Extract usernames used in authentication  
- Detect possible lateral movement behavior  

---

## 🧪 Lab Environment

- 🖥️ **Source (Initiator):** `192.168.1.111`  
- 🖥️ **Target System:** `192.168.1.4`  
- 📡 **Protocol:** SMB (TCP Port 445)  
- 🧰 **Tool:** Wireshark  
- 📁 **Data Source:** PCAP file (internal network capture)  

---

## 🔍 Investigation Summary

### 📡 SMB Traffic Detection
SMB protocol identified using TCP port:

```bash
tcp.port == 445

File-sharing communication observed between internal hosts.

🖥️ Source System (SMB Requester)
IP Address: 192.168.1.111
Behavior:
Initiating SMB connections
Connecting to multiple internal systems
Generating authentication attempts
🎯 Target System
IP Address: 192.168.1.4
Behavior:
Receiving SMB login attempts
Acting as file-sharing endpoint
❌ Authentication Failures

SMB authentication responses showed:

STATUS_LOGON_FAILURE (0xC000006D)

👉 Meaning:

Invalid credentials used
Possible brute force or credential misuse
👤 Username Observed

During NTLM authentication analysis:

Username: kali

Extracted from:

NTLMSSP_AUTH message
SMB2 Session Setup Request
🧠 Key Findings
Multiple SMB authentication attempts detected
NTLM authentication used between internal hosts
Repeated login failures observed
One system initiating SMB connections to another host
Suspicious credential usage pattern identified
🚨 Security Analysis

This behavior may indicate:

🔴 SMB brute force attempts
🔴 Password spraying
🔴 Lateral movement using stolen credentials
🔴 Penetration testing activity (Kali-based simulation)
🧩 MITRE ATT&CK Mapping
Technique	ID	Description
Lateral Movement via SMB	T1021.002	Remote services over SMB
Valid Accounts	T1078	Credential-based authentication
Brute Force	T1110	Repeated login attempts
🛠️ Tools Used
Wireshark (PCAP Analysis)
SMB2 Protocol Inspection
NTLM Authentication Analysis
TCP Stream Analysis
📊 Wireshark Filters Used
tcp.port == 445
smb || smb2
ntlmssp
smb2.nt_status == 0xc000006d
📁 Project Structure
SMB-Lateral-Movement-Project/
│
├── PCAP/
│   └── smb_traffic.pcap
│
├── Screenshots/
│   ├── smb_filter.png
│   ├── ntlm_auth.png
│   ├── conversations.png
│
├── Reports/
│   └── incident_report.pdf
│
├── README.md
📌 Outcome

This investigation successfully identified suspicious SMB authentication behavior, including failed login attempts and NTLM-based authentication between internal hosts. The activity is consistent with potential lateral movement or credential probing within a network.

🚀 Future Improvements
Add Splunk dashboard for SMB detection
Automate detection rules for STATUS_LOGON_FAILURE spikes
Integrate Wazuh endpoint logs
Simulate real-world SMB lateral movement attack
