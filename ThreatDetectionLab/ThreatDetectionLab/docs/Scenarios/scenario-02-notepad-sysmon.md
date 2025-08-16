# ✅ Scenario 2 — Notepad Process Create (Sysmon)

**Goal:** Show that launching Notepad generates **Sysmon Event ID 1 (Process Create)** and is searchable in Splunk with useful context.

---

## Evidence

**Event Viewer — Sysmon Event ID 1 (ProcessCreate) for Notepad**  
![Scenario 2 – Event Viewer ProcessCreate](docs/images/Screenshot%202025-08-16%20002404.png)

**Notepad running on the workstation**  
![Scenario 2 – Notepad Running](docs/images/Screenshot%202025-08-16%20033609.png)

**Splunk search showing the Sysmon event for Notepad**  
![Scenario 2 – Splunk Search](docs/images/Screenshot%202025-08-16%20033858.png)

> Save these screenshots in `docs/images/` in your repo (filenames already match the links above).

---

## SPL used in this lab (exact query from your session)

```spl
index=sysmon source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventID notepad.exe
| xmlkv
| search EventID=1
```

- `xmlkv` parses the XML so fields like **Image**, **CommandLine**, **ParentImage** are usable in the search.
- `search EventID=1` narrows to **Process Create**.
