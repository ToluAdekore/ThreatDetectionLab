# 🧪 SOC Lab Scenarios — Detection & Monitoring Validation

This document outlines progressively complex security scenarios designed to validate detection, logging, and monitoring within a homelab environment. Scenarios escalate from basic visibility checks to advanced adversary simulations, mimicking real-world attacker behavior to test Splunk, Security Onion, and Sysmon integrations.

---

## 🔰 Level 1: Basic Visibility Checks

**Objective:** Confirm log collection and visibility in Splunk and SOC infrastructure using Windows and Sysmon logs.

---

### ✅ Scenario 1: Successful User Login

- **Action:** Log onto a domain-joined workstation with valid credentials.
- **Expected Result:**
  - Windows **Event ID 4624 - Successful Logon** logged in Splunk.
  - **Relevant Fields:**
    - `Account_Name`: User account used for login.
    - `Computer_Name`: Workstation hostname.
    - `Source_IP`: Originating IP address.
  - **Sysmon Integration:** Sysmon **Event ID 1 - Process Create** logs `lsass.exe` activity during authentication (if configured to monitor logon processes).
  - Logs visible in Splunk with user and host details.

---

### ✅ Scenario 2: Common Process Execution

- **Action:** Run `notepad.exe` on the workstation.
- **Expected Result:**
  - **Sysmon Event ID 1 - Process Create** logged.
  - **Relevant Fields:**
    - `Image`: `C:\Windows\System32\notepad.exe`
    - `CommandLine`: Command used to launch Notepad.
    - `Computer`: Hostname of the workstation.
    - `ParentImage`: Typically `explorer.exe`.
  - Logs visible in Splunk, confirming benign process execution.

---

## 🛠 Level 2: Suspicious User Activity

**Objective:** Simulate suspicious but non-malicious activities to validate detection and alerting with Sysmon and Windows logs.

---

### ✅ Scenario 3: PowerShell Execution

- **Action:** Run PowerShell command: `Get-Process`.
- **Expected Result:**
  - **Sysmon Event ID 1 - Process Create** for `powershell.exe` or `pwsh.exe`.
  - **Relevant Fields:**
    - `Image`: Path to PowerShell executable.
    - `CommandLine`: `powershell.exe -Command Get-Process`.
    - `Computer`: Hostname.
    - `ParentImage`: Process launching PowerShell (e.g., `cmd.exe`).
  - *(Optional)* Windows **PowerShell/Operational Event ID 4104** captures script block if Script Block Logging is enabled.
  - Command line arguments visible in Splunk for analysis.

---

### ✅ Scenario 4: Suspicious File Drop

- **Action:** Place a `.exe` file in `C:\Users\Public` (e.g., `suspicious.exe`).
- **Expected Result:**
  - **Sysmon Event ID 11 - File Create** detects the file creation.
  - **Relevant Fields:**
    - `TargetFilename`: `C:\Users\Public\suspicious.exe`.
    - `Image`: Process creating the file (e.g., `explorer.exe` or `powershell.exe`).
    - `Computer`: Hostname.
  - Splunk file monitoring rules may trigger alerts for sensitive paths like `C:\Users\Public`.

---

### ✅ Scenario 5: Outbound Network Connection

- **Action:** Run PowerShell command: `Test-NetConnection example.com -Port 80`.
- **Expected Result:**
  - **Sysmon Event ID 3 - Network Connection** logs the outbound connection.
  - **Relevant Fields:**
    - `DestinationIp`: IP of `example.com`.
    - `DestinationPort`: 80.
    - `Image`: `powershell.exe`.
    - `Computer`: Hostname.
  - Windows Firewall logs capture the outbound connection.
  - Logs visible in Splunk, showing network activity to `example.com`.

---

## 💥 Level 3: Adversary Simulation

**Objective:** Simulate real-world attacker behavior to validate advanced detection with Sysmon, Windows logs, and Security Onion.

---

### ✅ Scenario 6: RDP Lateral Movement

- **Action:** RDP from a Kali machine into a domain-joined host.
- **Expected Result:**
  - Windows **Event ID 4624 - Successful Logon** with Logon Type 10 (Remote Interactive) logged in Splunk.
  - **Relevant Fields:**
    - `Account_Name`: User account used for RDP.
    - `Computer_Name`: Target host.
    - `Source_IP`: Kali machine IP.
  - **Sysmon Event ID 3 - Network Connection** logs RDP-related network activity (e.g., `svchost.exe` or `termsrv.dll`).
  - Logs in Splunk show remote logon from the Kali machine's IP.

---

### ✅ Scenario 7: Reverse Shell

- **Action:**
  - Start a listener on the Kali machine (e.g., `nc -lvnp 4444`).
  - Establish a reverse shell from the Windows host to Kali (e.g., using PowerShell or `nc.exe`).
- **Expected Result:**
  - **Sysmon Event ID 3 - Network Connection** logs the outbound connection to the Kali listener.
  - **Relevant Fields:**
    - `DestinationIp`: Kali machine IP.
    - `DestinationPort`: 4444.
    - `Image`: `powershell.exe` or `nc.exe`.
    - `Computer`: Hostname.
  - Windows Firewall logs capture the outbound connection.
  - Suricata IDS alert triggered for potential C2 activity in Security Onion.
  - Logs visible in Splunk/Security Onion for network and process activity.

---

### ✅ Scenario 8: Privilege Escalation - Admin Group Modification

- **Action:** Add a user to the local Administrators group (e.g., `net localgroup Administrators user /add`).
- **Expected Result:**
  - Windows **Event ID 4732 - Group Membership Change** logged in Splunk.
  - **Relevant Fields:**
    - `Account_Name`: User added to the group.
    - `Group_Name`: Administrators.
    - `Computer_Name`: Hostname.
  - **Sysmon Event ID 1 - Process Create** logs the `net.exe` execution.
  - Detection of privileged group modification in Splunk.

---

### ✅ Scenario 9: Malicious Scheduled Task

- **Action:** Create a scheduled task with a suspicious payload (e.g., `schtasks /create /sc onlogon /tn EvilTask /tr "C:\Users\Public\suspicious.exe"`).
- **Expected Result:**
  - Windows **Event ID 4698 - Scheduled Task Creation** logged.
  - **Relevant Fields:**
    - `TaskName`: `EvilTask`.
    - `TaskContent`: Payload details (e.g., `C:\Users\Public\suspicious.exe`).
    - `Account_Name`: User creating the task.
  - **Sysmon Event ID 1 - Process Create** logs the `schtasks.exe` execution.
  - Associated process execution traceable in `Microsoft-Windows-TaskScheduler/Operational` logs.
  - Logs and potential alerts visible in Splunk for suspicious task creation.

---

## 📝 Notes & Best Practices

- **Usage:** These scenarios are for **controlled, educational lab use only**.
- **Validation:** Confirm detections across:
  - Splunk (Windows and Sysmon logs).
  - Security Onion (Suricata IDS and network logs).
  - Other monitoring tools in your lab.
- **Sysmon Configuration:** Ensure Sysmon rules monitor:
  - Process creation (Event ID 1) for suspicious binaries like `powershell.exe`, `net.exe`, and `schtasks.exe`.
  - Network connections (Event ID 3) for non-standard ports or destinations.
  - File creation (Event ID 11) in sensitive paths like `C:\Users\Public`.
- **Logging Requirements:**
  - Enable PowerShell Script Block Logging for `Event ID 4104`.
  - Configure Windows Firewall to log outbound connections.
- **Documentation:** After each scenario:
  - Capture screenshots of logs and alerts.
  - Record Splunk Search Processing Language (SPL) queries.
  - Save detection notes and artifacts to your GitHub repository for reference.
