# 🧪 Scenario 3: PowerShell Execution

---

## 📋 Overview

**Objective:** Simulate suspicious PowerShell activity by executing the `ipconfig` command to validate detection and logging using Sysmon and Windows event logs in a homelab environment.

---

## ✅ Scenario Details

- **Action:** Run PowerShell command: `ipconfig`.
- **Expected Result:**
  - **Sysmon Event ID 1 - Process Create** for `powershell.exe` or `pwsh.exe`.
  - **Relevant Fields:**
    - `Image`: Path to PowerShell executable (e.g., `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`).
    - `CommandLine`: `powershell.exe -Command ipconfig`.
    - `Computer`: Hostname of the workstation.
    - `ParentImage`: Process launching PowerShell (e.g., `cmd.exe`).
  - *(Optional)* Windows **PowerShell/Operational Event ID 4104** captures script block if Script Block Logging is enabled, logging the `ipconfig` command.
  - Command line arguments visible in Splunk for analysis.

---

## 📝 Notes & Best Practices

- **Sysmon Configuration:** Ensure Sysmon is configured to log process creation (Event ID 1) for `powershell.exe` and `pwsh.exe` with detailed command-line arguments.
- **Logging Requirements:** Enable PowerShell Script Block Logging to capture Event ID 4104 for detailed visibility of the `ipconfig` command. Configure via:
  - **Group Policy:** Computer Configuration > Administrative Templates > Windows Components > Windows PowerShell > Enable Turn on PowerShell Script Block Logging.
  - **Registry:** Set `HKLM\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging\EnableScriptBlockLogging` to `1`.
- **Validation:** Confirm logs in Splunk or Security Onion, focusing on the `CommandLine` field in Sysmon Event ID 1 and `ScriptBlockText` field in Event ID 4104 (if enabled).
- **Splunk Query Example:**
  ```
  index=* sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 CommandLine=*ipconfig*
  ```
  or
  ```
  index=* sourcetype="WinEventLog:Microsoft-Windows-PowerShell/Operational" EventCode=4104 ipconfig
  ```
- **Documentation:**
  - Capture screenshots of Splunk logs showing the event.
  - Record Splunk Search Processing Language (SPL) queries used to detect the activity.
  - Save notes and any related `ipconfig` output files to your GitHub repository in the `Scenario_3` folder.