# Week 02: Network Reconnaissance & Open Service Port Analysis

## Overview

This lab focused on identifying exposed services and analyzing attack surface across segmented network zones using Nmap within a Kali Linux environment.

The objective was to perform reconnaissance, enumerate open ports, identify running services and operating systems, and evaluate associated security risks.

This exercise reinforced concepts related to attack surface analysis, threat vectors, segmentation weaknesses, and mitigation strategy development.

## Environment

- Kali Linux
- Nmap
- Multiple network segments (External, Guest, Internal)
- Windows and firewall targets

## Methodology

Performed SYN-based port scans and service enumeration using:

Techniques applied:

- Stealth SYN scanning (-sS)
- Service version detection (-sV)
- OS fingerprinting (-O)
- Output logging for documentation (-oN)
- Network interface switching (eth0 / VLAN environments)
- DHCP renewal and IP verification

Scanning was conducted across three zones:

1. Internet-facing firewall interface
2. Guest network
3. Internal client VLAN

## Key Findings

### External Firewall Exposure

Open ports discovered:
- FTP (21)
- SSH (22)
- SMTP (25)
- DNS (53)
- HTTP (80)
- HTTPS (443)
- RDP (3389)

Security Implication:
Internet-exposed remote access services significantly increase brute-force and credential attack risk.

---

### Guest Network Exposure

Identified accessible firewall management interface (OPNsense) from guest network.

Security Implication:
Guest network users should not have management-level visibility into firewall services. This represents a segmentation weakness and privilege boundary concern.

---

### Internal Server Exposure

Multiple services discovered on internal server including:

- FTP
- SMTP
- HTTP
- MSRPC
- NTP
- IMAP
- Microsoft-DS
- NFS
- MySQL
- RDP

Security Implication:
Wide internal attack surface increases lateral movement risk in the event of compromise.

Operating system identified as Windows Server 2016, highlighting lifecycle and legacy system considerations.

## Risk & Mitigation Considerations

Identified that:

- An attack surface represents the collection of exposed vulnerabilities.
- A threat vector represents a pathway that supports intrusion attempts.
- Mitigation options include:
  - Closing unnecessary exposed ports
  - Enforcing encrypted services
  - Applying segmentation controls
  - Reducing externally exposed services

## Skills Demonstrated

- Network reconnaissance
- Port scanning and service enumeration
- OS fingerprinting
- Network segmentation analysis
- Risk evaluation of exposed services
- Identification of lateral movement pathways
- Defensive mitigation reasoning

## Supporting Documentation

[Lab Completion Verification](https://github.com/user-attachments/files/25462690/Lab02.-.Assisted.Lab.Finding.Open.Service.Ports.pdf)


[Threat Tracker – Security+ V7 Certificate (Issued Feb 21, 2026)](https://github.com/user-attachments/files/25462693/Week02Certification.pdf)
