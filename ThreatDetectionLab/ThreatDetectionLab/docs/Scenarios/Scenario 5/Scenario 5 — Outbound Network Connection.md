# ✅ Scenario 5 — Outbound Network Connection

**Goal:** Demonstrate that initiating an outbound connection with PowerShell (e.g., `Test-NetConnection`) generates **Sysmon Event ID 3 (Network Connect)** and is searchable in Splunk with useful context.

---

## Evidence (text-only, no images)
- **Event Viewer:** Sysmon **Event ID 3 – NetworkConnect** in *Microsoft-Windows-Sysmon/Operational*, showing a PowerShell-initiated outbound connection with details of source/destination IP and port.
- **Host state:** PowerShell executed a network connectivity test (`Test-NetConnection`) resulting in an outbound TCP connection attempt on port **445**.
- **Splunk:** Search returns Sysmon NetworkConnect events, showing PowerShell as the initiating process, along with IP/port details of the attempted connection.
- **Relevant Fields:**
  - `Image`: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
  - `ProcessGuid`: `{6221ade6-2a16-68a1-cb11-00000003800}`
  - `User`: `TOLU\James`
  - `SourceIp`: `192.168.2.67`
  - `DestinationIp`: `192.168.2.10`
  - `DestinationPort`: `445`
  - `Computer`: `TOLUADEKORE2`

---

## SPL used in this lab (exact query from your session)

```spl
index=sysmon source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=3
| xmlkv
| search EventID=3
