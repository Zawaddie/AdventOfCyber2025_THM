# Web Attack Forensics - Drone Alone

Explore web attack forensics using Splunk.

---

TBFC’s drone scheduler web UI is getting strange, long HTTP requests containing Base64 chunks. Splunk raises an alert: “Apache spawned an unusual process.” On some endpoints, these requests cause the web server to execute shell code, which is obfuscated and hidden within the Base64 payloads. For this room, your job as the Blue Teamer is to triage the incident, identify compromised hosts, extract and decode the payloads and determine the scope.

You’ll use Splunk to pivot between web (Apache) logs and host-level (Sysmon) telemetry.

Follow the investigation steps below; each corresponds to a Splunk query and investigation goal.

## Learning Objectives
- Detect and analyze malicious web activity through Apache access and error logs
- Investigate OS-level attacker actions using Sysmon data
- Identify and decode suspicious or obfuscated attacker payloads
- Reconstruct the full attack chain using Splunk for Blue Team investigation

---

## 🔍 Splunk Investigation — Web Attack Analysis
### 🚀 Accessing the Splunk Dashboard

After starting the AttackBox and target machine, we allow ~3 minutes for services to fully initialize.

**🔐 Login Details**

URL: `http://MACHINE_IP:8000`

Username: `Blue`

Password: `Pass1234`

Protocol: `HTTP`

<img width="863" height="418" alt="image" src="https://github.com/user-attachments/assets/b8c54781-ca21-4cd2-a9b1-04bc306d93da" />


Upon successful login, you will be redirected to the Splunk Search Dashboard.

<img width="1354" height="417" alt="image" src="https://github.com/user-attachments/assets/c6e804bc-def0-4d6c-915c-2c006e9c34c1" />

| ⚠️ Important: We adjust the time range to Last 7 Days or All Time. A narrow time window may result in “No results found”.

---

## 🧙 Blue Team Perspective

In this task, we follow **Elf Log McBlue** as he uses Splunk to trace an attacker’s activity through **web logs, error logs**, and **Sysmon process creation events**.

### 🕵️ Detect Suspicious Web Commands
**Objective**

Identify command injection attempts in Apache access logs.

**Splunk Query**

```spl
index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression")
| table _time host clientip uri_path uri_query status
```
<img width="1362" height="440" alt="image" src="https://github.com/user-attachments/assets/e0981ac6-b4eb-40b2-9e0f-a1d9430a9d4f" />

**Analysis**
- Searches for suspicious command execution keywords
- Targets abuse of vulnerable CGI scripts (e.g., hello.bat)
- Focus on Base64-encoded PowerShell commands

**Base64 Decoding**
Example encoded payload:

```nginx
VABoAGkAcwAgAGkAcwAgAG4AbwB3ACAATQBpAG4AZQAhACAATQBVAEEASABBAEEASABBAEEA
```

**Decode using:**

```url
https://www.base64decode.org/
```
<img width="801" height="618" alt="image" src="https://github.com/user-attachments/assets/ea3c6f61-44c9-4cc4-89a3-d305a02f9290" />

➡️ Decoding helps reveal the attacker’s intended command.

----

### 🧾 Inspect Apache Error Logs
**Objective**

Determine whether malicious requests reached backend execution.

**Splunk Query**

```spl
index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")
```
<img width="1365" height="505" alt="image" src="https://github.com/user-attachments/assets/f0cb4f26-d07d-4b00-8705-5e60cd2b8bfb" />

**Notes**

- Switch View → Raw in Splunk
- A 500 Internal Server Error after a request like:

```bash
/cgi-bin/hello.bat?cmd=powershell
```

strongly suggests attempted command execution

**NB:** If a request like **/cgi-bin/hello.bat?cmd=powershel**l triggers a 500 “Internal Server Error,” it often means the attacker’s input was processed by the server but failed during execution, a key sign of exploitation attempts.

Checking these results helps confirm whether the attack reached the backend or remained blocked at the web layer.

✔️ Confirms exploitation attempts beyond simple probing.

---

### ⚙️ Trace Malicious Process Creation (Sysmon)
**Objective**

Identify system-level command execution spawned by Apache.

**Splunk Query**
```sql
index=windows_sysmon ParentImage="*httpd.exe"
```
 <img width="1365" height="516" alt="image" src="https://github.com/user-attachments/assets/be721a06-41d5-4971-ae6a-015c24ac5665" />

**Expected vs Malicious Behavior**

✅ Normal: Apache worker threads

❌ Suspicious:

```ini
ParentImage = C:\Apache24\bin\httpd.exe
Image       = C:\Windows\System32\cmd.exe
```
**NB**: ypically, Apache should only spawn worker threads, not system processes like cmd.exe or powershell.exe.

If results show child processes such asParentImage = `C:\Apache24\bin\httpd.exe` and Image        = `C:\Windows\System32\cmd.exe`, It indicates a successful command injection where Apache executed a system command.



The finding above is one of the strongest indicators that the web attack penetrated the operating system.

🚨 This indicates **successful command injection** and OS-level compromise.

---

## 👤 Confirm Attacker Enumeration
**Objective**

Detect post-exploitation reconnaissance.

**Splunk Query**

```spl
index=windows_sysmon *cmd.exe* *whoami*
```
<img width="1361" height="508" alt="image" src="https://github.com/user-attachments/assets/fc28ea64-4cf4-4be0-8a23-ef922178d3e3" />

**Why This Matters**

- Attackers commonly run whoami to identify execution context

- Confirms code execution on the host

✔️ Strong indicator of attacker foothold.

---

## 🔐 Identify Base64-Encoded PowerShell Payloads
**Objective**

Find hidden or obfuscated PowerShell commands.

**Splunk Query**

```sql
index=windows_sysmon Image="*powershell.exe"(CommandLine="*enc*" OR CommandLine="*-EncodedCommand*" OR CommandLine="*Base64*")
```
<img width="1357" height="340" alt="image" src="https://github.com/user-attachments/assets/eda4f429-807c-48d9-bbe5-f6c1320caedd" />

**Interpretation**
- Encoded commands are commonly used to evade detection
- If no results appear, security controls likely prevented execution
- Any hits should be decoded to inspect attacker intent

**NB:** This query detects PowerShell commands containing -EncodedCommand or Base64 text, a common technique attackers use to hide their real commands.If your defences are correctly configured, this query should return no results, meaning the encoded payload (such as the “Muahahaha” message) never ran.

---
## ✅ Key Takeaways
- Splunk is powerful for attack path reconstruction
- Web logs reveal initial exploitation
- Error logs confirm backend interaction
- Sysmon validates OS-level execution
- Encoded PowerShell is a common attacker technique

📌 This investigation confirms a command injection attack that progressed from web exploitation to system-level execution.


