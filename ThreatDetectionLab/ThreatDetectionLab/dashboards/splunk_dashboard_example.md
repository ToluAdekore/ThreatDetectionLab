## 📊 Host-Centric Splunk Dashboard

A centralized Splunk dashboard built to monitor key host-specific activity in a security operations environment. This dashboard was built using Windows event logs and provides insights into:

- ✅ Successful and failed logons
- ⚙️ Process executions
- 🔐 File/registry permission changes

---

### 🧠 Features

| Panel                     | Description                                                |
|---------------------------|------------------------------------------------------------|
| ✅ **Recent Logons**       | See who logged in, when, from where, and how              |
| ❌ **Failed Logons**       | Spot invalid login attempts and brute force patterns       |
| ⚙️ **Top Processes**       | Identify the most frequently run processes per host        |
| 🔐 **Permission Changes**  | Detect file and registry security descriptor modifications |

---

### 🖼️ Screenshots

| 🧭 Overview | 🔍 Detail View |
|------------|----------------|
| ![Dashboard Overview](./dashboards/Screenshot 2025-07-08 111933.png) | ![Process + Permission Panels](./dashboards/Screenshot%202025-07-08%20111933.png) |

---

### 🧾 Importing the Dashboard

1. Go to **Settings → User Interface → Views → Add New**
2. Name it: `comprehensive_host_dashboard`
3. Paste the XML contents from the file below
4. Click **Save**

📂 XML File: [`dashboards/comprehensive_host_dashboard.xml`](./dashboards/comprehensive_host_dashboard.xml)

---

### 🛠️ Built With

- **Splunk Enterprise**
- Windows Event Logs (`WinEventLog:Security`)
- XML-based Classic Dashboard Editor

---

> 💡 This dashboard is part of my custom SOC lab. Visit the [dashboards/](./dashboards/) folder to explore additional visualizations.
