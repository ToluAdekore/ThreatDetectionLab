# 🧪 SOC Lab Scenarios — Detection & Monitoring Validation

This document outlines progressively complex security scenarios designed to validate detection, logging, and monitoring within the homelab. Scenarios escalate from basic visibility checks to advanced adversary simulation, mimicking real-world attacker behavior.

---

## 🔰 Level 1: Basic Visibility Checks

**Objective:** Validate basic log collection, Splunk visibility, and SOC infrastructure.

### ✅ Scenario 1: Successful User Login
- **Action:** Log onto a domain-joined workstation with valid credentials.
- **Expected Result:**  
  - Event ID `4624 - Successful Logon` appears in Splunk.
  - **Relevant Fields:** `Account_Name`, `Computer_Name`, `Source_IP`.

## ✅ Scenario 3: Common Process Execution

**Action:** Run `notepad.exe` on the workstation.  
**Expected Result:**  
- **Sysmon Event ID 1 – Process Create** logged.  
- **Relevant Fields:** `Image`, `CommandLine`, `Computer` (host), `ParentImage`.

---

## 🛠 Level 2: Suspicious User Activity

**Objective:** Simulate suspicious but not overtly malicious activity to validate detection and alerting.

### ✅ Scenario 4: PowerShell Execution
**Action:** Run PowerShell command: `Get-Process`.  
**Expected Result:**  
- **Sysmon Event ID 1 – Process Create** for `powershell.exe` (or `pwsh.exe`).  
- *(Optional)* Windows **PowerShell/Operational Event ID 4104** if Script Block Logging is enabled.  
- Command line arguments visible in Splunk.

### ✅ Scenario 5: Suspicious File Drop
**Action:** Place a `.exe` file in `C:\Users\Public`.  
**Expected Result:**  
- **Sysmon Event ID 11 – File Create** detects the drop.  
- Potential detection via Splunk file monitoring of that path.

### ✅ Scenario 6: Outbound Network Connection
**Action:** Run:  
```powershell
Test-NetConnection example.com -Port 80


---

## 💥 Level 3: Adversary Simulation

**Objective:** Simulate real-world attacker behavior for advanced detection validation.

✅ Scenario 7: Mimikatz Execution
- **Action:** Run Mimikatz (or decoy binary) on workstation.
- **Expected Result:**
  - Security Event ID **4688** – Suspicious process creation captured.
  - Possible AV/Defender alerts triggered.
  - Alerts in Splunk and Security Onion (if detection rules active).

✅ Scenario 8: RDP Lateral Movement
- **Action:** RDP from Kali machine into domain-joined host.
- **Expected Result:**
  - Event ID **4624** – Remote Logon (Logon Type 10) visible in Splunk.
  - Source IP from Kali machine captured.

✅ Scenario 9: Reverse Shell
- **Action:**
  - Start Kali listener.
  - Establish reverse shell from Windows host to Kali.
- **Expected Result:**
  - Outbound connection logged in Windows Firewall logs and visible in Splunk/Security Onion.
  - Suricata IDS alert triggered for potential C2 activity.

✅ Scenario 10: Privilege Escalation - Admin Group Modification
- **Action:** Add a user to the local Administrators group.
- **Expected Result:**
  - Event ID **4732** – Group Membership Change captured in Splunk.
  - Detection of privileged group modification.

✅ Scenario 11: Malicious Scheduled Task
- **Action:** Create a scheduled task with a suspicious payload.
- **Expected Result:**
  - Event ID **4698** – Scheduled Task Creation logged.
  - Associated process execution traceable in `Microsoft-Windows-TaskScheduler/Operational` logs.

---

## 📝 Notes & Best Practices

- These scenarios are for **controlled, educational lab use only**.
- Validate detections across:
  - Splunk
  - Security Onion
  - Other monitoring tools in your lab.
- After each scenario:
  - Document screenshots.
  - Record SPL queries.
  - Save detection notes to your GitHub repo.

---
