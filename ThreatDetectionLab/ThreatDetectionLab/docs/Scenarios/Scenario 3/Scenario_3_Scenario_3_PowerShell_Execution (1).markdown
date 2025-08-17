# ✅ Scenario 3 — PowerShell Execution

**Goal:** Demonstrate that executing the `ipconfig` command via PowerShell generates **Sysmon Event ID 1 (Process Create)** and is searchable in Splunk with useful context, optionally including **Windows PowerShell/Operational Event ID 4104** if Script Block Logging is enabled.

---

## Evidence (text-only, no images)
- **Event Viewer:** Sysmon **Event ID 1 – Process Create** for `powershell.exe` or `pwsh.exe` in *Microsoft-Windows-Sysmon/Operational*, with `CommandLine` showing `powershell.exe -Command ipconfig`.
- **Host state:** PowerShell was launched to execute `ipconfig`, displaying network configuration, and then closed (simple execution and termination).
- **Splunk:** Search returns the PowerShell ProcessCreate event with key fields (Image, CommandLine, ParentImage, User), and optionally **Event ID 4104** with `ScriptBlockText` containing `ipconfig` if Script Block Logging is enabled.
- **Relevant Fields:**
  - `Image`: Path to PowerShell executable (e.g., `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`).
  - `CommandLine`: `powershell.exe -Command ipconfig`.
  - `Computer`: Hostname of the workstation.
  - `ParentImage`: Process launching PowerShell (e.g., `cmd.exe`).

---

## SPL used in this lab (exact query from your session)

```spl
index=sysmon source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=1 ipconfig
| xmlkv
| search EventID=1
```