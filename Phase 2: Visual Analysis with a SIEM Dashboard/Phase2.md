
# Phase 2: Visual Analysis with Splunk SIEM

##  Objective
Use **Splunk** as a SIEM platform to collect, forward, and visualize security logs from a victim machine (Metasploitable3) or honeypot system.

---

##  Environment Setup

| Machine         | Role            | IP Address       |
|----------------|------------------|------------------|
| Kali Linux     | Attacker         | 192.168.64.2     |
| Metasploitable3| Victim           | 192.168.64.3     |
| Local Machine  | SIEM             | 192.168.129.1    |

---

## Part 1: Splunk SIEM Setup (Server & Web Interface)

### Step 1: Install Splunk Server (SIEM)
```bash
wget -O splunk-9.3.2.deb https://download.splunk.com/products/splunk/releases/9.3.2/linux/splunk-9.3.2-d8bb32809498-linux-2.6-amd64.deb
sudo dpkg -i splunk-9.3.2.deb
sudo apt --fix-broken install
sudo /opt/splunk/bin/splunk start --accept-license
```

### Step 2: Access Splunk Interface
Navigate to `http://127.0.0.1:8000`  
Login with:
- Username: `admin`
- Password: *(Was set during first boot)*

![Splunk Admin Interface](screenshots/mainPage.jpeg)

---

##  Part 2: Extract Log Files (Victim & Attacker)

### Step 1: Install Splunk Universal Forwarder
```bash
wget -O splunkforwarder-9.4.1.deb https://download.splunk.com/products/universalforwarder/releases/9.4.1/linux/splunkforwarder-9.4.1-e3bdab203ac8-linux-arm64.deb
sudo dpkg -i splunkforwarder-9.4.1.deb
sudo apt --fix-broken install
sudo /opt/splunkforwarder/bin/splunk start --accept-license
```

### Step 2 Configure the Forwarder
```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server <splunk-server-ip>:9997
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log
sudo /opt/splunkforwarder/bin/splunk enable boot-start
```

---

##  Part 3: Log Upload and Visualization in Splunk

### Step 1: Uploading `auth.log` (Manual if no forwarder)
```bash
scp user@victim:/var/log/auth.log ~/Desktop/
```

#### Splunk Upload Walkthrough:
- **Upload File**: `auth.log`
- **Set Source Type**: `auth_log`
- **Define Host & Index**
- **Confirm Upload**

 Screenshots:

- ![File Upload](https://raw.githubusercontent.com/USERNAME/REPO/main/screenshots/select-source.png)
- ![Review Step](https://raw.githubusercontent.com/USERNAME/REPO/main/screenshots/upload-review.png)
- ![Success Message](https://raw.githubusercontent.com/USERNAME/REPO/main/screenshots/upload-successful.png)

---

##  Part 4: Queries & Dashboard

###  Example Queries

#### 1. Failed Password Attempts Over Time
```spl
index=main sourcetype="auth_log" "Failed password"
| timechart span=1h count
```

![Failed Password Chart](https://raw.githubusercontent.com/USERNAME/REPO/main/screenshots/search-failed-bar-chart-updated.png)

---

#### 2. Accepted Passwords by IP
```spl
index=main sourcetype="auth_log" "Accepted password"
| rex "from (?<ip>\d+\.\d+\.\d+\.\d+)"
| stats count by ip
| sort -count
```

![Accepted Passwords](https://raw.githubusercontent.com/USERNAME/REPO/main/screenshots/search-accepted-bar-chart.png)

---

### Save Panel to Dashboard
![Save Dashboard](https://raw.githubusercontent.com/USERNAME/REPO/main/screenshots/save-to-dashboard.png)

---

##  Troubleshooting & Common Errors

### Log File Not Found
```bash
cat /var/log/auth.log
# No such file or directory
```
![Missing Log](https://raw.githubusercontent.com/USERNAME/REPO/main/screenshots/missing-auth-log-kali.png)

---

##  What to Monitor in Splunk

| Item                 | Reason                              |
|----------------------|-------------------------------------|
| `auth.log`           | Login attempts, brute-force signs   |
| `syslog`             | System-wide alerts, reboots         |
| `messages`           | Kernel messages, error reporting    |
| `secure`             | Authentication-related events       |

---

##  Summary

This phase demonstrated:
- How to install and configure Splunk SIEM
- Use of a forwarder to collect logs
- Manual log upload and analysis
- Creating dashboards to detect intrusions


