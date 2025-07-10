# Nmap Port Scanning (Pre-Attack Recon)
---
## Full TCP SYN scan on all ports
### nmap -sS -Pn -T4 -p- -v <target>
---
## Version detection and default scripts
### nmap -sV -sC -p 21,22,80,3389 <target>
---
## SMB-specific vulnerability scripts
### nmap --script vuln -p 445 <target>
