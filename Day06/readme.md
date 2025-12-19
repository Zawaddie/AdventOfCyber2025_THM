# Malware Analysis - Egg-xecutable

Discover some common tooling for malware analysis within a sandbox environment.

---

## Task Summary – Malware Analysis Using Sandboxes

In this task, I explored the fundamental principles of **malware analysis** and the role of **sandboxed environments** in safely examining malicious samples. Malware analysis focuses on understanding how malicious software behaves, the actions it performs on a system, and how its activity can be detected, mitigated, or prevented. By analyzing malware, defenders can identify indicators such as network communication with attacker-controlled servers or system artifacts left behind after execution, which can be used to strengthen detection and response strategies.

The task introduced the two main approaches to malware analysis:

- **Static Analysis** – examining a malicious file without executing it to understand its structure and potential behavior.
- **Dynamic Analysis** – executing the malware in a controlled environment to observe its real-time behavior and impact on the system.

These approaches complement each other and provide deeper insight into an attacker’s techniques and objectives.

A key concept emphasized was the use of **sandboxes**, typically implemented as virtual machines, to safely execute potentially malicious code in an isolated environment. Sandboxes protect production systems and sensitive data while allowing analysts to observe malware behavior. Features such as system isolation and snapshotting make virtual machines particularly effective for malware analysis.

Overall, this task reinforced best practices in malware analysis, highlighting the importance of isolation, safe handling of malicious samples, and translating technical findings into actionable defensive measures. It also emphasized a core principle of malware analysis: **never execute untrusted code on systems you care about**.

---

## PRACTICAL:Static and Dynamic Malware Analysis

This room focused on the safe and systematic analysis of a malicious sample using both static and dynamic analysis techniques within a sandboxed environment. Emphasis was placed on the importance of executing potentially malicious code only in isolated systems to prevent unintended damage and contamination.

The **static analysis phase** involved examining the malware sample without execution to gather early indicators of compromise. Using tools such as **PeStudio,** key information was extracted including file hashes for threat intelligence, embedded strings that revealed potential flags and network indicators, and imports that suggested how the malware interacts with the Windows operating system.

**Dynamic analysis** was then performed to observe the malware’s real behavior during execution. **Regshot** was used to compare registry states before and after execution, allowing identification of registry modifications associated with persistence mechanisms. **Process Monitor (ProcMon)** was leveraged to monitor runtime activity such as file creation, registry access, and network communications. Filtering by process name and TCP operations helped isolate relevant events and identify the protocol used by the malware to communicate externally.

Overall, this room demonstrated a practical workflow for malware analysis by combining static inspection with behavioral analysis. It reinforced best practices in malware handling, persistence detection, and network activity investigation, while highlighting how analysis results can be translated into actionable defensive insights.

### Tools Used

**PeStudio** – Static analysis of Windows executables, including hash extraction, strings analysis, and import inspection

**Regshot** – Registry snapshot comparison to identify changes and persistence mechanisms

**Process Monitor (ProcMon)** – Runtime monitoring of file, registry, and network activity

**Windows Virtual Machine** – Isolated sandbox environment for safe malware execution

## Skills Demonstrated

Malware static analysis and indicator extraction

Malware dynamic analysis and behavioral monitoring

Identification of persistence mechanisms via registry analysis

Network activity analysis using process-level monitoring

Safe handling of malicious samples in sandboxed environments

Translating technical findings into defensive and detection insights

