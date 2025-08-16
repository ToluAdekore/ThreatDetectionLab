# ✅ Scenario 2 — Notepad Process Create (Sysmon)

**Goal:** Show that launching Notepad generates **Sysmon Event ID 1 (Process Create)** and is searchable in Splunk with useful context.

---

## Evidence

**Splunk search showing the Sysmon event for Notepad**  
![Scenario 2 – Sysmon Notepad Search](docs/images/Screenshot%202025-08-16%20103357.png)

**Notepad running on the workstation**  
![Scenario 2 – Notepad Running](docs/images/Screenshot%202025-08-16%20033609.png)

> Put these screenshots in `docs/images/` in the repo (or update the paths above to match your structure).

---

## SPL used in this lab (exact query from the screenshot)

```spl
index=sysmon source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventID notepad.exe
| xmlkv
| search EventID=1
```

- `xmlkv` parses the XML so fields like **Image**, **CommandLine**, **ParentImage** are usable in the search.
- `search EventID=1` narrows to **Process Create**.

### (Optional) Recommended normalized SPL (when the Sysmon TA is in place)
```spl
index=sysmon sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 Image="*\\notepad.exe"
| table _time Computer Image CommandLine ParentImage User ProcessGuid Hashes
| sort - _time
```

---

## Prerequisites

- **Sysmon** installed with `ProcessCreate` enabled.
- **Splunk UF** forwarding the Sysmon channel in XML:
  ```conf
  [WinEventLog://Microsoft-Windows-Sysmon/Operational]
  disabled = 0
  renderXml = true
  index = sysmon
  ; sourcetype = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational  ; (recommended)
  ```
- **Splunk Add-on for Microsoft Sysmon** installed on the search head (for automatic field extractions).

---

## Steps to Generate the Event

1. On the workstation, launch Notepad:
   ```powershell
   Start-Process notepad.exe
   ```
2. Type a few characters, then close Notepad.

*Quick generator (open/close pair):*
```powershell
Start-Process notepad.exe; Start-Sleep -Seconds 2; Stop-Process -Name notepad -ErrorAction SilentlyContinue
```

---

## Verify in Event Viewer

- **Event Viewer** → **Applications and Services Logs** → **Microsoft** → **Windows** → **Sysmon** → **Operational**  
- Find **Event ID 1 – Process Create** for `C:\Windows\System32\notepad.exe`.

---

## Verify in Splunk

Use the **SPL used in this lab** (above), or the normalized SPL if your sourcetype is already
`XmlWinEventLog:Microsoft-Windows-Sysmon/Operational`.

---

## Acceptance Criteria

- At least **one** Sysmon **EventID/EventCode=1** record for **notepad.exe**.
- Event contains `Image`, `CommandLine`, `ParentImage`, `User` (via `xmlkv` or the Sysmon TA).
- Event visible in **both** Event Viewer and **Splunk**.

---

## Troubleshooting

- **Only raw XML shows:** keep using `| xmlkv` in the search, or set the sourcetype on the UF to  
  `XmlWinEventLog:Microsoft-Windows-Sysmon/Operational` so the Sysmon TA auto-extracts fields.
- **No events:** confirm UF is running and forwarding to your indexer (port 9997) and that `index=sysmon` exists.
