
## 🛠️ Commands Used (SPL Queries) with Quick Explanations

### ✅ Scenario 1 - Successful User Login

**Explanation:** Simulates a normal, valid login to the domain.  
**What We Are Looking For:**  
Successful login event (`4624`) confirming user authenticated properly.

**SPL Query:**  
```
index=wineventlog EventCode=4624
| stats count by Account_Name, Computer_Name, Logon_Type
```

### ✅ Scenario 2 - Failed Login Attempt

**Explanation:** Tests detection of incorrect password attempt.  
**What We Are Looking For:**  
Failed login event (`4625`) showing the username, source IP, and failure reason.

**SPL Query:**  
```
index=wineventlog EventCode=4625
| stats count by Account_Name, Computer_Name, Source_IP, Failure_Reason
```

### ✅ Scenario 3 - Common Process Execution

**Explanation:** Basic process execution like opening Notepad or Calculator.  
**What We Are Looking For:**  
Process creation event (Sysmon `EventCode=1`) showing process name and command line.

**SPL Query:**  
```
index=sysmon EventCode=1
(Process_Name="notepad.exe" OR Process_Name="calc.exe")
| stats count by Computer_Name, Process_Name, Command_Line
```

### ✅ Scenario 4 - PowerShell Execution

**Explanation:** PowerShell is often abused by attackers, so we track its use.  
**What We Are Looking For:**  
Process creation event for `powershell.exe` with command line arguments visible.

**SPL Query:**  
```
index=sysmon EventCode=1 Process_Name="powershell.exe"
| stats count by Computer_Name, Command_Line, Parent_Process_Name
```

### ✅ Scenario 5 - Suspicious File Drop

**Explanation:** Files dropped in public directories are often used for staging attacks.  
**What We Are Looking For:**  
File creation event for `.exe` files in `C:\Users\Public`.

**SPL Query:**  
```
index=sysmon EventCode=11
File_Path="C:\\Users\\Public\\*.exe"
| stats count by Computer_Name, File_Name, File_Path
```

### ✅ Scenario 6 - Outbound Network Connection

**Explanation:** Tests if outbound connections from the host are visible.  
**What We Are Looking For:**  
Network connection event showing destination IP and port (Port 80 example).

**SPL Query:**  
```
index=sysmon EventCode=3
Destination_IP=* Destination_Port=80
| stats count by Computer_Name, Destination_IP, Destination_Port
```

### ✅ Scenario 7 - Mimikatz Execution

**Explanation:** Mimikatz is a common post-exploitation tool for credential dumping.  
**What We Are Looking For:**  
Suspicious process creation event for `mimikatz.exe` or similar command lines.

**SPL Query:**  
```
index=sysmon EventCode=1
(Process_Name="mimikatz.exe" OR Command_Line="*mimikatz*")
| stats count by Computer_Name, Process_Name, Command_Line
```

### ✅ Scenario 8 - RDP Lateral Movement

**Explanation:** Remote Desktop access from Kali simulates lateral movement.  
**What We Are Looking For:**  
`4624` Logon Event with `Logon_Type=10` (Remote Desktop), source IP from Kali visible.

**SPL Query:**  
```
index=wineventlog EventCode=4624 Logon_Type=10
| stats count by Account_Name, Computer_Name, Source_IP
```

### ✅ Scenario 9 - Reverse Shell

**Explanation:** Reverse shells are common for remote control during attacks.  
**What We Are Looking For:**  
Outbound connection to Kali's listener IP/Port. Detected via Sysmon or Suricata.

**SPL Query:**  
```
index=sysmon EventCode=3
Destination_IP=<Kali_IP> Destination_Port=<Listener_Port>
| stats count by Computer_Name, Destination_IP, Destination_Port
```

*Replace `<Kali_IP>` and `<Listener_Port>` with actual values used in your lab.*

### ✅ Scenario 10 - Privilege Escalation (Admin Group Modification)

**Explanation:** Attackers often add themselves to privileged groups for persistence.  
**What We Are Looking For:**  
Event ID `4732` showing user added to Administrators or other sensitive groups.

**SPL Query:**  
```
index=wineventlog EventCode=4732
| stats count by Target_Username, Group_Name, Computer_Name
```

### ✅ Scenario 11 - Malicious Scheduled Task

**Explanation:** Scheduled tasks are a common persistence mechanism for attackers.  
**What We Are Looking For:**  
Task creation event (`4698`) showing task name, associated binary, and account.

**SPL Query:**  
```
index=wineventlog EventCode=4698
| stats count by Task_Name, Command, Computer_Name, Account_Name
```
