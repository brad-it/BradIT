# 🛡️ Ethical Hacking Notes | Red Team |

Cybersecurity beginner | Ethical hacking notes
Documenting my learning journey through hands-on challenges.

This repository documents my structured ethical hacking learning path, aligned with a progressive roadmap from beginner to advanced concepts.

The purpose of this repository is to understand **attacker techniques, tools, and methodologies** in order to strengthen **defensive detection, monitoring, and incident response** capabilities.

All content is for **educational and defensive security purposes only**.

---

## 🧭 Roadmap Stages

### 🟢 Beginner
Focuses on foundational knowledge required to understand how systems communicate and how attackers interact with them.

- Networking Basics
- Linux Fundamentals

---

### 🟡 Intermediate
Introduces common attacker techniques used to identify and exploit weaknesses in systems and applications.

- Web Application Security
- Enumeration & Scanning

---

### 🔴 Advanced
Covers post-exploitation activity and attacker tradecraft, with an emphasis on how these actions can be detected and mitigated.

- Exploitation & Privilege Escalation
- Red Teaming

---

## 📁 Repository Structure

| Folder | Contents |
|--------|----------|
| `Ethical Hacking Tools/` | Notes on common offensive security tools (Nmap, Burp Suite, Gobuster, hashcat, Hydra, John the Ripper, Metasploit, sqlmap). |
| `CTF Writeups/TryHackMe/` | Writeups from Capture The Flag challenges (TryHackMe, etc.). |

### 🛠️ Ethical Hacking Tools

| Tool | Category |
|------|----------|
| [Nmap](Ethical%20Hacking%20Tools/nmap.md) | Network discovery & scanning |
| [Gobuster](Ethical%20Hacking%20Tools/Gobuster.md) | Directory & file brute-forcing |
| [Burp Suite](Ethical%20Hacking%20Tools/burpsuite.md) | Web application security testing |
| [sqlmap](Ethical%20Hacking%20Tools/sqlmap.md) | SQL injection testing |
| [Metasploit](Ethical%20Hacking%20Tools/metasploit.md) | Exploitation framework |
| [Hydra](Ethical%20Hacking%20Tools/Hydra.md) | Password cracking |
| [John The Ripper](Ethical%20Hacking%20Tools/John%20The%20Ripper.md) | Password auditing & recovery |
| [hashcat](Ethical%20Hacking%20Tools/hashcat.md) | Hash cracking |

### 🚩 CTF Writeups — TryHackMe

| Room | Difficulty | Summary |
|------|-----------|---------|
| [Blue](CTF%20Writeups/TryHackMe/Blue%20-%20Walkthrough/README.MD) | Easy | EternalBlue (MS17-010) exploitation with Metasploit, privilege escalation to SYSTEM, NTLM hash dumping |
| [Kenobi](CTF%20Writeups/TryHackMe/Kenobi%20-%20Walkthrough/README.md) | Easy | NFS share mounting, SSH key access, SUID binary PATH hijacking to root |
| [GamingServer](CTF%20Writeups/TryHackMe/GamingServer%20-%20Walkthrough/README.md) | Medium | SSH key passphrase cracking, LXD container escape to root |
| [Bolt](CTF%20Writeups/TryHackMe/Bolt%20-%20Walkthrough/README.md) | Medium | Web enumeration, Bolt CMS exploit via Metasploit |

---

> ⚠️ **Disclaimer:** The techniques, tools, and walkthroughs documented here are for **educational, defensive, and authorized testing purposes only**. Only apply these methods to systems you own or have explicit written permission to test.