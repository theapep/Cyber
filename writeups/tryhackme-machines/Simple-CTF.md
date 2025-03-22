**Simple CTF Walkthrough by The Apep**

**Room Name**: Simple CTF  
**Platform**: TryHackMe  
**Difficulty**: Easy  
**Objective**: Find 3 flags hidden in a Linux-based machine.

---

## 🛠 Tools Used
- `nmap` for port scanning
- `gobuster` for directory enumeration
- `netcat` for reverse shell
- `Linux CLI` for file exploration

---

## 🔍 Step-by-Step Walkthrough

### Step 1: Initial Recon with Nmap
```bash
nmap -sC -sV -oA simplectf 10.10.10.10
```
**Findings**:
- Port 22 (SSH) open
- Port 80 (HTTP) open — Apache server running

### Step 2: Web Enumeration
Checked `http://10.10.10.10` in browser → default Apache page.

Ran Gobuster:
```bash
gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt
```
Found `/flag.txt`, which gave us **Flag 1**.

### Step 3: Looking Deeper
View source of the page — found reference to `/robots.txt` → led to a hidden directory `/hidden/`
Found a login page there.

### Step 4: Gaining Access
Tried basic SQL Injection on login form:
```sql
admin' OR '1'='1
```
It worked. We got into a simple dashboard with a file upload field.

### Step 5: Uploading Reverse Shell
- Created a PHP reverse shell (`php-reverse-shell.php`)
- Set up listener:
```bash
nc -lvnp 4444
```
- Uploaded file, triggered it by accessing the URL

**Reverse shell obtained!** 🐚

### Step 6: Privilege Escalation
Found a user flag in `/home/user/` → **Flag 2**
Checked `sudo -l` — user can run `vim` as sudo

Used this to escalate:
```bash
sudo vim -c '!sh'
```
Got root shell.

### Step 7: Root Flag
Navigated to `/root/flag.txt` → **Flag 3 obtained.**

---

## 🧠 Lessons Learned
- Web recon often reveals hidden gems (robots.txt, etc.)
- Even basic SQLi can break weak logins
- Always check `sudo -l` after gaining shell access
- TryHackMe's "easy" boxes are perfect for building real-world skills

---

## ✅ Flags Captured
- Flag 1: Web directory discovery
- Flag 2: User flag after reverse shell
- Flag 3: Root flag via privilege escalation


---
**Walkthrough by TheApep. For educational use only.**

