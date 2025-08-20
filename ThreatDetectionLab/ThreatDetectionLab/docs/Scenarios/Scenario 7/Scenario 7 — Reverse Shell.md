# ✅ Scenario 7 — Reverse Shell

**Goal:** Demonstrate that establishing an outbound reverse-shell–style connection from a Windows host to a Kali listener produces **Sysmon Event ID 3 (Network Connection)**, **Windows Firewall/Security Event ID 5156**, and is visible in Suricata/Splunk for correlation.

---

## Evidence (text-only, no images)
- **Kali (Listener):** Netcat listener (`nc -lvnp 4444`) received a connection from Windows host `192.168.2.67`, printing `hello from TOLUADEKORE2`.
- **Windows (Outbound):** PowerShell created a TCP session to `192.168.2.131:4444`, simulating a reverse shell connection.
- **Event Viewer — Sysmon:** **Event ID 3 – Network Connection** recorded:  
  - `Image`: powershell.exe  
  - `DestinationIp`: 192.168.2.131  
  - `DestinationPort`: 4444  
  - `SourceIp`: 192.168.2.67  
  - `Computer`: TOLUADEKORE2  
- **Event Viewer — Security (Windows Firewall):** **Event ID 5156 – Filtering Platform Connection** recorded:  
  - `Application Name`: powershell.exe  
  - `Direction`: Outbound  
  - `SourceAddress`: 192.168.2.67  
  - `DestinationAddress`: 192.168.2.131  
  - `DestinationPort`: 4444  
- **Splunk:** Both Sysmon (Event ID 3) and Security (Event ID 5156) events indexed, showing outbound connection initiated by PowerShell.
- **Security Onion / Suricata:** Flow (and possible alert, depending on rules) observed for traffic `192.168.2.67 → 192.168.2.131:4444`.

---

## Commands used in this lab (exact commands from your session)

Kali — Listener
```bash
nc -lvnp 4444
## Windows — Safe Outbound Connect (PowerShell)

- $ip="192.168.2.131"; $port=4444
- $client = New-Object Net.Sockets.TcpClient($ip, $port)
- $stream = $client.GetStream()
- $bytes = [Text.Encoding]::ASCII.GetBytes("hello from $env:COMPUTERNAME`n")
- $stream.Write($bytes,0,$bytes.Length)
- Start-Sleep -Seconds 3
- $client.Close()

```

## SPL used in this lab (exact queries from your session)
Sysmon (Event ID 3)
```spl
index=sysmon source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=3
| search DestinationIp=192.168.2.131 DestinationPort=4444
```
## Windows Security (5156)
```spl
index="wineventloguser" (EventCode=5156 OR EventID=5156)
| search Destination_Address=192.168.2.131

