#hydra #passwordcracking

# Hydra Overview

## What is Hydra?

Hydra is a fast, flexible, and powerful open-source password cracking tool that supports attacking numerous protocols. It's designed for credential brute-forcing across a wide range of services.

It is commonly used by:

- Penetration testers
- Security researchers
- Bug bounty hunters
- System administrators
- Incident response teams

Hydra can:

- Brute-force SSH, FTP, Telnet, HTTP, and many other protocols
- Support dictionary and brute-force attacks
- Parallelize attacks across multiple threads
- Handle custom wordlists
- Test weak credentials across services
- Perform distributed attacks

---

## Key Features

- **Fast**: Multi-threaded architecture for rapid credential testing
- **Flexible**: Supports 50+ protocols (SSH, FTP, HTTP, SMB, SNMP, etc.)
- **Customizable**: Works with any wordlist and custom payloads
- **Efficient**: Minimal resource usage with high performance
- **Reliable**: Well-maintained and actively developed

---

# Installing Hydra on Linux

## Ubuntu / Debian

```bash
sudo apt update
sudo apt install hydra -y
```

## Fedora

```bash
sudo dnf install hydra
```

## Arch Linux

```bash
sudo pacman -S hydra
```

## From Source (Latest Version)

```bash
git clone https://github.com/vanhauser-thc/thc-hydra.git
cd thc-hydra
./configure
make
sudo make install
```

---

## Verify Installation

```bash
hydra --version
```

---

# Basic Terminology

| Term | Meaning |
|---|---|
| Brute Force | Trying every possible combination of passwords |
| Dictionary Attack | Using a predefined list of passwords |
| Wordlist | Text file containing possible passwords |
| Protocol | Service being attacked (SSH, HTTP, FTP, etc.) |
| Credentials | Username and password combination |
| Threading | Running multiple attacks simultaneously |

---

# Common Protocols Supported

| Protocol | Port | Example |
|---|---|---|
| SSH | 22 | `hydra -l admin -P pass.txt ssh://target.com` |
| FTP | 21 | `hydra -l admin -P pass.txt ftp://target.com` |
| HTTP | 80 | `hydra -l admin -P pass.txt http-get://target.com` |
| HTTPS | 443 | `hydra -l admin -P pass.txt https-get://target.com` |
| SMB | 445 | `hydra -l admin -P pass.txt smb://target.com` |
| Telnet | 23 | `hydra -l admin -P pass.txt telnet://target.com` |
| MySQL | 3306 | `hydra -l root -P pass.txt mysql://target.com` |
| PostgreSQL | 5432 | `hydra -l postgres -P pass.txt postgres://target.com` |

---

# Basic Usage

## SSH Brute Force (Dictionary Attack)

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.100
```

## HTTP Form Brute Force

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt http-post-form://target.com:80 "/login.php:username=^USER^&password=^PASS^:F=Invalid"
```

## FTP Brute Force

```bash
hydra -l admin -P wordlist.txt ftp://target.com
```

## Multiple Usernames

```bash
hydra -L usernames.txt -P passwords.txt ssh://target.com
```

---

# Common Flags Reference

| Flag | Purpose | Example |
|---|---|---|
| `-l` | Single username | `-l admin` |
| `-L` | Username wordlist | `-L usernames.txt` |
| `-p` | Single password | `-p password123` |
| `-P` | Password wordlist | `-P rockyou.txt` |
| `-t` | Number of threads | `-t 16` |
| `-T` | Connection timeout | `-T 30` |
| `-s` | Target port | `-s 2222` |
| `-o` | Output file | `-o results.txt` |
| `-f` | Stop after first valid pair found | `-f` |
| `-v` / `-V` | Verbose output | `-v` |

---

# Tips and Best Practices

- ✅ Always get written permission before testing
- ✅ Use appropriate wordlists for your target
- ✅ Adjust thread count based on target stability
- ✅ Use the `-f` flag to stop after finding valid credentials
- ✅ Save results to files for documentation
- ❌ Don't use excessive threads on sensitive production systems
- ❌ Don't scan without authorization
