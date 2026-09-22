# 🛡️ Advanced Network Security Assessment & Traffic Investigation (Week 3)

**Author:** Mohammed Avesh Shaikh  
**Email:** aveshshaikh05042005@gmail.com  
**Track:** Cybersecurity Foundation / SOC Analyst  
**Organization:** DG Interns Hub  
**Date:** September 2026  
**Lab Environment:** Isolated VirtualBox Network (Subnet: `10.0.2.0/24`)

---

## 🎯 Project Objective
This repository documents an end-to-end practical security assessment of an internal network segment. The audit covers host discovery, deep service enumeration with Nmap, wire-level packet forensics using Wireshark, vulnerability classification, and infrastructure hardening using host-based firewalls.

---

## 🏗️ Lab Environment & Architecture
- **Auditor Workstation:** Kali Linux 2026.x (`10.0.2.15`)
- **Gateway & DNS:** VirtualBox Gateway (`10.0.2.2`) / Resolver (`10.0.2.3`)
- **Target Systems:** Authorized scanning target `scanme.nmap.org` (`45.33.32.156`) & local lab nodes
- **Core Tool Stack:** Nmap 7.94, Wireshark 4.6.6, UFW Firewall, systemd

---

## 📋 Tasks Completed (Tasks 7 to 14)

### 1. Network Discovery & Scanning (Tasks 7 & 8)
- Discovered active hosts on the subnet via ping sweeps (`nmap -sn 10.0.2.0/24`).
- Conducted deep service fingerprinting (`-sV`), OS detection (`-O`), and TCP SYN scanning (`-sS`).
- Demystified port states: Open (active listening daemon), Closed (immediate `RST/ACK` rejection), and Filtered (firewall packet drop).

### 2. Wireshark Network Traffic Forensics (Task 9)
- Captured live traffic on interface `eth0` and applied protocol display filters:
  - **ICMP:** Verified round-trip latency and echo reachability with `8.8.8.8` (Packets 85–92).
  - **DNS (UDP 53):** Dissected domain resolutions for `google.com` and `neverssl.com` (Packets 95–102).
  - **ARP:** Inspected L2 address resolution probes and announcements for MAC-to-IP mapping.
  - **HTTP (TCP 80):** Captured unencrypted `HEAD / HTTP/1.1` and `200 OK` cleartext web traffic.

### 3. Packet-Level Nmap Investigation (Task 10)
- Analyzed the wire-level mechanics of Nmap's Half-Open SYN Stealth Scan (`-sS`):
  - **Open Ports (22, 80):** Kali sends `[SYN]` ➔ Target replies `[SYN, ACK]` ➔ Kali sends `[RST]` to tear down connection before application logging.
  - **Filtered/Closed Ports (9999):** Kali sends `[SYN]` ➔ Target rejects with `[RST, ACK]`.
  - Correlated terminal outputs with Wireshark packet sequence numbers 145–156.

### 4. Advanced Traffic Investigation (Task 11)
- Profiled top communicating hosts and investigated 5 anomalous events (port scanning sweeps, unencrypted credential flows, excessive ARP broadcast chatter, and rapid resets).

### 5. Vulnerability Assessment (Task 12)
- Identified 5 critical security findings: Plaintext Telnet daemon (Critical), Insecure FTP (High), Exposed MySQL database (High), Cleartext HTTP (Medium), and Permissive Firewall Policy (High).

### 6. Security Hardening & Verification (Task 13)
- Decommissioned legacy Telnet and FTP daemons.
- Configured UFW host firewall with a Default-Deny ingress policy, permitting only Port 22 (SSH).
- Bound MySQL service strictly to `127.0.0.1` (localhost loopback).

| Security Issue | Before Hardening | Action Taken | After Hardening | Attack Surface Impact |
| :--- | :--- | :--- | :--- | :--- |
| **Plaintext Telnet (Port 23)** | Open | Service stopped & purged | Closed / Filtered | Eliminates credential theft |
| **Insecure FTP (Port 21)** | Open | vsftpd package removed | Closed / Filtered | Prevents unauthorized file transfers |
| **Exposed MySQL (Port 3306)** | Open to Subnet | Bound to `127.0.0.1` | Loopback Only | Protects database from remote access |
| **Host Firewall Policy** | Inactive | Enabled UFW Default Deny | Only Port 22 Open | **80% Attack Surface Reduction** |

### 7. Strategic Security Recommendations (Task 14)
1. Mandate end-to-end encryption (enforce SSH, SFTP, and TLS).
2. Implement Principle of Least Privilege via network and host-based firewalls.
3. Deploy Network Intrusion Detection Systems (NIDS) like Snort/Suricata.
4. Restrict sensitive databases to private subnets / local loopback.
5. Establish continuous automated vulnerability scanning and patch management.

---

## 📁 Repository Deliverables
- `Week-3-Report.pdf`: 15+ Page Comprehensive Technical Report
- `Week-3-Presentation.pptx`: 11-Slide Executive Presentation Deck
- Screenshots folder containing live terminal and Wireshark evidence
