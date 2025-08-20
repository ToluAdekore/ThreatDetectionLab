# ✅ Scenario 9 — Registry Persistence: Run Key Modification

**Goal:** Demonstrate that creating a registry value under the `Run` key for persistence generates **Sysmon Event ID 13 (Registry Value Set)** and is searchable in Splunk with useful context.

---

## Evidence (text-only, no images)
- **Event Viewer (Sysmon/Operational):** Sysmon **Event ID 13 – Registry Value Set** logged when a new value (`PersistenceTest`) was added under the `Run` key.  
- **Host state:** Registry modified to launch `notepad.exe` at logon for the current user.  
- **Splunk:** Search returns the Sysmon registry event showing the modification.  
- **Relevant Fields:**  
  - `EventID`: 13 (Registry Value Set).  
  - `TargetObject`: `HKCU\Software\Microsoft\Windows\CurrentVersion\Run\PersistenceTest`.  
  - `Details`: `C:\Windows\System32\notepad.exe`.  
  - `Image`: `C:\Windows\System32\reg.exe`.  
  - `Computer`: Hostname (ToluAdekore2.Tolu.local).  
  - `User`: Account modifying the registry.  

---

## SPL used in this lab (exact query from your session)

```spl
index=sysmon source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=13 
| xmlkv 
| search EventID=13 TargetObject="HKU\\S-1-5-21-506213592-2373238444-3720423507-1109\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run\\PersistenceTest"
