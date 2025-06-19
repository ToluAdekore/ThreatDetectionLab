# 🛡️ Blue Team Lab Environment - Full SOC Topology

This repository documents the setup and configuration of a **Blue Team Home SOC Lab** built in **VMware** using **pfSense**, **Security Onion**, **Splunk**, and vulnerable machines (e.g., Metasploitable, DVWA, Ubuntu, etc.).

![Lab Topology](625e6d08-ad15-44a6-a991-4d0220e7cdf6.png)

---

## 🧠 Lab Overview

This lab simulates a real-world Security Operations Center (SOC) setup with:

- 🔥 A firewall and routing layer (`pfSense`)
- 📦 Intrusion Detection/Prevention and packet capture (`Security Onion`)
- 📊 Log aggregation and analysis (`Splunk`)
- 🧪 Vulnerable virtual machines for attack simulation
- 💻 Kali Linux for offensive testing
- 🖥️ Domain Controller & victim network for realistic attacks

---

## 🌐 Network Segments

| Network | Purpose                       | Devices                          |
|--------|-------------------------------|----------------------------------|
| Attacker Network   | Kali Linux                      | Offensive testing                |
| Victim Network     | Windows AD, Ubuntu, DVWA, etc. | Vulnerable systems               |
| Security Monitoring | Security Onion                 | IDS/NSM                          |
| Logging Network    | Splunk Server                  | Log ingestion & analysis         |
| SPAN / Mirror Port | Network monitoring             | Traffic mirror from victim net   |

---

## ⚙️ Components and Configuration

### 1. 🧱 pfSense Firewall
- Routes traffic between attacker, victim, and monitoring networks
- Acts as the central gateway for all VM interfaces
- Configured to mirror traffic from victim network to SPAN port for analysis

### 2. 🧨 Security Onion
- Connected to SPAN port to sniff all victim network traffic
- Tools include:
  - Suricata (IDS/IPS)
  - Zeek (Network analysis)
  - Wazuh (HIDS)
  - Elasticsearch/Kibana for visualization

### 3. 📈 Splunk Server
- Collects logs via Splunk Universal Forwarder and syslog
- Ingests Windows logs, IDS alerts, Sysmon, and more
- Dashboards for threat hunting and alert correlation

### 4. 🧪 Victim Machines
- Includes:
  - Windows Server (Domain Controller)
  - Ubuntu with DVWA
  - Optional: Metasploitable, CentOS, VulnHub machines
- Used to simulate attacks and detection

### 5. ⚔️ Kali Linux Attacker
- Offensive toolset includes:
  - Nmap
  - Hydra
  - Metasploit
  - Burp Suite
- Used to simulate real-world attacks against vulnerable targets

---

## 🔌 Virtual Networking in VMware

Each VM is connected to isolated VMnet adapters:

- `Kali` → Attacker Network (VMnet)
- `pfSense` → Multiple VMnets bridging attacker, victim, monitor, and log networks
- `Security Onion` → Monitoring/SPAN Network
- `Splunk` → Logging Network
- `Victim Machines` → Internal Victim Network

Ensure each VMnet is set to **Host-Only** or **Custom** to segment traffic and simulate real-world isolation.

---

## 📥 Log Sources

Logs collected from:

- Windows Event Logs (via Splunk Universal Forwarder)
- Suricata and Zeek alerts from Security Onion
- Sysmon logs (on Windows machines)
- pfSense firewall logs (optional)

---

## 🧪 Attack Scenarios

| Scenario | Description |
|---------|-------------|
| 🔐 Brute Force | Use Hydra to brute-force RDP or SSH logins |
| 🐛 Web Exploits | Launch XSS or SQLi against DVWA or vulnerable web servers |
| 🧭 Recon | Scan the network with Nmap, analyze detection in Suricata |
| 🦠 Malware Execution | Execute malware samples and observe Sysmon behavior |

---

## 🚀 Lab Goals

- Understand the flow of network-based attacks
- Practice detection and response techniques
- Correlate events across IDS and endpoint logs
- Build a foundational SOC lab for continued blue team learning

---

## 📂 Repository Structure

