# 🧪 SOC Lab Scenarios — Detection & Monitoring Validation

This document outlines progressively complex security scenarios designed to validate detection, logging, and monitoring within the homelab. Scenarios escalate from basic visibility checks to advanced adversary simulation, mimicking real-world attacker behavior.

---

## 🖥 Level 1: Basic User Activity

**Objective:** Validate logging of normal user actions using Sysmon and Windows event logs.

---

### ✅ Scenario 1: Successful User Login

- **Action:** Log onto a domain-joined workstation with valid credentials.
- **Expected Result:**
  - Windows **Event ID 4624 - Successful Logon** logged in Splunk.
  - **Relevant Fields:**
    - `Account_Name`
    - `Computer_Name`
    - `Source_IP`
  - **Sysmon Integration:** Sysmon **Event ID 1 - Process Create** for `lsass.exe` activity during authentication (if configured to monitor logon processes).
  - Logs visible in Splunk with user and host details.

---

### ✅ Scenario 2: Common Process Execution

- **Action:** Run `notepad.exe` on the workstation.
- **Expected Result:**
  - **Sysmon Event ID 1 - Process Create** logged.
  - **Relevant Fields:**
    - `Image` (e.g., `C:\Windows\System32\notepad.exe`)
    - `CommandLine`
    - `Computer` (host)
    - `ParentImage` (e.g., `explorer.exe`)
  - Logs visible in Splunk, confirming benign process execution.

---

## 🛠 Level 2: Suspicious User Activity

**Objective:** Simulate suspicious but not overtly malicious activity to validate detection and alerting with Sysmon and Windows logs.

---

### ✅ Scenario 3: PowerShell Execution

- **Action:** Run PowerShell command: `Get-Process`.
- **Expected Result:**
  - **Sysmon Event ID 1 - Process Create** for `powershell.exe` or `pwsh.exe`.
  - **Relevant Fields:**
    - `Image`
    - `CommandLine` (e.g., `powershell.exe -Command Get-Process`)
    - `Computer`
    - `ParentImage`
  - *(Optional)* Windows **PowerShell/Operational Event ID 4104** if Script Block Logging is enabled, capturing the script block.
  - Command line arguments visible in Splunk for analysis.

---

### ✅ Scenario 4: Suspicious File Drop

- **Action:** Place a `.exe` file in `C:\Users\Public`.
- **Expected Result:**
  - **Sysmon Event ID 11 - File Create** detects the file creation.
  - **Relevant Fields:**
    - `TargetFilename` (e.g., `C:\Users\Public\suspicious.exe`)
    - `Image` (process creating the file)
    - `Computer`
  - Potential detection via Splunk file monitoring rules for sensitive paths like `C:\Users\Public`.

---

### ✅ Scenario 5: Outbound Network Connection

- **Action:** Run PowerShell command: `Test-NetConnection example.com -Port 80`.
- **Expected Result:**
  - **Sysmon Event ID 3 - Network Connection** logged for the outbound connection.
  - **Relevant Fields:**
    - `DestinationIp`
    - `DestinationPort` (80)
    - `Image` (e.g., `powershell.exe`)
    - `Computer`
  - Windows Firewall logs may capture the outbound connection.
  - Logs visible in Splunk, showing network activity to `example.com`.

---

## 💥 Level 3: Adversary Simulation

**Objective:** Simulate real-world attacker behavior for advanced detection validation with Sysmon and other logging.

---

### ✅ Scenario 6: Mimikatz Execution

- **Action:** Run Mimikatz (or a decoy binary) on the workstation.
- **Expected Result:**
  - **Sysmon