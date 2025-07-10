# 🛠️ ThreatDetectionLab Setup Guide

This guide walks you through building a full Blue Team cybersecurity home lab in VMware Workstation, inspired by [Cyberwox Academy's Homelab Guide](https://blog.cyberwoxacademy.com/post/building-a-cybersecurity-homelab). It includes networking, VM configuration, monitoring, and detection stack.

---

## 🧱 1. Lab Goals

- Build a realistic Security Operations Center (SOC)-style lab
- Simulate adversary behavior using offensive tools
- Monitor, detect, and analyze attacks using blue team tools

---

## 🖥️ 2. Host Machine Requirements

- OS: Windows 10/11 or Linux host
- CPU: 6+ cores recommended
- RAM: 16–32 GB
- Storage: 250+ GB SSD recommended
- Virtualization: Enable VT-x/AMD-V in BIOS

---

## 🔌 3. Install VMware Workstation Pro or Player

- [Download VMware Workstation Pro](https://www.vmware.com/products/workstation-pro.html)
- Open **Virtual Network Editor** and create these networks:

| VMnet  | Subnet           | Purpose                            |
|--------|------------------|------------------------------------|
| VMnet2 | 192.168.1.0/24   | Attacker Network (Kali)            |
| VMnet3 | 192.168.2.0/24   | Victim Network (Windows, DVWA)     |
| VMnet4 | 192.168.3.0/24   | Monitoring Network (Security Onion)|
| VMnet5 | N/A              | SPAN / Mirror Port                 |
| VMnet6 | 192.168.4.0/24   | Management/Logging (Splunk)        |

All should be **Host-only** networks. VMnet5 will be used for sniffing only.

---

## 🏗️ 4. Virtual Machines to Deploy

### ✅ 1. **pfSense (Firewall & Router)**
- Connects to VMnet2, VMnet3, VMnet4, VMnet6, and VMnet5 (SPAN port)
- Routes traffic between networks
- Sets DHCP/static IPs, DNS, and NAT rules

### ✅ 2. **Kali Linux (Attacker)**
- Connected to VMnet2
- Used to simulate attacks (Hydra, Nmap, Metasploit, etc.)

### ✅ 3. **Windows Server (Domain Controller)**
- Connected to VMnet3
- Hosts Active Directory and Group Policy
- Splunk Universal Forwarder installed

### ✅ 4. **Windows 10 or 11 Endpoint**
- Domain-joined to Windows Server
- Sysmon and Splunk Forwarder installed

### ✅ 5. **Security Onion**
- Connected to VMnet4 (mgmt) and VMnet5 (SPAN port)
- Tools: Zeek, Suricata, Wazuh, Elasticsearch, Kibana

### ✅ 6. **Splunk Server**
- Connected to VMnet6
- Collects logs from all endpoints

### ✅ 7. **Optional Vulnerable Boxes**
- DVWA, Metasploitable2, VulnHub targets
- Connected to VMnet3

---

## 🧠 5. Detection Stack

| Tool         | Role                             |
|--------------|----------------------------------|
| Suricata     | Network-based IDS (Security Onion) |
| Zeek         | Network metadata (Security Onion) |
| Wazuh        | Host IDS (Security Onion)         |
| Splunk       | Log aggregation and SIEM         |
| Sysmon       | Endpoint telemetry (Windows)     |
| pfSense Logs | Optional firewall logging         |

---

## 🔐 6. Simulated Attacks

- Brute Force (RDP, SSH, SMB via Hydra/Ncrack)
- Reverse Shells (using Netcat, MSFvenom)
- Malware execution and registry persistence (PowerShell)
- Web exploits on DVWA
- Port scans and recon (Nmap)

---

## ✅ 7. Logging & Monitoring

- Universal Forwarders on Windows machines push logs to Splunk
- Security Onion listens to SPAN port and processes all traffic
- Use Kibana and Splunk dashboards to detect malicious activity

---

## 📁 8. File Storage and Snapshots

- Take snapshots before/after attack scenarios
- Store evidence (logs, screenshots, PCAPs) in organized folders
- Backup Splunk configs and search queries regularly

---

## 🙏 Acknowledgment

This lab setup was inspired and modeled after [Cyberwox Academy’s Cybersecurity Homelab Guide](https://blog.cyberwoxacademy.com/post/building-a-cybersecurity-homelab). All configurations and implementations are adapted for educational use.

---

**Build. Break. Detect. Repeat.**

