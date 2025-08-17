# ✅ Scenario 4 — Suspicious File Drop (C:\Users\Public)

**Goal:** Show that dropping an executable into `C:\Users\Public` generates **Sysmon Event ID 11 (File Create)** and is searchable in Splunk with useful context.

---

## Evidence (text-only, no images)
- **Event Viewer:** Sysmon **Event ID 11 – File Create** for `C:\Users\Public\suspicious.exe` (or similar, e.g., filename beginning with `sus`) in *Microsoft-Windows-Sysmon/Operational*.
- **Host state:** A `.exe` was placed in `C:\Users\Public` via copy/rename.
- **Splunk:** Search returns the FileCreate event with key fields (TargetFilename, Image, Computer). Splunk rules may alert on sensitive paths like `C:\Users\Public`.

---

## SPL used in this lab (exact query from your session)

```spl
index=sysmon source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventID Image sus | xmlkv | search EventID=11
