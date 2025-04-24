#  Phase 3: Defensive Strategy – SSH Protection with Fail2Ban

##  Objective
To secure the SSH service on the victim machine (Metasploitable3) by using **Fail2Ban** to block brute-force login attempts. The effectiveness of the defense is validated by re-running the attack from Phase 1 and showing the IP gets banned.

---

##  Defense Mechanism Used: **Fail2Ban**

Fail2Ban is a log-parsing tool that protects services like SSH from brute-force attacks by banning IPs with repeated failed login attempts.

---
##  Step-by-Step Defense Implementation

---

### Step 1: Install Fail2Ban

On the **victim machine** (Metasploitable3):

```bash
sudo apt update
sudo apt install fail2ban -y
```
---

### Step 2: Backup and Edit Jail Config
After installing Fail2Ban, you need to edit the jail.local file to configure protection for the SSH service.
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```
---
### Step 3: Configure [sshd] Section
Add the following section
```bash
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 600
findtime = 600
```
This means if someone fails to log in 6 times within 10 minutes, they get banned for 10 minutes.

---
### Step 4: Restart Fail2Ban


