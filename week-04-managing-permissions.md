# Week 04: Identity & Access Management – Managing Permissions

## Overview

This lab demonstrates hands-on implementation of access control across both Linux and Windows enterprise environments. Completed as part of the CompTIA Security+ (SY0-701) curriculum, the scenario placed me in the role of a security team member at a simulated organization responsible for auditing and enforcing file and share permissions across multiple systems.

The objective was to configure, modify, and evaluate permissions using industry-standard tools, applying least privilege principles across platforms.

## Environment

- Kali Linux (KALI virtual machine)
- Windows Server 2019 (PC10 virtual machine)
- Linux terminal (bash)
- Windows PowerShell (Administrator)
- Windows File Explorer

## Methodology

### Part 1 – Linux File Permissions

Audited and reconfigured file permissions on a Kali Linux system using both symbolic and octal notation via `chmod`.

- Used `ls -l` to audit existing permissions in long-list format
- Applied symbolic notation (`chmod u+rwx,g+x-rw,o-rwx`) for granular, targeted changes
- Applied octal notation to set complete permission states in a single command
- Configured `demofile.sh` to enforce full owner access, execute-only for group, and no access for others — octal value `710`

### Part 2 – Windows NTFS File Permissions

Managed NTFS permissions on Windows Server 2019 using `icacls` in PowerShell as Administrator.

- Audited current permission assignments on `comptia-logo.jpg`
- Denied read access to a user using `/deny`, granted full control using `/grant`
- Removed user-specific entries using `/remove:g`
- Confirmed that inherited group permissions remain active after user-specific entries are removed

### Part 3 – Windows Effective Permissions

Used the Windows Advanced Security Settings interface to evaluate accumulated effective permissions for a user account.

- Accessed the Effective Access tab to evaluate permissions based on direct assignments and group membership inheritance
- Confirmed the tool functions as a read-only diagnostic utility — useful for troubleshooting without modifying live permissions

### Part 4 – Windows Share Permissions

Created and managed SMB network share permissions via PowerShell, demonstrating how share-level controls layer on top of NTFS permissions.

- Created a share using `New-SmbShare`, audited with `Get-SmbShare` and `Get-SmbShareAccess`
- Granted and revoked Change-level share access using `Grant-SmbShareAccess` and `Revoke-SmbShareAccess`
- Confirmed that the most restrictive permission between NTFS and share levels determines actual network access

## Key Findings

**Linux octal notation** is the most efficient method for setting complete permission states. Symbolic notation is better suited for targeted incremental changes.

**NTFS and share permissions are independent layers.** Removing a user's directly assigned NTFS permissions does not eliminate access inherited through group membership. Both layers must be evaluated together when auditing network resource access.

**Effective Access is diagnostic only.** It calculates accumulated permissions without making any modifications — a key tool for troubleshooting access issues safely.

**The most restrictive permission wins.** When accessing a resource over a network, the lesser of NTFS and share permissions determines actual access. A user with Modify-level NTFS access requires at minimum Change share permission to alter file contents.

## Skills Demonstrated

- Linux file permission auditing and enforcement using `chmod` (symbolic and octal)
- Windows NTFS permission management using `icacls`
- Effective permissions evaluation using Windows Advanced Security Settings
- SMB share creation and access management via PowerShell cmdlets
- Cross-platform access control configuration across Linux and Windows Server
- Application of least privilege principles in multi-environment access management
- Permission inheritance analysis and accumulated access evaluation

## Tools Used

- `chmod`, `ls -l` — Linux permission management and auditing
- `icacls` — Windows NTFS permission management
- `New-SmbShare`, `Get-SmbShare`, `Get-SmbShareAccess`, `Grant-SmbShareAccess`, `Revoke-SmbShareAccess` — Windows share management
- Windows Advanced Security Settings — Effective permissions evaluation
- Kali Linux terminal, Windows PowerShell (Administrator)

## Lab Result

**Score: 11/11 — Passed**
Duration: 59 minutes, 31 seconds

## Supporting Documentation

- [Lab – Completion: Managing Permissions](https://github.com/user-attachments/files/26490118/Lab04.-.Managing.Permissions.pdf)
- [Access Architect – Security+ V7 Certificate of Completion](https://github.com/user-attachments/files/26490120/Week04Certification.pdf)

