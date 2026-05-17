# Week 05: Secure Enterprise Network Architecture – Setting Up Remote Access

## Overview

This lab focused on configuring and establishing secure remote access connections
across both Windows and Linux environments. Working as a simulated security
professional, the objective was to implement and verify remote access tools used
in enterprise infrastructure management — a core skill for SOC analysts and
systems administrators.

This exercise reinforced Security+ concepts related to secure communications,
remote access protocols, and enterprise network security principles.

## Environment

- Windows Server 2019 (DC10 virtual machine)
- Windows Server 2019 (PC10 virtual machine)
- Kali Linux (KALI virtual machine)
- PuTTY SSH client (GUI)
- Windows Command Prompt (CLI)
- Microsoft Remote Desktop Protocol (RDP)
- OpenSSH

## CompTIA Security+ Objectives Covered

- 3.2 – Given a scenario, apply security principles to secure enterprise
  infrastructure
- 5.6 – Given a scenario, implement security awareness practices

---

## Methodology

### Part 1 – Windows Remote Desktop Configuration

Configured and established a Remote Desktop Protocol (RDP) connection between
two Windows Server 2019 systems.

- Enabled remote desktop connections on PC10 via System Properties
- Added a specific user account (Rene) to the authorized Remote Desktop Users
  list, demonstrating least privilege — only explicitly authorized users can
  connect remotely
- Established an RDP session from DC10 to PC10 using alternate credentials
- Verified remote system identity using `ipconfig` and system information
- Confirmed that RDP is not enabled by default on standard Windows systems —
  it requires explicit configuration and user authorization

### Part 2 – SSH Verification on Kali Linux

Confirmed the installation and configuration of the OpenSSH server on a Kali
Linux system.

- Verified OpenSSH server installation using `apt list openssh-server`
- Inspected the SSH configuration file at `/etc/ssh/sshd_config` using
  `cat` and `grep` to confirm password authentication settings
- Confirmed the SSH service was active and listening on port 22 using
  `systemctl status ssh`
- Identified the system's IPv4 address using `ip a s eth0` for use in
  subsequent connection tasks

### Part 3 – SSH Connection via GUI (PuTTY)

Established a cross-platform SSH session from Windows to Kali Linux using
PuTTY, a free open-source terminal emulator and network file transfer tool.

- Configured PuTTY with the target IP address and port 22
- Accepted the host key security alert, establishing trust with the remote
  server
- Authenticated using root credentials via password authentication
- Executed commands on the remote Kali system including `hostname` and
  `mkdir remote` to verify full command execution capability
- Terminated the session cleanly using `exit`

### Part 4 – SSH Connection via CLI

Established the same cross-platform SSH connection using the native Windows
Command Prompt SSH client, demonstrating that SSH is not limited to Linux
systems.

- Connected using standard SSH CLI syntax: `ssh root@10.1.16.66`
- Accepted the host key fingerprint on first connection
- Authenticated via password
- Suppressed the default SSH login banner by creating a `~/.hushlogin` file
  using the `touch` command, then verified the suppression on reconnection
- Terminated sessions using `exit`

---

## Key Concepts Demonstrated

### Remote Desktop Protocol (RDP)

RDP provides full graphical desktop access to a remote Windows system. It
transmits screen, audio, keyboard, and mouse data across the connection.
RDP is encrypted by default but must be explicitly enabled and configured with
authorized users — it is not active on standard Windows systems by default.

### Secure Shell (SSH)

SSH provides encrypted remote command-line access to a system. It is the
standard protocol for secure remote administration of Linux and Unix systems,
and is also available natively on modern Windows systems. SSH is not limited
to Linux.

Default authentication method is password-based, though public key
authentication is the more secure alternative. The SSH server listens on
port 22 by default.

### PuTTY

PuTTY is a free, open-source tool that functions as a terminal emulator, serial
console, and network file transfer application. It is commonly used on Windows
systems to initiate SSH and Telnet connections to remote devices including
servers, routers, and switches.

### Host Key Verification

On first connection, SSH presents the server's host key for the client to
verify. Accepting this key establishes trust with the remote server. If the
host key changes unexpectedly on a subsequent connection, it may indicate a
spoofing or man-in-the-middle attack.

### SSH Login Banner Suppression

Creating a `~/.hushlogin` file suppresses the default welcome message
displayed on SSH login. This is a configuration technique used to streamline
automated or frequent administrative connections.

---

## Skills Demonstrated

- Windows Remote Desktop configuration and user authorization
- Cross-platform SSH session establishment using both GUI and CLI tools
- Linux SSH service verification using `systemctl`, `apt`, and `grep`
- SSH CLI syntax and connection workflow
- Host key acceptance and trust establishment
- Remote command execution across operating systems
- Application of least privilege in remote access configuration
- Network interface identification using `ip a s`

## Tools Used

- Microsoft Remote Desktop Protocol (RDP)
- OpenSSH — Linux SSH server
- PuTTY — GUI SSH client for Windows
- Windows Command Prompt — CLI SSH client
- `systemctl` — Linux service management
- `apt` — Linux package verification
- `ip a s` — Linux network interface inspection
- `cat` / `grep` — Linux configuration file inspection

---

## Lab Result

**Score: 9/9 — Passed**
Duration: 45 minutes, 23 seconds

---

## Supporting Documentation

- [Lab 09 – Assisted: Setting Up Remote Access](https://github.com/user-attachments/files/27902571/Lab05.-.Setting.Up.Remote.Access.pdf)
- [Infrastructure Innovator – Security+ V7 Certificate of Completion](https://github.com/user-attachments/files/27902575/Week05Certification.pdf)
