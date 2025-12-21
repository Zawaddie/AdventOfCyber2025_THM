# Day 13: YARA Rules - YARA mean one!

Learn how YARA rules can be used to detect anomalies.

---

When McSkidy went missing, there was chaos and uncertainty at The Best Festival Company (TBFC). However, even in her disappearance, McSkidy was trying to help the TBFC blue team. Taking a page out of the crisis communication process, McSkidy sent what looks like a bunch of images to the blue team from an anonymous location. These images looked like they were related to Easter preparations, but they contained a message sent by McSkidy. The crisis communication process outlines that a message might be sent through a folder of images containing hidden messages that can be decoded if you know the keyword. The blue team has to create a YARA rule that runs on the directory containing the images. The YARA rule must trigger on a keyword followed by a code word. After extracting all the code words in ascending order, the blue team will be able to decode the message.

## Learning Objectives

- Understand the basic concept of YARA.
- Learn when and why we need to use YARA rules.
- Explore different types of YARA rules.
- Learn how to write YARA rules.
- Practically detect malicious indicators using YARA.

---

# YARA Overview

## Introduction
YARA is a powerful detection tool used by defenders to **identify and classify malware** based on patterns rather than filenames. In the SOC-mas story, McSkidy tasks you with using YARA to uncover traces of King Malhare’s activity within TBFC systems.

Think of YARA as a **pattern-matching engine** for malware—scanning files, memory, and processes for digital fingerprints left by attackers.

---

## Why YARA Matters
Modern threats often blend in as legitimate files. YARA helps defenders spot what traditional tools may miss by detecting **behavioral and structural patterns**.

### Key Benefits
- **Speed** – Scans large datasets quickly  
- **Flexibility** – Supports text, hex, regex, and complex logic  
- **Control** – Analysts define what “malicious” means  
- **Shareability** – Rules can be reused and improved by others  
- **Visibility** – Connects scattered indicators into clear detections  

YARA enables defenders to move from passive monitoring to **active threat hunting**.

---

## When to Use YARA
Common defensive use cases include:
- **Post-incident analysis** – Check if malware exists elsewhere
- **Threat hunting** – Search for known malware families
- **Intelligence-based scans** – Apply shared community rules
- **Memory analysis** – Detect malicious code in running processes

---

## YARA Rule Structure
A YARA rule has three core sections:

- **Meta** – Descriptive information (author, purpose, date)
- **Strings** – Indicators to search for
- **Condition** – Logic that determines a match

### Example Rule

```yara
rule TBFC_KingMalhare_Trace
{
    meta:
        author = "Defender of SOC-mas"
        description = "Detects traces of King Malhare’s malware"
        date = "2025-10-10"

    strings:
        $s1 = "rundll32.exe" fullword ascii
        $s2 = "msvcrt.dll" fullword wide
        $url = /http:\/\/.*malhare.*/ nocase

    condition:
        any of them
}
```

---

## Strings in YARA

### 1. Text Strings
Simple words or phrases found in files or memory.

```yara
$xmas = "Christmas" nocase
```

**Common Modifiers**

- `nocase` – Case-insensitive

- `wide ascii` – Matches Unicode and ASCII

- `xor` – Detects XOR-obfuscated strings

- `base64 / base64wide` – Detects Base64-encoded content

### 2. Hexadecimal Strings

Used for raw byte patterns (e.g., file headers, shellcode).

```yara
$mz = { 4D 5A }   // PE file header
```

### 3. Regular Expressions

Flexible matching for changing patterns (URLs, commands).

```yara
$url = /http:\/\/.*malhare.*/ nocase
```

⚠️ Regex is powerful but can slow scans if too broad.

---

### Conditions

The condition section defines when a rule triggers.

**Common Examples** 

```yara
$xmas                  // match one string
any of them            // match any string
all of them            // match all strings
```

**Logical Operators**
```yara
($s1 or $s2) and not $benign
```

**File Property Checks**

```yara
any of them and filesize < 700KB
```

### Practical Use Case: IcedID Detection

TBFC analysts identified small IcedID loaders sharing:

- PE (MZ) headers
- Common binary fragments
- Consistent file sizes

**Detection Rule**
```yara
rule TBFC_Simple_MZ_Detect
{
    meta:
        author = "TBFC SOC L2"
        description = "IcedID Loader Detection"
        date = "2025-10-10"
        confidence = "low"

    strings:
        $mz   = { 4D 5A }
        $hex1 = { 48 8B ?? ?? 48 89 }
        $s1   = "malhare" nocase

    condition:
        all of them and filesize < 10MB
}
```

### Running YARA

```bash
yara -r icedid_starter.yar C:\
```


**Useful Flags**

- `-r`– Recursive directory scan

- `-s` – Show matched strings

### Summary

YARA is a defender’s precision tool for detecting hidden threats through patterns, not names. By combining smart strings and logical conditions, analysts can uncover malware even when attackers attempt to hide it.

➡️ Next step: create and test your own YARA rule.

<img width="681" height="557" alt="image" src="https://github.com/user-attachments/assets/13bd0da5-c7fe-4f4a-9897-1caca60c173a" />



