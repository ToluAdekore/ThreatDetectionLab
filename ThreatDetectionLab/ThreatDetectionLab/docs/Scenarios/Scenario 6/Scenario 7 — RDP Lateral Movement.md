# ✅ Scenario 7 — RDP Lateral Movement

**Goal:** Demonstrate that connecting via RDP from a Kali machine to a domain-joined Windows host generates **Windows Security Event ID 4624 (Logon Success, LogonType 10 — Remote Interactive)** and **Sysmon Event ID 3 (Network Connection to port 3389)**, both searchable in Splunk.

---

## Evidence (text-only, no images)
- **Event Viewer (Security):** **Event ID 4624 – Successful Logon** with `LogonType=10 (RemoteInteractive)`.
  - `Account_Name`: Domain user used for RDP.
  - `Computer`: Target host.
  - `IpAddress` / `SourceNetworkAddress`: Kali machine IP.
- **Event Viewer (Sysmon/Operational):** **Event ID 3 – NetworkConnect** for RDP.
  - `Image`: `C:\Windows\System32\svchost.exe` (TermService).
  - `DestinationPort`: `3389`.
  - `SourceIp`: Kali IP, `DestinationIp`: Target host.

- **Relevant Fields:**
  - `Account_Name`, `Computer`, `IpAddress` (source), `DestinationPort=3389`, `Image`, `LogonType=10`.

---

## SPL used in this lab (exact query from your session)

```spl
index=sysmon source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventID 3389
| xmlkv
| search EventID=3
