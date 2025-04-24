#  Phase 3: Defensive Strategy – SSH Protection with Fail2Ban

##  Objective
To secure the SSH service on the victim machine (Metasploitable3) by using **Fail2Ban** to block brute-force login attempts. The effectiveness of the defense is validated by re-running the attack from Phase 1 and showing the IP gets banned.

---

##  Defense Mechanism Used: **Fail2Ban**

Fail2Ban is a log-parsing tool that protects services like SSH from brute-force attacks by banning IPs with repeated failed login attempts.

---

##  Step-by-Step Implementation

### 1. Install Fail2Ban
```bash
sudo apt update
sudo apt install fail2ban -y
```
