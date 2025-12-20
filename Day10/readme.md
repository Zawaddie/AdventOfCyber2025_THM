# SOC Alert Triaging - Tinsel Triage

Investigate and triage alerts through Microsoft Sentinel.

---

## Scenario

The Best Festival Company's Security Operations Center was in chaos. Screens flickered, lights flashed, and the sound of alerts echoed through the room like a digital thunderstorm. The elves rushed between consoles, their faces lit by the glow of red and orange warnings. It was raining alerts, and no one knew where the storm had begun.

Whispers spread through the SOC as tension filled the air. Something strange was happening across the cloud environment, and the timing couldn't be worse. As the blizzard of alerts grew heavier, one name surfaced among the worried elves: the evil Easter Bunnies. But why now? And what were they after this time?

## Learning Objectives
Understand the importance of alert triage and prioritisation
Explore Microsoft Sentinel to review and analyse alerts
Correlate logs to identify real activities and determine alert verdicts

---

# ALERT Triaging Primer

# It’s Raining Alerts ☔

## Scenario Overview

McSkidy receives a flood of alerts from the Azure tenant — dashboards are lighting up, and early indicators suggest suspicious activity linked to the **Evil Bunnies**. With the festival season at risk, the SOC must quickly separate **signal from noise** before operations are impacted.

When alerts spike, investigating each one individually is inefficient. Some alerts are benign, others are false positives, and only a few indicate real threats. This is where **alert triaging** becomes critical.

---

## Why Alert Triaging Matters

Alert triaging helps SOC analysts:

- Identify alerts requiring **immediate action**
- Deprioritise low-risk or duplicate alerts
- Avoid wasting time on false positives
- Focus resources on real threats in progress

A structured triage process turns chaos into clarity.

---

## Alert Triaging Fundamentals

When multiple alerts appear, analysts should assess them consistently using four key factors:

### Key Triage Factors

| Factor | Description | Why It Matters |
|------|------------|----------------|
| **Severity** | Informational → Critical | Indicates urgency and business risk |
| **Time** | When the alert fired and how often | Helps detect ongoing or repeated activity |
| **Context** | Stage in the attack lifecycle | Shows how far the attacker has progressed |
| **Impact** | Affected user, system, or resource | Prioritises based on business importance |

### Triage Summary

- **Severity** → How bad?
- **Time** → When?
- **Context** → Where in the attack?
- **Impact** → Who or what is affected?

These four dimensions provide a fast but effective decision-making framework.

---

## Triage Decision Outcomes

After reviewing the alert:

- **Escalate** → Indicators of active compromise
- **Investigate further** → More context or correlation needed
- **Close or suppress** → Confirmed false positive

---

## Diving Deeper into an Alert

Once an alert is prioritised, investigation begins.

### Investigation Steps

1. **Review alert details**
   - Entities, event data, and detection logic
   - Validate if behaviour is truly malicious

2. **Check related logs**
   - Examine relevant log sources
   - Look for anomalies or supporting evidence

3. **Correlate alerts**
   - Identify alerts tied to the same user, IP, or device
   - Correlation often reveals attack chains

4. **Build a timeline**
   - Combine timestamps, actions, and assets
   - Determine if the threat is ongoing or contained

5. **Decide next actions**
   - Escalate to IR if compromised
   - Continue investigation if uncertain
   - Close and tune detections if false positive

6. **Document findings**
   - Record analysis, decisions, and remediation steps
   - Improves SOC maturity and future response

---

## Key Takeaway

When alerts rain down, **triage is the umbrella**.  
By prioritising severity, timing, context, and impact, SOC analysts can quickly identify real threats, reduce noise, and uncover the attacker’s bigger picture — just as McSkidy begins connecting the dots inside the Azure tenant.

---

## Task

Proceed to the next section and begin analysing the alerts on the azure portal -->Microsoft Sentinel

<img width="1365" height="618" alt="image" src="https://github.com/user-attachments/assets/1b2f9912-b9ae-4a69-af2d-a580be1e7b37" />



---

# McSkidy Goes Triaging

## Scenario Context

With alert triaging fundamentals covered, McSkidy now moves into the **Best Festival Company SOC**, hosted in **Azure**. Here, she applies her skills using **Microsoft Sentinel**, a cloud-native SIEM and SOAR platform.

Microsoft Sentinel ingests logs from Azure services and connected data sources, enabling analysts to:
- Monitor alerts and incidents
- Correlate activity across logs
- Investigate and respond to threats in real time

The goal is to uncover the truth behind the **Evil Bunnies’** activity in the Azure tenant.

---

## Microsoft Sentinel in Action

### Accessing Incidents

1. Open **Microsoft Sentinel**
2. Select your **Sentinel instance**
3. Navigate to: **Threat management → Incidents**
4. 4. Expand the view if needed using the `<<` button

> **Note**
> - Refresh the page if incidents do not appear  
> - Set a **custom date range** if no incidents are visible

---

## Incident Overview

In this lab environment, multiple incidents are visible:
- **High severity** and **Medium severity** alerts  
- Counts may vary depending on the instance

Since high-severity incidents represent the greatest risk, triage begins there.

<img width="1365" height="641" alt="image" src="https://github.com/user-attachments/assets/b598c1f1-c812-4165-8757-30a49d693983" />


---

## High-Severity Incident: Kernel Module Insertion

McSkidy starts with the **Linux PrivEsc — Kernel Module Insertion** incident.

By selecting the incident, Sentinel reveals a summary panel with key details:

- Multiple related events
- Recent alert creation time
- Several affected entities
- Classified under **Privilege Escalation**

Click **View full details** to access:
- Incident timeline
- Related entities
- Similar incidents

---

## Understanding Related Alerts

In the detailed view, multiple alerts may reference the **same entity** (VM, user, or IP). This indicates the alerts are likely **connected stages of the same attack**, not isolated events.

### Example Alert Chain

| Alert | What It Indicates |
|------|------------------|
| Root SSH Login from External IP | Initial access gained |
| SUID Discovery | Privilege escalation reconnaissance |
| Kernel Module Insertion | Persistence via kernel-level access |

By identifying shared entities, McSkidy can trace the attacker’s **progression through the attack lifecycle**.

---

## Triage Outcome

At this stage, McSkidy has:

- Prioritised **high-severity** incidents
- Identified affected hosts and users
- Recognised multiple alerts as part of a **single compromise**

This initial triage provides enough context to decide which incidents need immediate attention and which require deeper investigation.


---

## Next Step

With the surface-level triage complete, McSkidy is ready to dive into the **underlying logs** to validate activity and uncover additional evidence in the next task.

---

# In-Depth Log Analysis with Sentinel

## Overview

With initial triage complete, McSkidy now investigates the **raw log data** behind the alerts. The goal is to validate detections and uncover the **exact attacker actions** that triggered them.

By analysing authentication attempts, command execution, and system changes in **Microsoft Sentinel**, McSkidy begins reconstructing how the attack unfolded.

---

## Reviewing Alert Evidence

From the **incident full details** view, select **Events** under the *Evidence* section.

This view reveals:
- The **kernel module name** installed
- The **timestamp** of installation
- The **affected host(s)**

These details confirm that a kernel-level modification occurred and provide a pivot point for deeper analysis.

---

## Querying Raw Logs with KQL

To investigate further, McSkidy queries raw logs for a single affected host: `app-02`.

### Steps

1. Switch from **Simple mode** to **KQL mode**
2. Run the following query:

```kql
set query_now = datetime(2025-10-30T05:09:25.9886229Z);
Syslog_CL
| where host_s == 'app-02'
| project _timestamp_t, host_s, Message


