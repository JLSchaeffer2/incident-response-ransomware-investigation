# Incident Response Investigation: Web Compromise, Lateral Movement & Ransomware

## Overview
This project documents a full-scale incident response investigation involving a web application compromise, credential abuse, malware execution, and ransomware deployment across multiple systems.

The investigation began with a high-severity alert indicating suspicious web activity and expanded to include lateral movement and widespread file encryption consistent with Cerber ransomware behavior.

---

## Objective
To analyze and reconstruct the full attack chain, identify indicators of compromise, determine scope and impact, and provide actionable remediation and detection improvements.

---

## Investigation Components

### 1. Triage & Initial Analysis
- Reviewed high-severity alert indicating abnormal web traffic
- Identified potential web-based attack targeting a Joomla application
- Determined alert disposition as **True Positive**
- Established initial scope of compromise

---

### 2. Attack Chain Analysis
The investigation identified a complete multi-stage attack:

- Automated vulnerability scanning (Acunetix)
- Brute-force authentication attack
- Successful administrative login
- Malware delivery and execution (3791.exe)
- VBScript-based payload retrieval
- Steganography-based ransomware delivery (mhtr.jpg)
- Lateral movement via SMB
- File encryption across shared systems

---

### 3. Systems Impacted
- Web Server: 192.168.250.70 (Joomla application)
- Workstation: 192.168.250.100 (user endpoint)
- File Server: 192.168.250.20 (shared storage)

---

## Key Findings

### Initial Access
- Target: Joomla CMS login interface
- Method: Brute-force password attack
- Successful password identified: `batman`
- Over **400+ unique password attempts** observed

### Malware Execution
- Malicious executable: `3791.exe`
- Delivered post-authentication
- Executed on compromised web server

### Command & Control / Payload Delivery
- Malicious domains identified:
  - prankglassinebracket.jumpingcrab.com
  - solidaritedeproximite.org
- Ransomware domain:
  - cerberhhyed5frqa.xmfir0.win

### Obfuscation Technique
- Payload disguised as image file (`mhtr.jpg`)
- Confirmed use of **steganography** for malware delivery

### Lateral Movement
- Protocol: SMB
- Source: Compromised workstation
- Target: File server (192.168.250.20)

### Impact
- 257 PDF files encrypted
- 401 text files encrypted
- Multi-system compromise with operational disruption

---

## MITRE ATT&CK Techniques Observed

- T1190 – Exploit Public-Facing Application  
- T1110.001 – Brute Force: Password Guessing  
- T1059.005 – Command and Scripting Interpreter (VBScript)  
- T1105 – Ingress Tool Transfer  
- T1027 – Obfuscated Files or Information  
- T1486 – Data Encrypted for Impact  
- T1021.002 – Remote Services (SMB)  

---

## Indicators of Compromise (IOCs)

### IP Addresses
- 40.80.148.42 (scanning activity)
- 23.22.63.114 (exploitation and brute-force)

### Domains
- prankglassinebracket.jumpingcrab.com
- solidaritedeproximite.org
- cerberhhyed5frqa.xmfir0.win

### Files
- 3791.exe
- mhtr.jpg
- 121214.tmp

### Hashes
- MD5: AAE3F5A29935E6ABCC2C2754D12A9AF0  
- SHA256: 9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8  

---

## Investigation Methodology

### Log Analysis
- Analyzed HTTP, DNS, SMB, and Sysmon logs
- Correlated events across multiple data sources
- Reconstructed timeline of attacker activity

### SIEM Queries
- Extracted credentials from HTTP POST data
- Identified brute-force patterns and password reuse
- Tracked malicious file execution and process chains
- Investigated DNS queries for command-and-control activity

### OSINT & Threat Intelligence
- Pivoted on IPs and hashes using VirusTotal
- Identified attacker infrastructure and malware samples
- Decoded embedded hex strings using CyberChef

---

## Detection & Response Recommendations

### Incident Response Actions
- Isolate compromised systems immediately
- Reset all affected credentials
- Perform forensic imaging and analysis
- Escalate to incident response leadership

### Prevention Improvements
- Enforce strong password policies and MFA
- Implement account lockout mechanisms
- Patch and harden web applications
- Restrict script execution from user directories
- Harden SMB access controls

### Detection Enhancements
- Detect repeated failed login attempts followed by success
- Monitor for VBScript execution from AppData
- Correlate DNS activity with malware downloads
- Improve ransomware detection visibility

---

## Skills Demonstrated
- Incident Response & Triage  
- SIEM Log Analysis (Splunk)  
- Threat Hunting & Correlation  
- Malware Analysis (Behavioral)  
- OSINT & Threat Intelligence  
- Attack Chain Reconstruction  
- MITRE ATT&CK Mapping  

---

## Supporting Documentation

This investigation is supported by:

- Triage report (initial findings and impact assessment)
- Detailed investigation notes (queries, analysis, and evidence)

## Project Files

- [IR Triage Report](./IR%20Triage%20Report%20%7BJerome%20Schaeffer%20II%7D.docx)
- [Investigation Notes](./Investigation%20notes%20%7BJerome%20Schaeffer%20II%7D.docx)

---

## Notes
All analysis was conducted in a controlled lab environment. No real-world systems were impacted.
