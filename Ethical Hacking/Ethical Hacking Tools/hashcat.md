#hashcat

# Hashcat Overview

## What is Hashcat?

Hashcat is the world's fastest and most advanced password recovery and cracking engine. It supports GPU acceleration for rapid hash cracking across numerous algorithms.

It is commonly used by:

- Penetration testers
- Security researchers
- Incident response teams
- Password auditors
- Digital forensics analysts

Hashcat can:

- Crack password hashes using GPU acceleration
- Support 300+ hash types and algorithms
- Perform dictionary, brute-force, and hybrid attacks
- Use wordlists, rules, and masks
- Crack encrypted archives and disks
- Work with hybrid attacks and custom rules

---

## Key Features

- **Ultra-Fast**: GPU-accelerated cracking with massive performance gains
- **Comprehensive**: Supports more hash types than any other tool
- **Flexible**: Dictionary, brute-force, hybrid, and mask attacks
- **Powerful**: Can handle massive wordlists and custom rules
- **Scalable**: Distribute across multiple GPUs or machines

---

# Installing Hashcat on Linux

## Ubuntu / Debian

```bash
sudo apt update
sudo apt install hashcat -y
```

## Fedora

```bash
sudo dnf install hashcat
```

## Arch Linux

```bash
sudo pacman -S hashcat
```

## From Source (Latest Version)

```bash
git clone https://github.com/hashcat/hashcat.git
cd hashcat
make
sudo make install
```

---

## Verify Installation

```bash
hashcat --version
```

---

# Basic Terminology

| Term | Meaning |
|---|---|
| Hash | One-way encrypted representation of a password |
| Crack | Recover original password from hash |
| Dictionary Attack | Using wordlist of common passwords |
| Brute Force | Trying all possible character combinations |
| Mask Attack | Using patterns for password generation |
| Rule | Custom transformation applied to wordlist |
| Wordlist | Text file containing candidate passwords |
| GPU | Graphics Processing Unit for acceleration |

---

# Hash Types Reference

| Hash Type | Example | Type ID |
|---|---|---|
| MD5 | `5f4dcc3b5aa765d61d8327deb882cf99` | 0 |
| SHA-1 | `cbfdac6008f9cab4083784cbd1874f76618d2a97` | 100 |
| SHA-256 | `2c26b46911185131006ba471a8e704bb...` | 1400 |
| bcrypt | `$2a$10$...` | 3200 |
| NTLM | `31d6cfe0d16ae931b73c59d7e0c089c0` | 1000 |
| MySQL | `*81F5E21E35407D884A6CD3A731AEBFB6AF209E1B` | 300 |
| WPA/WPA2 | `.cap` or `.hccapx` file | 2500 |

---

# Basic Usage

## Dictionary Attack

```bash
hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt
```

Example output:

```text
5f4dcc3b5aa765d61d8327deb882cf99:password

Session..........: hashcat
Status...........: Cracked
Hash.Type........: MD5
Hash.Target......: 5f4dcc3b5aa765d61d8327deb882cf99
Time.Started.....: Mon Nov 04 22:00:00 2026 (0 secs)
Time.Estimated...: Mon Nov 04 22:00:00 2026 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........: 1234.5 MH/s (GPU) (Accel: 128 x 4 x 1)
Recovered........: 1/1 (100.00%) Digests
Progress.........: 500000/14344391 (3.49%)
Rejected.........: 0/500000 (0.00%)
Restore.Point....: 0/14344391 (0.00%)
```

---

## Brute Force Attack

```bash
hashcat -m 0 hashes.txt -a 3 ?a?a?a?a
```

---

## Mask Attack (Realistic Patterns)

```bash
hashcat -m 0 hashes.txt -a 3 ?u?l?l?l?d?d
```

---

## Show Cracked Passwords

```bash
hashcat -m 0 hashes.txt --show
```

---

# Common Flags Reference

| Flag | Purpose | Example |
|---|---|---|
| `-m` | Hash type ID | `-m 0` (MD5) |
| `-a` | Attack mode | `-a 0` (dictionary), `-a 3` (brute force) |
| `-w` | Workload profile | `-w 3` (high) |
| `-O` | Optimized kernel | `-O` |
| `--gpu-devices` | Select GPU | `--gpu-devices 1,2` |
| `-o` | Output file | `-o cracked.txt` |
| `--show` | Show cracked hashes | `--show` |
| `--potfile` | Potfile location | `--potfile=hashcat.pot` |
| `-r` | Apply rules | `-r rules/best64.rules` |
| `--increment` | Increment mask length | `--increment` |
| `-j` | Prepend rule | `-j "l"` |
| `-k` | Append rule | `-k "u"` |
| `--session` | Session name | `--session=mysession` |
| `--restore` | Restore session | `--restore` |

---

# Attack Modes

| Mode | Type | Example |
|---|---|---|
| 0 | Dictionary | `hashcat -m 0 hashes.txt wordlist.txt` |
| 1 | Combination | `hashcat -m 0 hashes.txt list1.txt list2.txt` |
| 3 | Brute Force | `hashcat -m 0 hashes.txt -a 3 ?a?a?a?a` |
| 6 | Hybrid 1 | `hashcat -m 0 hashes.txt -a 6 wordlist.txt ?d?d?d` |
| 7 | Hybrid 2 | `hashcat -m 0 hashes.txt -a 7 ?d?d?d wordlist.txt` |
| 9 | Association | `hashcat -m 0 hashes.txt -a 9` |

---

# Mask Attack Patterns

| Pattern | Meaning | Example |
|---|---|---|
| `?l` | Lowercase a-z | `?l?l?l` (aaa-zzz) |
| `?u` | Uppercase A-Z | `?u?u?u` (AAA-ZZZ) |
| `?d` | Digits 0-9 | `?d?d?d` (000-999) |
| `?a` | All alphanumeric | `?a?a?a` (all combo) |
| `?s` | Special chars | `?s?s` (!@#$...) |
| `?b` | Binary | `?b?b` (0x00-0xFF) |

---

# Advanced Usage Examples

## GPU-Accelerated Attack

```bash
hashcat -m 1400 hashes.txt wordlist.txt -w 4 -O
```

## Apply Rules to Wordlist

```bash
hashcat -m 0 hashes.txt wordlist.txt -r /usr/share/hashcat/rules/best64.rules
```

## Hybrid Attack (Wordlist + Mask)

```bash
hashcat -m 0 hashes.txt -a 6 wordlist.txt ?d?d?d
```

## Increment Mask Length

```bash
hashcat -m 0 hashes.txt -a 3 -i ?a?a?a?a?a
```

## Multiple GPUs

```bash
hashcat -m 0 hashes.txt wordlist.txt --gpu-devices 1,2,3,4
```

## Create and Restore Session

```bash
hashcat -m 0 hashes.txt wordlist.txt --session=mysession
hashcat --restore --session=mysession
```

## Optimize for Speed

```bash
hashcat -m 0 hashes.txt wordlist.txt -O -w 4
```

## Use Multiple Wordlists

```bash
hashcat -m 0 hashes.txt wordlist1.txt wordlist2.txt wordlist3.txt
```

---

# Tips and Best Practices

- ✅ Use GPU acceleration for massive speed gains
- ✅ Start with common wordlists (rockyou.txt)
- ✅ Apply rules to increase coverage
- ✅ Use mask attacks for realistic patterns
- ✅ Monitor GPU usage and temperatures
- ✅ Save sessions for long-running attacks
- ✅ Use `--show` to verify cracked passwords
- ✅ Keep potfile updated
- ❌ Don't crack hashes without authorization
- ❌ Don't exceed GPU temperature limits
- ❌ Don't leave running attacks unattended on shared systems