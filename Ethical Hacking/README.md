# 🛡️ Ethical Hacking Notes | Red Team |

Cybersecurity beginner | Ethical hacking notes
Documenting my learning journey through hands-on challenges.

This repository documents my structured ethical hacking learning path, aligned with a progressive roadmap from beginner to advanced concepts.

The purpose of this repository is to understand attacker techniques, tools, and methodologies in order to strengthen defensive detection, monitoring, and incident response capabilities.

All content is for educational and defensive security purposes only.

---

## Tools

I document the security tools I use and study in this repository. They span network discovery, web application testing, password cracking, and exploit development:

Nmap — Network discovery and scanning
Gobuster — Directory and file brute-forcing
Burp Suite — Web application security testing
sqlmap — SQL injection testing
Metasploit — Exploitation framework
Hydra — Password cracking
John The Ripper — Password auditing and recovery
hashcat — Hash cracking

---

## CTF Walkthroughs

These are my writeups from TryHackMe Capture The Flag rooms. They walk through real attacks end-to-end — recon, exploitation, privilege escalation — so they double as case studies for each technique covered in the tools section above.

Blue — EternalBlue (MS17-010) exploitation with Metasploit, privilege escalation to SYSTEM, NTLM hash dumping
Kenobi — NFS share mounting, SSH key access, SUID binary PATH hijacking to root
GamingServer — SSH key passphrase cracking, LXD container escape to root
Bolt — Web enumeration, Bolt CMS exploit via Metasploit

---

## Projects

These are the hands-on projects I built as part of my learning — real exercises where I apply every tool above to practice end-to-end recon workflows:

Enumeration sandbox

---

Disclaimer: The techniques, tools, and walkthroughs documented here are for educational, defensive, and authorized testing purposes only. Only apply these methods to systems you own or have explicit written permission to test.