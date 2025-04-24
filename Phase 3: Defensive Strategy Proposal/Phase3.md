#  Phase 3: Defensive Strategy – SSH Protection with Fail2Ban

##  Objective
To secure the SSH service on the victim machine (Metasploitable3) by using **Fail2Ban** to block brute-force login attempts. The effectiveness of the defense is validated by re-running the attack from Phase 1 and showing the IP gets banned.

---

##  Defense Mechanism Used: **Fail2Ban**

Fail2Ban is a log-parsing tool that protects services like SSH from brute-force attacks by banning IPs with repeated failed login attempts.

---

##  Step-by-Step Implementation

### Step 1: Install and Configure Fail2Ban on the Victim Machine
```bash
sudo apt update
sudo apt install fail2ban -y
```

### Step 2: Configure jail.local Using nano
After installing Fail2Ban, you need to edit the jail.local file to configure protection for the SSH service.
```bash
sudo nano /etc/fail2ban/jail.local
```
This opens the file in the nano text editor.
Scroll to the [sshd] Section and  
add this block at the end of the file:
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
