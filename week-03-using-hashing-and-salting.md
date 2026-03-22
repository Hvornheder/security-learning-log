# Week 03: Hashing, Salting, and File Integrity Validation

## Overview

This lab focused on applying cryptographic concepts to validate file integrity, evaluate suspicious files through hash-based threat intelligence, and demonstrate how salting strengthens password hash resistance against brute-force attacks.

The objective was to use hashing to verify file consistency, perform malware research using a file hash rather than the original file, and compare the security difference between salted and unsalted password hashes.

This exercise reinforced core Security+ concepts related to data integrity, password storage security, cryptographic weaknesses, and secure analysis workflows.

## Environment

- Kali Linux
- OpenSSL
- John the Ripper
- Linux `/etc/shadow`
- MetaDefender Cloud
- Suspicious sample files in Kali lab resources

## Methodology

1. Navigated to the suspicious file directory:  
   `/usr/share/windows-resources/binaries`

2. Enumerated files and identified `nc.exe` as the file of interest.

3. Generated a SHA1 hash of the suspicious file using:  
   `sha1sum nc.exe`

4. Used the resulting hash to perform a lookup in MetaDefender Cloud.

5. Interpreted multi-engine detection results to distinguish between malicious software, dual-use tools, and potentially unwanted programs (PUPs).

6. Returned to the root home directory and inspected Linux password hash entries in `/etc/shadow` to observe salted password storage.

7. Created an unsalted MD5 password hash using OpenSSL and saved it to `hash.txt`.

8. Used John the Ripper to crack the unsalted password hash.

9. Created a salted MD5 password hash using a known salt and saved it to `salted-hash.txt`.

10. Verified that John the Ripper could still crack the salted hash when the salt remained visible in the full hash string.

11. Removed the visible salt from the hash file and tested cracking again to observe the dramatic increase in cracking difficulty when the salt was unknown.

## Key Findings

### File Integrity Verification with Hashing

A SHA1 hash was generated for `nc.exe` and used as a file fingerprint for reputation analysis.

**Security implication:**
- Hashes allow analysts to verify file integrity without relying on filenames alone.
- Even a small file change produces a different hash value.
- Hash comparison is an efficient method for detecting tampering or confirming known-good/known-bad files.

---

### Threat Intelligence via Hash Lookup

The suspicious file hash was searched in MetaDefender Cloud instead of uploading the actual file.

**Security implication:**
- Hash-based lookup can reduce the risk of exposing a suspicious or proprietary file to third-party services.
- Detection results must be interpreted carefully because not every flagged file is inherently malicious.
- Some detections may reflect administrative tools, hacker tools, or PUP classifications rather than confirmed malware.

---

### Salted vs. Unsalted Password Hashes

An unsalted MD5 hash of `pass1` was cracked quickly with John the Ripper.

A salted MD5 hash using the salt `SALT` was also cracked quickly when the salt remained visible in the hash string.

After removing the salt value from the hash file, the projected cracking time increased dramatically.

**Security implication:**
- Unsalted hashes are far more vulnerable to brute-force and precomputed cracking attacks.
- Salting increases cracking complexity by forcing attackers to account for extra unknown characters.
- Salts do not make passwords uncrackable, but they significantly reduce the usefulness of rainbow tables and identical-hash reuse.

## Security Impact Analysis

This exercise demonstrated three important defensive lessons:

- **Hashing supports integrity validation** by allowing defenders to detect whether a file has changed.
- **Salting improves password hash resilience** by increasing the search space attackers must brute force.
- **Older algorithms such as MD5 are not suitable for modern security use** because of known weaknesses, especially collision issues.

The lab also reinforced an important analytical principle: detection output is evidence, not proof. Security analysts must interpret results in context before classifying a file as malicious.

## Skills Demonstrated

- File integrity verification using cryptographic hashing
- Hash-based malware research and threat intelligence lookup
- SHA1 and MD5 hash generation in Linux
- Password hash analysis and salt identification
- Use of OpenSSL for password hashing
- Use of John the Ripper for brute-force password testing
- Interpretation of dual-use tools vs. confirmed malware
- Security analysis of password storage practices

## Lessons Learned

- Hashes are essential for integrity validation and forensic comparison.
- Salting is a critical control for protecting stored password hashes.
- A file flagged by some engines is not automatically malicious.
- MD5 should be avoided in modern environments due to its collision weakness.
- Threat research by hash can be safer than uploading a file directly.

## Supporting Documentation

- [Lab Completion Verification](https://github.com/user-attachments/files/26163424/Lab03.-.Assisted.Live.Lab.Using.Hashing.and.Salting.pdf)

- [Security+ V7 Badge / Certificate](https://github.com/user-attachments/files/26163559/Week03Certification.pdf)
