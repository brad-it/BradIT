#nmap #networkdiscovery

# Nmap Overview

Nmap is a powerful open-source network security scanner used to discover hosts and services on a network. It's essential for penetration testing, network auditing, and reconnaissance.

## What Nmap Does

1. Creation of the map
2. Host Identification
3. Service Identification
4. Service Details Identification

---

# Installing Nmap on Linux

## Ubuntu / Debian

```bash
sudo apt update
sudo apt install nmap -y
```

## Fedora

```bash
sudo dnf install nmap
```

## Arch Linux

```bash
sudo pacman -S nmap
```

## From Source (Latest Version)

```bash
git clone https://github.com/nmap/nmap.git
cd nmap
./configure
make
sudo make install
```

---

## Verify Installation

```bash
nmap --version
```

---

# Nmap Targeting

| Command | Description |
|---|---|
| nmap 192.168.1.1 | Scanning a single IP |
| nmap www.domain.com | Scanning a hostname |
| nmap 192.168.1.1-100 | Scanning an IP range |
| nmap 192.168.1.1/24 | Scanning a subnet |
| nmap -iL list.txt | Scanning from a predefined list |

---

# Nmap Scan Types

| Command | Description |
|---|---|
| nmap -sS <target> | TCP SYN Scan, also known as a stealth scan. When nmap receives a SYN ACK on a probed port, it means the port exists and is open. This is a fast and accurate scan |
| nmap -sT <target> | More accurate than -sS but it is slower and makes more noise, thus more detectable by firewalls |
| nmap -sU <target> | This is used to scan UDP ports. This is typically slow |
| nmap -sn <target> | This is a simple and fast ping scan to see which hosts reply to an ICMP ping packet |
| nmap -sV <target> | This is a service version scan. In order to know which exploits will work, it is helpful to know the service version behind an open port |
| nmap -O <target> | This is a remote OS detection scan. We use this scan to learn what OS the target is running |
| nmap -A <target> | This is an aggressive scan. Performs OS & Version Detection, script scanning, and traceroute. This is the nosiest scan |
