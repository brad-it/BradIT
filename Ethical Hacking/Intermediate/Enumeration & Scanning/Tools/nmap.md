# Nmap Overview

Nmap is a security scanner used to discover hosts and services on a network. It performs the following key functions:

1. Creation of the network map  
2. Host identification  
3. Service identification  
4. Service details identification  

---

## Nmap Targeting

| Command              | Description                     |
| -------------------- | ------------------------------- |
| `nmap 192.168.1.1`     | Scan a single IP address        |
| `nmap www.domain.com`  | Scan a hostname                 |
| `nmap 192.168.1.1-100` | Scan an IP address range        |
| `nmap 192.168.1.1/24`  | Scan a subnet                   |
| `nmap -iL list.txt`    | Scan targets from a file list   |

---

## Nmap Scan Types

| Command              | Description |
| -------------------- | ----------- |
| `nmap -sS <target>` | TCP SYN scan (stealth scan). If Nmap receives a SYN-ACK, the port is open. Fast and accurate. |
| `nmap -sT <target>` | TCP Connect scan. More accurate than `-sS` but slower and noisier, making it easier to detect. |
| `nmap -sU <target>` | UDP scan used to discover open UDP ports. Typically slow. |
| `nmap -sn <target>` | Ping scan to identify which hosts are online using ICMP. |
| `nmap -sV <target>` | Service version detection scan to identify service versions on open ports. |
| `nmap -O <target>`  | Remote OS detection scan to identify the operating system of the target. |
| `nmap -A <target>`  | Aggressive scan that enables OS detection, version detection, script scanning, and traceroute. Very noisy. |
