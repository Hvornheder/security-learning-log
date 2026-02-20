## Overview

This first entry in my Security Learning Log documents a system configuration gap analysis performed on Windows Server 2019 (Version 1809). The objective was to compare a live system configuration against Microsoft’s recommended security baseline and identify deviations that could increase enterprise risk exposure.

This exercise reinforces foundational security concepts including configuration management, baseline enforcement, and authentication hardening.

## Environment

- Windows Server 2019 (Version 1809, Build 17763)
- Virtual Machine (PC10)
- PowerShell (Administrator)
- Microsoft Security Compliance Toolkit
- Policy Analyzer

## Methodology

1. Validated the operating system version using `winver` to ensure baseline alignment.
2. Copied compliance toolkit files from mounted virtual media using PowerShell.
3. Extracted baseline templates and launched Policy Analyzer.
4. Imported the Microsoft security baseline:
   `MSFT-Win10-v1809-RS5-WS2019-FINAL`
5. Used Policy Analyzer’s **“Compare to Effective State”** function to evaluate the live system configuration against baseline-recommended values.
6. Identified deviations and analyzed their security implications.

## Key Findings

Two significant deviations were observed:

### Account Lockout Threshold
- **Baseline Value:** 10 failed attempts
- **Effective System Value:** 0 (No lockout enforced)

Impact:
- Allows unlimited login attempts
- Increases exposure to brute-force and password spraying attacks
- Represents a critical authentication hardening gap

### Minimum Password Length
- **Baseline Value:** 14 characters
- **Effective System Value:** 7 characters

Impact:
- Reduces password entropy
- Increases susceptibility to brute-force attacks
- Falls below recommended enterprise security standards

## Security Impact Analysis

This exercise demonstrated how configuration drift can introduce meaningful risk even when systems appear operationally functional.

The lab reinforced that:
- Gap analysis identifies misalignment but does not enforce remediation.
- Baselines must match the exact OS version to ensure valid audit results.
- Not all deviations carry equal risk, but authentication policy gaps significantly increase attack surface.

## Skills Demonstrated

- Configuration auditing and baseline validation
- Use of Microsoft Security Compliance Toolkit
- Policy interpretation and comparison analysis
- PowerShell file operations and tool deployment
- Risk assessment of authentication controls
- Identification of brute-force exposure risks

## Lessons Learned

- Secure configuration is foundational to enterprise defense.
- Weak authentication policies dramatically increase risk exposure.
- Baseline comparison tools are critical for maintaining compliance and detecting misconfigurations.
- Governance requires both assessment and enforcement mechanisms.

## Supporting Documentation

Lab completion verification:
- [Security+ Lab 01 – Perform System Configuration Gap Analysis](https://github.com/user-attachments/files/25454014/Lab01.-.Performing.System.Configuration.Gap.Analysis.pdf)

