# 🖥️ SOC Lab Network Diagram Notes

---

# 📡 1. Network Segments & IP Ranges

| **Network**           | **VMware Network** | **Subnet**         | **Purpose**                               |
|-----------------------|--------------------|--------------------|-------------------------------------------|
| Attacker / External   | `VMnet2`           | `192.168.1.0/24`   | Kali attacker network                     |
| Victim / Internal     | `VMnet3`           | `192.168.2.0/24`   | Domain Controller & vulnerable targets    |
| Monitoring (Security Onion) | `VMnet4`    | `192.168.3.0/24`   | Security Onion management & monitoring    |
| SPAN / Mirror Port    | `VMnet5`           | _N/A_               | Captures traffic from Victim network      |
| Management / Logs     | `VMnet6`           | `192.168.4.0/24`   | Splunk log collection & admin access      |

---

# 🖥️ 2. Devices & IP Assignments

| **Device**              | **Interface** | **IP Address**     | **Notes**                                   |
|-------------------------|---------------|--------------------|---------------------------------------------|
| Kali Linux (Attacker)   | `em1`         | `192.168.1.10`     | Used for attack simulation & pentesting     |
| pfSense Firewall (WAN)  | `em1`         | `192.168.1.1`      | Gateway between Attacker and Victim network |
| pfSense Firewall (LAN)  | `em2`         | `192.168.2.1`      | Gateway for Victim network                  |
| pfSense Firewall (SPAN) | `em4`         | _N/A_               | Mirrors traffic for Security Onion          |
| pfSense Firewall (Mgmt) | `em5`         | `192.168.4.1`      | Management interface                        |
| Security Onion (Monitoring) | `em3`     | `192.168.3.10`     | IDS/NSM for traffic visibility & detection  |
| Splunk Server (Logs)    | `em5`         | `192.168.4.10`     | Receives logs from Universal Forwarders     |
| Victim Network Targets  | `em2`         | `192.168.2.x`      | Windows DC, Ubuntu, CentOS, DVWA, VulnHub   |

---

# 🛡️ 3. Network Behavior Overview

✅ Kali machine simulates attacker activity targeting Victim Network.  
✅ pfSense controls traffic flow between networks.  
✅ Security Onion monitors Victim Network traffic via SPAN/mirror port.  
✅ Splunk collects logs from endpoints using Universal Forwarders.  
✅ Victim machines include Windows DC, Ubuntu, CentOS, DVWA, VulnHub targets.  

---

# ⚒️ 4. Lab Notes & Recommendations

- All networks are **isolated within VMware** for safety.  
- SPAN/mirror port simulation depends on VMware or pfSense bridging configuration.  
- Dual NIC setup recommended for Security Onion to separate management and monitoring.  
- Snapshots recommended before major testing or exploitation.  
- Validate visibility through Splunk, Security Onion, and manual packet captures.  

---

