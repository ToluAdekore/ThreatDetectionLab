#🧪 Lab Scenarios - SMOC SOC Lab
This document outlines progressively more complex security scenarios designed for detection and monitoring validation within the homelab. Scenarios move from basic to advanced to simulate real-world attacker behavior.

🔰 Level 1 - Basic Visibility Checks
Objective: Validate basic log collection, Splunk visibility, and SOC infrastructure.

✅ Scenario 1: Successful User Login
Action: Log onto a domain-joined workstation with valid credentials

Expected Result:

4624 Successful Logon event in Splunk

Fields: Account_Name, Computer_Name, Source_IP

✅ Scenario 2: Failed Login Attempt
Action: Attempt incorrect password on workstation or DC

Expected Result:

4625 Failed Logon event in Splunk

Includes Account_Name, Failure_Reason, Source_IP

✅ Scenario 3: Common Process Execution
Action: Run notepad.exe or calc.exe on workstation

Expected Result:

Sysmon Event ID 1 (Process Creation)

Fields: Process_Name, Command_Line, Computer_Name

⚒️ Level 2 - Suspicious User Activity
Objective: Simulate suspicious but not overtly malicious activity to validate alerting.

✅ Scenario 4: PowerShell Execution
Action: Run simple PowerShell command:
Get-Process

Expected Result:

Process Creation Event for powershell.exe

Command line arguments visible in Splunk

✅ Scenario 5: Suspicious File Drop
Action: Place .exe file in C:\Users\Public

Expected Result:

File Creation Event detected via Sysmon

Potential detection via Splunk file monitoring

✅ Scenario 6: Outbound Network Connection
Action: Run:
Test-NetConnection example.com -Port 80

Expected Result:

Outbound connection visible in Splunk (Sysmon/Firewall logs)

Suricata may detect traffic leaving monitored network

💥 Level 3 - Adversary Simulation
Objective: Simulate real-world attacker behavior for advanced detection validation.

✅ Scenario 7: Mimikatz Execution
Action: Run Mimikatz (or simulate with decoy binary) on workstation

Expected Result:

Suspicious Process Creation Event

Possible AV or Defender alerts

Alerts in Splunk & Security Onion if detection rules active

✅ Scenario 8: RDP Lateral Movement
Action: RDP into domain-joined host from Kali machine

Expected Result:

Remote 4624 Logon Event in Splunk

Source IP from Kali captured

✅ Scenario 9: Reverse Shell
Action:

Kali listener running

Reverse shell established from Windows host to Kali

Expected Result:

Outbound connection visible in Splunk/Security Onion

Suricata IDS alert triggered for potential C2 channel

✅ Scenario 10: Privilege Escalation - Admin Group Modification
Action: Add user to local Administrators group

Expected Result:

Event ID 4732 in Splunk

Change to privileged group membership detected

✅ Scenario 11: Malicious Scheduled Task
Action: Create scheduled task with suspicious payload

Expected Result:

Task creation logged (Event ID 4698)

Process execution traceable in Splunk/Sysmon

🛡️ Future Scenario Ideas
Phishing & Payload Delivery Simulation

Kerberos Golden Ticket Attack

DNS Tunneling Detection

Data Exfiltration over Non-Standard Ports

Lateral Movement with PsExec or WMI

✅ Notes
All scenarios are for controlled, educational lab use only

Validate detection in Splunk, Security Onion, and other tools after each test

Document screenshots, SPL queries, and detection notes in your GitHub repo
