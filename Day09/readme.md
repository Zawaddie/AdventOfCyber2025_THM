# DAY 9

---

# How Attackers Recover Weak Passwords

## Overview

Attackers rarely attempt to **break modern encryption algorithms directly**—doing so would be computationally infeasible. Instead, they focus on **guessing the password protecting the encrypted file**. If the password is weak or predictable, recovering it is often trivial.

The two most common approaches are:

- **Dictionary attacks**
- **Brute-force and mask attacks**

Understanding these techniques is critical for both **offensive security testing** and **defensive detection**.

---

## Password Guessing Techniques

### Dictionary Attacks

A **dictionary attack** uses a predefined list of potential passwords (a *wordlist*) and tests each entry until the correct password is found.

Wordlists commonly include:

- Passwords leaked from previous data breaches
- Common passwords (`password`, `123456`, `qwerty`)
- Predictable variations (`password123`, `admin2024`)
- Names, dates, and simple substitutions (`@` for `a`, `0` for `o`)

Because many users reuse or slightly modify common passwords, dictionary attacks are:

- **Fast**
- **Highly effective**
- Often successful on the first attempt

**Common wordlists:**

- `rockyou.txt`
- `common-passwords.txt`
- SecLists collections

---

### Brute-Force and Mask Attacks

Brute-force attacks attempt **every possible character combination** until the correct password is found. While guaranteed to succeed eventually, the required time grows **exponentially** with password length and complexity.

#### Mask Attacks (Optimised Brute-Force)

Mask attacks reduce cracking time by limiting guesses to a **known or suspected format**.

Example mask:

**?l?l?l?d?d**



Meaning:

- `?l` → lowercase letter
- `?d` → digit
- Pattern: **three lowercase letters followed by two digits**

Mask attacks are especially effective when:

- Password policies are weak
- The attacker understands how users typically structure passwords

They strike a balance between **speed** and **coverage**.

---

## Practical Tips (Attackers Use — Defenders Should Know)

- Start with a **general wordlist** for fast wins  
  - Example: `rockyou.txt`
- If that fails, move to **targeted wordlists**
  - Company names
  - Project names
  - Usernames, emails, leaked internal data
- Use **mask or incremental attacks** for short passwords
- Prefer **GPU-accelerated cracking** when possible
- Monitor **CPU/GPU utilisation**
  - Cracking activity is resource-intensive and detectable

---

## Exercise: Cracking an Encrypted File

### Environment Setup

Files for this exercise are located in the **Desktop** directory.

```bash
cd Desktop











1. Confirm the File Type

Identify the file format before choosing tools.

file flag.pdf
file flag.zip


Decision path:

PDF → use PDF-specific tools

ZIP → use ZIP-specific tools

2. Tools to Use

Choose tools based on file type.

PDF

pdfcrack

john (via pdf2john)

ZIP

fcrackzip

john (via zip2john)

General

john (very flexible)

hashcat (GPU-accelerated, advanced)

3. Dictionary Attack (Fast and Often Successful)
PDF Example (pdfcrack)
pdfcrack -f flag.pdf -w /usr/share/wordlists/rockyou.txt


Sample output:

found user-password: 'XXXXXXXXXX'

ZIP Example (john)

Step 1: Extract the hash

zip2john flag.zip > ziphash.txt


Step 2: Run John

john --wordlist=/usr/share/wordlists/rockyou.txt ziphash.txt


Sample result:

XXXXXXXXXXX (flag.zip/flag.txt)


Step 3: Show cracked passwords

john --show ziphash.txt

Detection: Indicators and Telemetry

Offline password cracking does not generate login failures or account lockouts. Detection must focus on endpoint telemetry, not authentication logs.

Process and Command-Line Indicators

Common binaries:

john

hashcat

pdfcrack

fcrackzip

zip2john

pdf2john.pl

7z, 7za, qpdf, unzip

perl invoking pdf2john.pl

Suspicious command-line traits:

--wordlist, -w

--rules, --mask

-a 3, -m (Hashcat)

References to rockyou.txt, SecLists

zip2john, pdf2john

Artefacts:

~/.john/john.pot

.hashcat/hashcat.potfile

john.rec

OS-Specific Telemetry

Windows

Sysmon Event ID 1 (Process Creation with full command line)

Linux

auditd

execve syscall monitoring

EDR process telemetry

GPU and Resource Artefacts

GPU-based cracking is noisy.

Indicators include:

Sustained high GPU utilisation

Increased power draw and fan activity

Long-running processes visible via:

nvidia-smi


Loaded libraries:

nvcuda.dll

OpenCL.dll

libcuda.so

amdocl64.dll

Network Hints (Light but Useful)

Offline cracking does not require network access—but preparation does.

Watch for:

Downloads of large wordlists (rockyou.txt)

Git clones of password repositories

Package installs:

apt install john hashcat


GPU driver or runtime updates

Unusual File Activity

Repeated reads of:

Wordlists

Encrypted archives

Access to cracking output files and potfiles

Detection Examples

```code

Sysmon (Windows)
(ProcessName="C:\Program Files\john\john.exe" OR
 ProcessName="C:\Tools\hashcat\hashcat.exe" OR
 CommandLine="*pdf2john.pl*" OR
 CommandLine="*zip2john*")
```

Linux Audit Rules (Temporary)

```code
auditctl -w /usr/share/wordlists/rockyou.txt -p r -k wordlists_read
auditctl -a always,exit -F arch=b64 -S execve -F exe=/usr/bin/john -k crack_exec
auditctl -a always,exit -F arch=b64 -S execve -F exe=/usr/bin/hashcat -k crack_exec

```

```yaml

Sigma Rule (Windows – Process Creation)
title: Password Cracking Tools Execution
id: 9f2f4d3e-4c16-4b0a-bb3a-7b1c6c001234
status: experimental
logsource:
  product: windows
  category: process_creation
detection:
  selection_name:
    Image|endswith:
      - '\john.exe'
      - '\hashcat.exe'
      - '\fcrackzip.exe'
      - '\pdfcrack.exe'
      - '\7z.exe'
      - '\qpdf.exe'
  selection_cmd:
    CommandLine|contains:
      - '--wordlist'
      - 'rockyou.txt'
      - 'zip2john'
      - 'pdf2john'
      - '--mask'
      - ' -a 3'
  condition: selection_name or selection_cmd

```
level: medium

Response Playbook

When password-cracking activity is detected:

Isolate the host

Capture triage artefacts

Process list

Memory dumps

nvidia-smi output

Preserve evidence

Working directories

Wordlists

Hash files

Shell history

Assess impact

Identify decrypted files

Check for lateral movement or exfiltration

Determine intent

Was the activity authorised?

Escalate if required

Remediate

Rotate passwords and keys

Enforce MFA

Close out

User education

Restrict cracking tools to approved sandboxes

## Key Takeaway

Weak passwords move the attack away from cryptography and into guessing.
Defenders who understand cracking techniques are far better equipped to detect, investigate, and respond before real damage occurs.

