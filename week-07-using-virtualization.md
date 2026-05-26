# Week 07: Cloud Infrastructure & Embedded Systems – Using Virtualization

## Overview

This lab focused on deploying and configuring a Hyper-V virtualization
environment on Windows Server 2019. Working as a security team member at
Structureality Inc., the objective was to install Hyper-V, create and configure
a virtual machine, install an operating system, and implement virtual networking
using private and external virtual switches — then evaluate the security
implications of each switch type.

This exercise reinforced Security+ concepts related to virtual networking
security, network isolation, and the architecture security implications of
different virtualization configurations.

## Environment

- Windows Server 2019 (MS10 virtual machine — Hyper-V host)
- Windows Server 2019 (My Lab VM — nested virtual machine)
- Hyper-V Manager
- Windows PowerShell (Administrator)
- Windows Server 2019 installation media (.iso)

## CompTIA Security+ Objectives Covered

- 3.1 – Compare and contrast security implications of different architecture models
- 3.2 – Given a scenario, apply security principles to secure enterprise infrastructure

---

## Methodology

### Part 1 – Installing Hyper-V

Installed the Hyper-V role on Windows Server 2019 using PowerShell.

- Ran the following command as Administrator to install Hyper-V with
  management tools:
  `Install-WindowsFeature -Name Hyper-V -IncludeManagementTools`
- Restarted the server to complete the installation

### Part 2 – Configuring a New Virtual Machine

Created a new virtual machine using the Hyper-V Manager New Virtual Machine Wizard.

- Opened Hyper-V Manager via Server Manager > Tools
- Selected New > Virtual Machine and stepped through the wizard
- Named the VM `My Lab VM`
- Selected Generation 1 — noted that Generation 2 does not support 32-bit
  operating systems
- Allocated 1024 MB startup memory
- Configured a 90 GB virtual hard disk
- Selected "Install an operating system later" on the Installation Options page
- Noted that bootable media for OS installation requires an `.iso` image file

### Part 3 – Installing an Operating System on the Virtual Machine

Mounted installation media and installed Windows Server 2019 on the VM.

- Attached the Windows Server 2019 installation media via DVD Drive settings
  using a physical CD/DVD drive mapped to Drive D:
- Loaded the installation disk via the lab resources interface
- Connected to the VM and started the installation
- Selected Windows Server 2019 Standard (Desktop Experience)
- Performed a custom installation (clean install) on the virtual hard disk
- Set the Administrator password to complete setup

### Part 4 – Configuring Virtual Networking Switches

Created both a Private and an External virtual switch using Hyper-V's Virtual
Switch Manager and evaluated the network isolation implications of each.

- Opened Virtual Switch Manager from the Hyper-V Actions panel
- Noted that the first available switch type option is External
- Created a Private virtual switch named `Private Lab Switch`
- Created an External virtual switch named `External Lab Switch`, associating
  it with the Microsoft Hyper-V Network Adapter on the host

### Part 5 – Connecting VMs to Virtual Switches and Testing Isolation

Connected My Lab VM to each switch type and verified network behavior.

- Connected My Lab VM to the Private Lab Switch via VM Settings > Network Adapter
- Assigned a static IP to My Lab VM (10.1.16.225, subnet 255.255.255.0)
- Ran `Ping -4 <MS10 IP>` from My Lab VM — ping failed, confirming that the
  private switch blocks communication with the host
- Switched My Lab VM's network adapter to the External Lab Switch
- Re-ran the ping — it succeeded, confirming full network access through the
  host's physical adapter

---

## Key Findings

### Virtual Switch Type Determines Network Isolation Level

Hyper-V offers three virtual switch types with distinct security implications:

| Switch Type | Host Access | Network Access | Other VMs |
|---|---|---|---|
| External | Yes | Yes | Yes |
| Internal | Yes | No | Yes |
| Private | No | No | Yes (same switch only) |

Selecting the appropriate switch type is a critical architectural decision with
direct security implications.

### Private Switch as a Security Sandbox

Connecting a new or untrusted VM to a private switch completely isolates it from
the production network and from the host. This is the recommended approach for
performing security checks on a new system before promoting it to a production
network — a practical application of network segmentation and zero-trust
principles.

### External Switch Requires a Host Physical Adapter

An external virtual switch must be bound to a physical network adapter on the
Hyper-V host. This gives VMs connected to it full outbound and inbound network
access, equivalent to placing a physical machine directly on the network. This
increases attack surface and requires the same hardening standards applied to
physical endpoints.

### Generation Selection Has Security Implications

Generation 2 VMs support UEFI and Secure Boot but do not support 32-bit
operating systems or legacy boot media. Generation 1 is required for legacy OS
compatibility but does not support Secure Boot, which is a meaningful security
distinction in environments with firmware-level threat considerations.

### VM File System Independence

Like containers, VMs maintain their own OS, hostname, IP address, and user
accounts entirely independent from the host. However, unlike containers, VMs
carry a full OS stack including a complete kernel, making them heavier but more
isolated and more flexible for running diverse workloads.

---

## Skills Demonstrated

- Hyper-V role installation using PowerShell on Windows Server 2019
- Virtual machine creation and configuration using the New VM Wizard
- OS installation from mounted virtual media inside a Hyper-V guest
- Virtual switch creation and type selection (External, Internal, Private)
- VM network adapter configuration and virtual switch assignment
- Network isolation testing using ping across switch types
- Static IP configuration inside a guest VM
- Evaluation of virtual networking security implications
- Application of network segmentation principles using virtual switch isolation

## Tools Used

- `Install-WindowsFeature -Name Hyper-V -IncludeManagementTools` — Hyper-V installation
- Hyper-V Manager — VM creation, configuration, and switch management
- Virtual Switch Manager — Private and External switch creation
- Windows PowerShell (Administrator) — Feature installation and network inspection
- `ipconfig` — IP address verification on host and guest
- `Ping -4` — Network reachability testing between host and guest VM
- Windows Server 2019 Setup — Guest OS installation

---

## Lab Result

**Score: 8/8 — Passed**
Duration: 1 hour, 1 minute

---

## Supporting Documentation

- [Lab – Completion: Using Virtualization](https://github.com/user-attachments/files/28276105/Lab06P2.-.Using.Virtualization.pdf)
- [Security+ V7 Badge: Cloud Centurion](https://github.com/user-attachments/files/28276134/Week06Certification.pdf)
