# Week 08: Resilience and Recovery – Implementing Backups

## Overview

This lab focused on implementing and validating data backup and recovery
strategies on a Windows Server 2019 system. Working as a simulated security
professional, the objective was to configure backup operations using Windows
Server Backup, perform file restoration from a backup set, and use Volume
Shadow Copy Service (VSS) to recover previous versions of modified files.

This exercise reinforced Security+ concepts related to data resilience,
recovery validation, and the practical distinctions between full backups and
point-in-time file versioning systems.

## Environment

- Windows Server 2019 (PC10 virtual machine)
- Windows Server Backup
- Volume Shadow Copy Service (VSS)
- Windows Command Prompt (Administrator)
- diskpart utility
- File Explorer
- Disk Management

## CompTIA Security+ Objectives Covered

- 3.4 – Explain the importance of resilience and recovery in security
  architecture

---

## Methodology

### Part 1 – Preparing Backup Storage and Source Files

Configured a dedicated backup volume and prepared sample files for backup and
recovery operations.

- Used the `diskpart` utility to bring Disk 1 online, clear the read-only
  attribute, and create a primary partition formatted with NTFS
- Assigned the volume label `Backup01` and mounted it as drive `F:`
- Created three sample text files (`document01.txt`, `document02.txt`,
  `document03.txt`) in `C:\Users\Jaime\Documents` using the `copy` command
- Created an additional sample file (`document04.txt`) in `C:\Users\Public\`
  for use in the Volume Shadow Copy Service exercise
- Confirmed that backup operations can protect files of any type, with
  available backup media capacity being the primary limiting factor

### Part 2 – Performing a One-Time Backup with Windows Server Backup

Configured and executed a manual backup of the `C:\Users` directory to the
prepared backup volume.

- Opened Windows Server Backup and selected Local Backup
- Initiated a one-time backup via the Backup Once Wizard
- Configured a custom backup targeting `C:\Users`
- Selected `Backup01 (F:)` as the local drive destination
- Confirmed the backup completed successfully and the backup set was
  available for restoration
- Noted that scheduled backups would be the standard real-world approach,
  configurable through the Backup Schedule option

### Part 3 – Deleting and Restoring a File from Backup

Validated the backup by deleting a file and recovering it from the backup
set.

- Deleted `document01.txt` from `C:\Users\Jaime\Documents` and emptied the
  Recycle Bin to ensure full removal from the system
- Verified the file no longer existed on the system, confirming that
  restoration could only occur from backup
- Used the Windows Server Backup Recovery Wizard to restore the file:
  - Selected the local server (PC10) as the recovery source
  - Selected the most recent backup set
  - Chose Files and folders recovery type
  - Navigated to `Local Disk (C:) > Users > jaime > Documents` and selected
    `document01.txt`
  - Restored the file to its original location
- Verified the restoration by opening the recovered file in File Explorer

### Part 4 – Enabling Volume Shadow Copy Service (VSS)

Enabled and forced a Volume Shadow Copy operation on the system drive to
capture a point-in-time snapshot.

- Opened Disk Management and accessed the properties of the `C:` drive
- Enabled Volume Shadow Copy Service via the Shadow Copies tab
- Confirmed an initial shadow copy was created automatically upon enablement
- Forced an additional shadow copy operation via Command Prompt using the
  Windows Management Instrumentation Command-line utility: wmic shadowcopy call create Volume=c:\
- Verified the command returned `Method execution successful`, confirming
  the shadow copy was created

### Part 5 – Modifying and Restoring a File via VSS

Modified a file and used VSS to restore its previous unmodified version.

- Opened `document04.txt` in Notepad from `C:\Users\Public\`
- Added the word `changed` as a new first line and saved the modification
- Verified the file now contained the modified content
- Right-clicked the modified file and selected Restore previous versions
- Selected the most recent VSS entry from the File versions list
- Restored the previous version, overwriting the modified file
- Confirmed restoration by reopening the file and verifying the `changed`
  line was no longer present

---

## Key Concepts Demonstrated

### Windows Server Backup

Windows Server Backup is the native Windows backup utility for performing
one-time or scheduled backup operations of files, folders, volumes, system
state, or entire systems. Backup data is stored on a designated destination —
local drive, network share, or removable media. Recovery operations are
performed through the Recovery Wizard, which supports file/folder-level
restoration, volume restoration, and system state recovery. Windows Server
Backup is the primary method for recovering files that have been permanently
deleted from a system.

### Volume Shadow Copy Service (VSS)

VSS is a Windows feature that captures point-in-time snapshots of files on a
volume, enabling restoration of previous versions of modified files. VSS
operates at the file level and is not a substitute for traditional backup.
If a file is permanently deleted from a volume, VSS cannot recover it — VSS
only retains previous versions of files that still exist on the system. VSS
is most useful for recovering from accidental edits, file corruption, or
unwanted modifications.

### Backup vs. VSS — Key Distinction

The two technologies serve different purposes and the distinction is critical:

- **Backup** — recovers files that have been deleted, lost, or destroyed.
  Stored on separate media. Required for full disaster recovery.
- **VSS** — recovers previous versions of files that still exist on the
  system. Stored on the same volume. Cannot recover deleted files.

A complete data protection strategy uses both: VSS for fast recovery from
common modification errors, and backups for comprehensive disaster recovery
and protection against catastrophic data loss.

### Recycle Bin as a Third Recovery Option

Beyond backups and VSS, the Recycle Bin provides a recovery option for
files that have been deleted but not yet permanently removed from the
system. Emptying the Recycle Bin eliminates this recovery option, requiring
either VSS (if the file still exists in a previous shadow copy) or backup
restoration.

### Backup Schedule Considerations

The most important consideration when designing a backup schedule is the
acceptable amount of data loss — defined formally as the Recovery Point
Objective (RPO). This determines backup frequency. Highly transactional
systems may require continuous or near-real-time backups, while less
critical systems may tolerate daily or weekly intervals. Other factors
including data volume, transfer speed, and storage location influence
implementation details but are secondary to the fundamental question of
acceptable data loss.

### Backup Best Practices

Storing essential backups offsite is a foundational best practice. Onsite-only
backups remain vulnerable to the same physical threats that affect the
primary systems — fire, flood, theft, or ransomware that propagates through
the network. Offsite backups provide protection against catastrophic site
loss and support broader disaster recovery and business continuity objectives.

---

## Skills Demonstrated

- Backup volume preparation using diskpart (partition creation, NTFS
  formatting, drive letter assignment)
- Windows Server Backup configuration and one-time backup execution
- File restoration from backup using the Recovery Wizard
- Volume Shadow Copy Service enablement and management
- Forcing VSS snapshot creation via WMIC commands
- Previous version restoration through the Windows Properties interface
- Understanding the operational distinctions between full backup recovery
  and VSS point-in-time restoration
- Application of recovery validation practices to confirm backup integrity

## Tools Used

- Windows Server Backup — primary backup and recovery utility
- Volume Shadow Copy Service (VSS) — point-in-time file versioning
- `diskpart` — disk partitioning and volume management
- `wmic shadowcopy call create` — forced VSS snapshot creation
- Disk Management — VSS configuration interface
- File Explorer — manual file operations and version restoration
- Windows Command Prompt (Administrator)

---

## Lab Result

**Score: 13/13 — Passed**  
Duration: 41 minutes, 33 seconds

---

## Supporting Documentation

- [Lab – Completion: Implementing Backups](https://github.com/user-attachments/files/28739091/Lab07.-.Implement.Backups.pdf)
- [Security+ V7 Badge: Resiliency Raider](https://github.com/user-attachments/files/28754315/ResiliencyRaiderCertification.pdf)
- [Security+ V7 Badge: OnDemand Security+ Part 1](https://github.com/user-attachments/files/28754374/OnDemandSecurity%2BPart1Certification.pdf)

