# Networking Fundamentals

## Table of Contents
- [Networks](#networks)
  - [What Is a Network?](#what-is-a-network)
  - [Types of Networks](#types-of-networks)
- [OSI Model](#osi-model)
- [Networking Devices](#networking-devices)
  - [Device Overview](#device-overview)
  - [What Is a Hub?](#what-is-a-hub)
  - [What Is a Layer 2 Switch?](#what-is-a-layer-2-switch)
  - [What Is a Layer 3 Switch?](#what-is-a-layer-3-switch)
  - [What Is a Router?](#what-is-a-router)
- [Basic Networking Protocols](#basic-networking-protocols)
- [DNS](#dns)
  - [What Is DNS?](#what-is-dns)
  - [DNS Records](#dns-records)
  - [DNS Resolution Process](#dns-resolution-process)
- [Tools](#tools)

---

## Description

This document provides an overview of fundamental networking concepts commonly used in IT, networking, and cybersecurity.

| Topic Area | Description |
|-----------|-------------|
| Networks | Core networking concepts and network types |
| OSI Model | The 7-layer networking reference model |
| Devices | Common networking hardware |
| Protocols | Common networking ports and protocols |
| DNS | Name resolution and DNS workflow |

---

## Networks

### What Is a Network?

| Term | Definition |
|-----|------------|
| Network | Two or more connected computers that can share resources such as data, printers, or internet access |

### Types of Networks

| Network Type | Full Name | Description |
|-------------|-----------|-------------|
| LAN | Local Area Network | Small geographic area such as a home or office |
| MAN | Metropolitan Area Network | Covers a city or large campus |
| WAN | Wide Area Network | Covers large geographic areas such as countries or continents |

---

## OSI Model

| Layer | Name | Description |
|------|------|-------------|
| 7 | Application | User-facing applications and services |
| 6 | Presentation | Data formatting, compression, and encryption |
| 5 | Session | Session management and connection control |
| 4 | Transport | End-to-end communication (TCP/UDP) |
| 3 | Network | Logical addressing and routing (IP) |
| 2 | Data Link | MAC addressing and frame delivery |
| 1 | Physical | Physical transmission of bits |

---

## Networking Devices

### Device Overview

| Device | Description |
|------|-------------|
| Hub | Broadcasts incoming data to all ports |
| Layer 2 Switch | Forwards frames using MAC addresses |
| Layer 3 Switch | Performs switching and basic routing. Has the ability for virtual LANS (VLANS) |
| Router | Routes packets between networks using IP addresses |

### What Is a Hub?

| Feature | Details |
|-------|---------|
| OSI Layer | Physical |
| Behavior | Sends data to all connected devices |
| Intelligence | None |

### What Is a Layer 2 Switch?

| Feature | Details |
|-------|---------|
| OSI Layer | Data Link |
| Forwarding | MAC address-based |
| Use Case | Communication within a LAN |

### What Is a Layer 3 Switch?

| Feature | Details |
|-------|---------|
| OSI Layer | Network |
| Capabilities | VLANs, switching, basic routing |
| Use Case | Inter-VLAN routing |

### What Is a Router?

| Feature | Details |
|-------|---------|
| OSI Layer | Network |
| Routing | IP address-based |
| Purpose | Communication between different networks |

---

## Basic Networking Protocols

| Protocol | Port(s) | Transport | Purpose |
|--------|--------|-----------|---------|
| HTTP | 80 | TCP | Web traffic |
| HTTPS | 443 | TCP | Secure web traffic |
| FTP | 20, 21 | TCP | File transfer |
| SMTP | 25, 587 | TCP | Email sending |
| Telnet | 23 | TCP | Remote terminal access |
| SSH | 22 | TCP | Secure remote access |
| DNS | 53 | TCP/UDP | Name resolution |
| DHCP | 67/68 | UDP | Dynamic IP assignment |
| SNMP | 161/162 | UDP | Network monitoring |

---

## DNS

### What Is DNS?

| Item | Description |
|----|-------------|
| DNS | Domain Name System |
| Function | Translates domain names into IP addresses |
| Example | `google.com` → `8.8.8.8` |

### DNS Records

| Record Type | Name | Purpose |
|------------|------|---------|
| NS | Name Server | Specifies authoritative DNS servers |
| SOA | Start of Authority | Domain administrative information |
| A | Address | Maps hostname to IPv4 address |
| MX | Mail Exchanger | Specifies mail servers |
| PTR | Pointer | Reverse DNS lookups |

### DNS Resolution Process

| Step | Description |
|-----|-------------|
| 1 | Local DNS cache is checked |
| 2 | HOSTS file is checked |
| 3 | Configured DNS resolver is queried |
| 4 | Query is forwarded through DNS hierarchy |
| 5 | Root and TLD servers are queried |
| 6 | Authoritative DNS server responds |

---

## Tools

Common networking and security analysis tools:

### Nmap
Nmap is used for network discovery and security auditing. It identifies live hosts, open ports, running services, and operating systems.

### Wireshark
Wireshark captures and analyzes network traffic at the packet level, helping professionals troubleshoot issues and detect suspicious activity.
