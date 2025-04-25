
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

![files content](screenshots/mainPage.jpeg)

---

##  Part 2: Extract Log Files (Victim & Attacker)

### Victim's Log File:
### Step 1: Read and Copy Log File

Read the log file `auth.log` on the **victim machine** (Metasploitable3), and copy it to directory `/home/vagrant/`
```bash
sudo cat /var/log/auth.log
sudo cp /var/log/auth.log /home/vagrant/
```

The ownership of the file was changed so **Kali Linux** could copy it successfully
```bash
sudo chown vagrant:vagrant /home/vagrant/auth.log
```

![files content](screenshots/2_1_1.jpeg)
![files content](screenshots/2_1_2.jpeg)

### Step 2: Transfer Log File
Copy the log file `auth.log` on **Kali Linux**, and send it to **local machine** through email

```bash
scp vagrant@192.168.64.3:/home/vagrant/auth.log ~/Desktop/
```

![files content](screenshots/2_2.jpeg)

### Attacker's Log File:
### Step 3: Copy and Transfer Log File

Copy `valid_credentials` to log file `attacker.log`, and send it to **local machine** through email

```bash
cp ~/valid_credintials.txt ~/Desktop/attacker.log
```

![files content](screenshots/2_3.jpeg)

---

##  Part 3: Log Upload and Visualization in Splunk

### Victim part:
### Step 1: Uploading Log File
1. In Splunk's webpage, click on `Add data`
2. Click on `Upload` to upload a file from the **local machine**
3. In the **Select Source** step, click on `Select File` and choose the log file `auth.log`
4. In the **Set Source Type** step, leave the source type as **default**
5. Cont. in the **Set Source Type** step, name and add description for the source type
6. In the **Input settings** step, configure the **Host** as **Constant value** and **Index** as **Default**
7. In the **Review** step, there is a summary for the configuration
8. In the **Done** step, click on `Start Searching` to start analyzing data

 Screenshots:

1. ![files content](screenshots/3_1.jpeg)
2. ![files content](screenshots/3_2.jpeg)
3. ![files content](screenshots/3_3.jpeg)
4. ![files content](screenshots/3_4.jpeg)
5. ![files content](screenshots/3_5.jpeg)
6. ![files content](screenshots/3_6.jpeg)
7. ![files content](screenshots/3_7.jpeg)
8. ![files content](screenshots/3_8.jpeg)

### Step 2: Queries & Dashboard
#### Failed SSH Login Attempts (Hourly)
```bash
index=main sourcetype="auth_log" "Failed password"
| timechart span=1h count
```
![files content](screenshots/3_9.png)

#### Accepted SSH Login Attempts (Hourly)
```bash
index=main sourcetype="auth_log" "Accepted password"
| timechart span=1h count
```
![files content](screenshots/3_10.png)

#### Top IPs with Successful Logins
```bash
index=main sourcetype="auth_log" "Accepted password"
| rex 'from (?<ip>\d+\.\d+\.\d+\.\d+)'
| stats count by ip
| sort - count
```
![files content](screenshots/3_11.png)

### Attacker part:
### Step 3: Uploading Log File
1. In Splunk's webpage, click on `Add data`
2. Click on `Upload` to upload a file from the **local machine**
3. In the **Select Source** step, click on `Select File` and choose the log file `attacker.log`
4. In the **Set Source Type** step, leave the source type as **default**
5. In the **Input settings** step, configure the **Host** as **Constant value** and **Index** as **Default**
6. In the **Review** step, there is a summary for the configuration

 Screenshots:

1. ![files content](screenshots/3_12.jpeg)
2. ![files content](screenshots/3_13.jpeg)
3. ![files content](screenshots/3_14.jpeg)
4. ![files content](screenshots/3_15.jpeg)
5. ![files content](screenshots/3_16.jpeg)
6. ![files content](screenshots/3_17.jpeg)

### Step 4: Queries & Dashboard

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


