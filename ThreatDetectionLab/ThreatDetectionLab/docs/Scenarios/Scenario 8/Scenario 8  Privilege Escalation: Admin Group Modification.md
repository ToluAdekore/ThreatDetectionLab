# ✅ Scenario 8 — Privilege Escalation: Admin Group Modification

Goal: Demonstrate that adding a user to the local Administrators group generates **Windows Security Event ID 4732 (Group Membership Change)** and **Sysmon Event ID 1 (Process Create)** for the execution of `net.exe`.

---

## Evidence (text-only, no images)
- **Event Viewer (Security Log):** Windows **Event ID 4732** shows a user added to the *Administrators* group.  
- **Event Viewer (Sysmon/Operational):** Sysmon **Event ID 1 – Process Create** for `net.exe` in *Microsoft-Windows-Sysmon/Operational*, with `CommandLine` showing `net localgroup Administrators <username> /add`.  
- **Host state:** A user was elevated to the local Administrators group via the `net localgroup` command.  
- **Splunk:** Search returns both the Sysmon ProcessCreate event (execution of `net.exe`) and the Security 4732 group modification event with key fields (Account_Name, Group_Name, Computer).  
- **Relevant Fields:**  
  - `Image`: Path to net.exe (e.g., `C:\Windows\System32\net.exe`).  
  - `CommandLine`: `net localgroup Administrators <username> /add`.  
  - `Computer`: Hostname where modification occurred.  
  - `ParentImage`: Process launching net.exe (e.g., `cmd.exe`).  
  - `Account_Name`: User added to the group.  
  - `Group_Name`: Administrators.  

---

## SPL used in this lab (exact query from your session)
```spl
index=sysmon source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=1 CommandLine="*net.exe*"
| xmlkv

index=wineventlog_security source="WinEventLog:Security" EventCode=4732
| xmlkv
| search EventCode=4732
