# Forensics - Registry Furensics
Learn what the Windows Registry is and how to investigate it.

---

# 🪟 Windows Registry — Fundamentals & Forensics

## 📌 Overview

The **Windows Registry** acts as the operating system’s configuration database. Just as the human brain stores behaviors and memories, the registry stores **system settings, user preferences, application data, and activity history** required for Windows to function.

Unlike a single database file, the registry is distributed across multiple files called **Registry Hives**, each responsible for different configuration categories.

---

## 🗂️ Windows Registry Hives

| Hive Name        | Contains                                 | Location                                                         |
| ---------------- | ---------------------------------------- | ---------------------------------------------------------------- |
| **SYSTEM**       | Services, drivers, hardware, boot config | `C:\Windows\System32\config\SYSTEM`                              |
| **SECURITY**     | Local security & audit policies          | `C:\Windows\System32\config\SECURITY`                            |
| **SOFTWARE**     | Installed programs, OS info, autostarts  | `C:\Windows\System32\config\SOFTWARE`                            |
| **SAM**          | User accounts, password hashes, groups   | `C:\Windows\System32\config\SAM`                                 |
| **NTUSER.DAT**   | User activity, preferences, Run history  | `C:\Users\username\NTUSER.DAT`                                   |
| **USRCLASS.DAT** | Shellbags, Jump Lists                    | `C:\Users\username\AppData\Local\Microsoft\Windows\USRCLASS.DAT` |

> ⚠️ Each hive stores far more data than the examples listed above.

---

## 🧩 Registry Hives vs Root Keys

Registry hives are **not viewed directly**. Instead, Windows exposes them via **Registry Root Keys** in the Registry Editor.

### Hive Mapping

| Hive on Disk | Registry Editor Path                       |
| ------------ | ------------------------------------------ |
| SYSTEM       | `HKEY_LOCAL_MACHINE\SYSTEM`                |
| SECURITY     | `HKEY_LOCAL_MACHINE\SECURITY`              |
| SOFTWARE     | `HKEY_LOCAL_MACHINE\SOFTWARE`              |
| SAM          | `HKEY_LOCAL_MACHINE\SAM`                   |
| NTUSER.DAT   | `HKEY_USERS\<SID>` and `HKEY_CURRENT_USER` |
| USRCLASS.DAT | `HKEY_USERS\<SID>\Software\Classes`        |

### Notes

* **HKLM** contains system-wide configuration
* **HKCU / HKU** contain user-specific data
* **HKCR** and **HKCC** are dynamically generated (no backing hive file)

---

## 🧭 Viewing the Registry

Registry hive files contain **binary data** and cannot be opened directly.

### Built-in Tool

* **Registry Editor** (`regedit`)
* Allows live inspection of registry data
* ⚠️ **Not suitable for forensic analysis** on compromised systems

---

## 🔎 Registry Data Examples

### Example 1: Connected USB Devices

**Hive:** SYSTEM
**Path:**

```text
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USBSTOR
```

Stores:

* USB make and model
* Device IDs
* Unique device instances

---

### Example 2: Programs Run via Win + R

**Hive:** NTUSER.DAT
**Path:**

```text
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU
```

Stores:

* Commands executed via the Run dialog
* Useful for tracking user activity

---

## 🕵️ Registry Forensics

**Registry forensics** involves extracting and analyzing registry data to reconstruct attacker activity and timelines. It is a core component of Windows incident response alongside:

* Event logs
* File system artifacts
* Memory analysis

### High-Value Registry Keys

| Registry Key                   | Forensic Value                     |
| ------------------------------ | ---------------------------------- |
| `HKCU\...\UserAssist`          | Recently executed GUI applications |
| `HKCU\...\TypedPaths`          | Explorer address bar entries       |
| `HKCU\...\WordWheelQuery`      | Explorer search terms              |
| `HKCU\...\RecentDocs`          | Recently accessed files            |
| `HKLM\...\Run`                 | Startup persistence mechanisms     |
| `HKLM\...\App Paths`           | Application execution paths        |
| `HKLM\SYSTEM\...\ComputerName` | Hostname                           |
| `HKLM\SOFTWARE\...\Uninstall`  | Installed applications             |

---

## 🛠️ Offline Registry Analysis

### Why Offline Analysis?

* Live systems risk **evidence modification**
* Registry Editor cannot open offline hives
* Many values are binary and unreadable

### Solution: Registry Explorer

* Open-source forensic tool
* Parses binary registry data
* Supports offline hive analysis

---

## 🧪 Practical — Registry Explorer Analysis

### Target System

* **Hostname:** `DISPATCH-SRV01`
* **Incident Start:** `21 October 2025`
* **Hive Location:**

  ```text
  C:\Users\Administrator\Desktop\Registry Hives
  ```

---

### Step 1: Launch Registry Explorer

* Open **Registry Explorer** from the taskbar

<img width="798" height="613" alt="image" src="https://github.com/user-attachments/assets/3d78b9e6-f520-460b-ba1c-35567ed89cbe" />


---

### Step 2: Load Registry Hives

1. Click **File → Load Hive**
2. Navigate to the Registry Hives directory
3. Select a hive (e.g., `SYSTEM`)

---

### Step 3: Handling Dirty Hives

To ensure consistency:

* Hold **SHIFT** while clicking **Open**
* This loads associated transaction logs (`.LOG1`, `.LOG2`)
* Confirm successful transaction replay

✔️ Repeat for all required hives

---

### Step 4: Investigate Registry Keys

#### Example: Find Computer Name

**Path:**

```text
ROOT\ControlSet001\Control\ComputerName\ComputerName
```

**Result:**

```
DISPATCH-SRV01
```

💡 Tip:

* Use **Search** or **Available Bookmarks** for faster navigation

---

## 🧠 Investigation Guidance

* Focus on **installed applications**, **startup entries**, and **user activity**
* Correlate findings with the attack start date
* The **Registry Forensics table** is essential for this task

---

## ❓ Challenge Questions

1. **What application was installed before abnormal activity started?**
2. **What is the full execution path of that application?**
3. **Which registry value was added for startup persistence?**

---

## 📚 Further Learning

If you enjoyed this room, check out:

* **Expediting Registry Analysis** (THM)

---

🧾 *Registry analysis provides powerful insight into attacker behavior and persistence mechanisms when performed correctly and offline.*



