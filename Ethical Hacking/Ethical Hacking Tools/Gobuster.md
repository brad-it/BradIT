#gobuster #webdirectory #bruteforcedirectories

# Gobuster Overview

## What is Gobuster?

Gobuster is a fast, powerful open-source directory and file brute-forcing tool written in Go. It's designed for web reconnaissance and enumeration.

It is commonly used by:

- Penetration testers
- Security researchers
- Bug bounty hunters
- Web application security professionals
- System administrators

Gobuster can:

- Brute-force directories and files
- Enumerate DNS subdomains
- Discover virtual hosts
- Test for common web paths
- Scan S3 buckets
- Work with custom wordlists

---

## Key Features

- **Fast**: Written in Go for high performance
- **Flexible**: Multiple modes (dir, dns, vhost, s3, etc.)
- **Customizable**: Works with any wordlist
- **Efficient**: Minimal resource usage
- **Extensible**: Supports custom HTTP headers and authentication

---

# Installing Gobuster on Linux

## Ubuntu / Debian

```bash
sudo apt update
sudo apt install gobuster -y
```

## Fedora

```bash
sudo dnf install gobuster
```

## Arch Linux

```bash
sudo pacman -S gobuster
```

## From Source (Latest Version)

```bash
git clone https://github.com/OJ/gobuster.git
cd gobuster
go build -o gobuster cmd/gobuster/main.go
sudo mv gobuster /usr/local/bin/
```

---

## Verify Installation

```bash
gobuster --version
```

---

# Basic Terminology

| Term | Meaning |
|---|---|
| Directory Brute-Force | Attempting to find hidden web directories |
| Wordlist | Text file containing directory/filename candidates |
| Status Code | HTTP response code (200, 301, 404, etc.) |
| Virtual Host | DNS subdomain pointing to a server |
| DNS Enumeration | Finding subdomains through DNS lookups |
| Recursive Scan | Scanning subdirectories after finding parent |

---

# Gobuster Modes

| Mode | Purpose | Example |
|---|---|---|
| `dir` | Brute-force directories and files | `gobuster dir -u http://target.com -w wordlist.txt` |
| `dns` | Enumerate DNS subdomains | `gobuster dns -d target.com -w wordlist.txt` |
| `vhost` | Find virtual hosts | `gobuster vhost -u http://target.com -w wordlist.txt` |
| `s3` | Scan AWS S3 buckets | `gobuster s3 -w wordlist.txt` |

---

# Basic Usage

## Directory Brute-Force

```bash
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt
```

Example output:

```text
/admin                (Status: 301)
/api                  (Status: 200)
/images               (Status: 301)
/uploads              (Status: 403)
/login                (Status: 200)
```

---

## DNS Subdomain Enumeration

```bash
gobuster dns -d target.com -w /usr/share/wordlists/subdomains-top1million-5000.txt
```

Example output:

```text
Found: www.target.com
Found: mail.target.com
Found: admin.target.com
Found: api.target.com
```

---

## Virtual Host Enumeration

```bash
gobuster vhost -u http://target.com -w /usr/share/wordlists/subdomains.txt
```

---

# Common Flags Reference

| Flag | Purpose | Example |
|---|---|---|
| `-u` / `--url` | Target URL | `-u http://target.com` |
| `-w` / `--wordlist` | Wordlist file | `-w common.txt` |
| `-o` / `--output` | Output file | `-o results.txt` |
| `-t` / `--threads` | Number of threads | `-t 50` |
| `-s` / `--statuscodes` | Filter status codes | `-s 200,301` |
| `-x` / `--extensions` | File extensions to test | `-x php,html,txt` |
| `-r` / `--followredirect` | Follow HTTP redirects | `-r` |
| `-H` / `--headers` | Custom HTTP headers | `-H "Authorization: Bearer token"` |
| `-a` / `--useragent` | Custom User-Agent | `-a "Mozilla/5.0..."` |
| `-p` / `--proxy` | Use HTTP proxy | `-p http://127.0.0.1:8080` |
| `--delay` | Delay between requests | `--delay 100ms` |
| `-q` / `--quiet` | Suppress banners | `-q` |
| `-v` / `--verbose` | Verbose output | `-v` |

---

# Directory Brute-Force Examples

## Basic Directory Scan

```bash
gobuster dir -u http://192.168.1.100 -w /usr/share/wordlists/dirb/common.txt
```

## With File Extensions

```bash
gobuster dir -u http://target.com -w wordlist.txt -x php,html,txt,js
```

## With Specific Status Codes

```bash
gobuster dir -u http://target.com -w wordlist.txt -s 200,301,302
```

## Multi-threaded Scan

```bash
gobuster dir -u http://target.com -w wordlist.txt -t 100
```

## Following Redirects

```bash
gobuster dir -u http://target.com -w wordlist.txt -r
```

## With Custom Headers (Authentication)

```bash
gobuster dir -u http://target.com -w wordlist.txt -H "Authorization: Bearer your_token"
```

## Output to File

```bash
gobuster dir -u http://target.com -w wordlist.txt -o results.txt
```

---

# DNS Enumeration Examples

## Basic DNS Enumeration

```bash
gobuster dns -d target.com -w /usr/share/wordlists/subdomains-top1million-5000.txt
```

## DNS with Show IPs

```bash
gobuster dns -d target.com -w wordlist.txt --show-ips
```

---

# Advanced Techniques

## Recursive Directory Scanning

After finding directories, scan within them:

```bash
gobuster dir -u http://target.com/admin -w wordlist.txt
```

## Rate Limiting (Stealth)

```bash
gobuster dir -u http://target.com -w wordlist.txt --delay 500ms -t 10
```

## Proxy Through Burp Suite

```bash
gobuster dir -u http://target.com -w wordlist.txt -p http://127.0.0.1:8080
```

## Custom Wordlist

```bash
gobuster dir -u http://target.com -w custom_wordlist.txt
```

---

# Common Wordlists

| Wordlist | Location | Purpose |
|---|---|---|
| `common.txt` | `/usr/share/wordlists/dirb/` | Basic directory enumeration |
| `big.txt` | `/usr/share/wordlists/dirb/` | Larger wordlist |
| `subdomains-top1million-5000.txt` | `/usr/share/wordlists/` | Top 5000 subdomains |
| `rockyou.txt` | `/usr/share/wordlists/` | Password wordlist |
| Custom | User-created | Project-specific paths |

---

# Tips and Best Practices

- ✅ Always get written permission before scanning
- ✅ Use appropriate wordlists for your target
- ✅ Start with common.txt for initial reconnaissance
- ✅ Use multiple threads to speed up scanning (be careful with target stability)
- ✅ Filter results by status codes to reduce noise
- ✅ Combine with other tools for comprehensive enumeration
- ✅ Save results to files for later analysis
- ✅ Respect rate limits and don't hammer servers
- ❌ Don't use excessive threads on unstable targets
- ❌ Don't scan without authorization