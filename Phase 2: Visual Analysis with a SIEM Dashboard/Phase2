
# 📊 ICS344 – Phase 2: Visual Analysis with a SIEM Dashboard (Splunk)

## 🎯 Objective
Use **Splunk** to collect and visualize logs from the **victim (Metasploitable3)** and optionally a **honeypot**. The objective is to detect, analyze, and compare attack patterns based on log data.

---

## 🧰 Tools Used

| Tool               | Purpose                                               |
|--------------------|-------------------------------------------------------|
| Splunk             | SIEM tool for log collection and visualization        |
| Metasploitable3    | Vulnerable machine acting as the victim               |
| Kali Linux         | Attacker machine (can also run Splunk)                |
| SCP / Shared Folders | Transfer logs from Metasploitable3 to Kali           |

---

## 🛠️ Setup Instructions

### 1. Install Splunk
- Download Splunk from: [https://www.splunk.com](https://www.splunk.com)
- Install using `.deb` or `.rpm` on your machine.
- Access it from your browser: `http://localhost:8000`
- Default credentials:
  ```plaintext
  Username: admin
  Password: changeme
  ```

---

### 2. Transfer Logs from Victim Machine

```bash
# On Metasploitable3
sudo cp /var/log/auth.log /home/vagrant/
sudo chown vagrant:vagrant /home/vagrant/auth.log

# From Kali Linux or host machine
scp vagrant@<victim-ip>:/home/vagrant/auth.log ~/Desktop/
```

📷 *Log Transfer*  
![Log Copy Commands](screenshots/log-copy-commands.png)

---

### 3. Upload Logs to Splunk

#### Step-by-Step:
1. Go to **Settings → Add Data**
2. Select **Upload**
3. Choose `auth.log`  
📷  
![Select File](screenshots/select-source.png)

4. Set Source Type  
📷  
![Set Source Type](screenshots/set-source-type-view.png)

5. Save Source Type (optional)  
📷  
![Save Source Type](screenshots/save-source-type.png)

6. Input Host & Index (e.g., Default or Custom)  
📷  
![Input Settings](screenshots/input-settings.png)

7. Review before submit  
📷  
![Upload Review](screenshots/upload-review.png)

8. Upload successful confirmation  
📷  
![Upload Successful](screenshots/upload-successful.png)

---

## 📊 Visualization in Splunk

### Example Search Queries

#### 1. Failed Logins
```spl
index=main sourcetype="auth_log" "Failed password"
```

#### 2. Failed Logins Over Time
```spl
index=main sourcetype="auth_log" "Failed password"
| timechart span=1h count
```

📷  
**Timechart for Failed Logins**  
![Failed Timechart](screenshots/search-failed-bar-chart-updated.png)

---

#### 3. Accepted Logins by IP
```spl
index=main sourcetype="auth_log" "Accepted password"
| rex "from (?<ip>\d+\.\d+\.\d+\.\d+)"
| stats count by ip
| sort -count
```

📷  
![Accepted Chart](screenshots/search-accepted-bar-chart.png)

---

#### 4. Accepted Logins Over Time
```spl
index=main sourcetype="auth_log" "Accepted password"
| timechart span=1h count
```

📷  
**Accepted Passwords (Sparse Data)**  
![Accepted Sparse](screenshots/accepted-timechart-empty.png)

📷  
**Accepted Passwords (Minimal Data)**  
![Accepted Minimal](screenshots/accepted-timechart-single.png)

---

### Dashboard Creation

After generating charts:
- Click **Save As** → **New Dashboard Panel**

📷  
![Save to Dashboard](screenshots/save-to-dashboard.png)

---

## ⚠️ Issues Encountered

### Missing File Example
```bash
cat /var/log/auth.log
# Error: No such file or directory
```

📷  
![Missing File](screenshots/missing-auth-log-kali.png)

---

## 🖼️ Other UI Snapshots

📷 Splunk Admin View  
![Admin Dashboard](screenshots/splunk-admin-dashboard.png)

📷 Data Input Options  
![Data Options](screenshots/data-source-options.png)

📷 Visualization Not Showing Warning  
![No Visualization](screenshots/search-no-visualization.png)

---

## ✅ Summary

- Splunk successfully used to ingest and analyze logs.
- Key visualizations include login attempts over time, source IP analysis, and comparison views.
- Issues such as missing log files were encountered and documented.

---

## 👥 Contributors

- **Name**: Farah Almutairi  
- **Course**: ICS344  
- **Instructor**: [Insert Name Here]  
- **Phase**: 2 – Log Analysis Using SIEM


