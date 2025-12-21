# Day 12: Phishing - Phishmas Greetings

Learn how to spot phishing emails from Malhare's Eggsploit Bunnies sent to TBFC users.

---

Since McSkidy’s disappearance, TBFC’s defences have weakened, and now the Email Protection Platform is down.
With filters offline, the staff must triage every suspicious message manually.
The SOC Team suspects Malhare’s Eggsploit Bunnies have sent phishing messages to TBFC’s users to steal credentials and disrupt SOC-mas.

Illustration of an Elf being phished by an Eggsploit Bunny.

You’ve joined the Incident Response Task Force to help identify which emails are legit or phishing attempts.

But beware, some phishing attempts are clever, disguised as routine TBFC operations, volunteer sign-ups, or SOC-mas event logistics. Every wrong call could bring Wareville one step closer to EAST-mas becoming a reality.

## Learning Objectives
- Spotting phishing emails
- Learn trending phishing techniques
- Understand the differences between spam and phishing

---

# Phishing Overview

## Introduction

Phishing is one of the oldest cyber attack techniques, yet it remains one of the most effective. As organisations like **TBFC** improve technical controls, attackers evolve their tactics to focus on **precision and persuasion** rather than volume.

Modern phishing targets **people**, not systems — making it a reliable path to initial access.

---

## Common Phishing Objectives

Phishing messages are designed to achieve one or more of the following:

- **Credential theft** – Stealing usernames and passwords
- **Malware delivery** – Delivering malicious attachments or links
- **Data exfiltration** – Collecting sensitive company or personal data
- **Financial fraud** – Triggering unauthorised payments or payroll changes

Attackers often impersonate employees or departments using **free email domains** to appear legitimate.

---

## Spam vs Phishing

Not every unwanted email is a threat.

### Spam
- Sent in bulk
- Marketing or promotional intent
- Low precision, low impact

**Common spam goals:**
- Advertising
- Clickbait traffic
- Data harvesting
- Scam exposure

### Phishing
- Targeted and deceptive
- Mimics trusted people or processes
- Designed to bypass user trust

**Key takeaway:**  
Spam is noise. Phishing is a **precision attack**.

---

## Common Phishing Techniques

### Impersonation

Attackers pretend to be:
- Employees
- Managers
- IT or HR departments
- Trusted services

**Indicator:**  
Sender email does not match the organisation’s domain.

Example:
- Display name looks legitimate
- Sender domain uses a free provider (e.g. `gmail.com`)

---

### Social Engineering

Phishing relies on **human manipulation**, not technical exploits.

Common techniques include:
- **Urgency** – “URGENT”, “Immediately required”
- **Authority** – Impersonating senior staff
- **Fear or pressure** – Incident response or account compromise
- **Side-channel avoidance** – Asking victims not to verify via normal channels

Goal: trick users into sharing credentials, approving payments, or opening files.

---

### Typosquatting and Punycode

#### Typosquatting
Attackers register **lookalike domains**:
- `glthub.com` instead of `github.com`

#### Punycode
Unicode characters are used to visually mimic Latin letters:
- Greek `α` instead of Latin `a`
- Cyrillic characters replacing normal letters

**Detection tip:**
- Check email headers
- Look for `xn--` (ACE prefix) in the **Return-Path**

---

### Email Spoofing

Email spoofing forges sender details to appear legitimate.

Indicators in email headers:
- Failed **SPF**
- Failed **DKIM**
- Failed **DMARC**
- Mismatched **Return-Path**

If all authentication checks fail, spoofing is highly likely.

---

### Malicious Attachments

Classic phishing often includes attachments disguised as:
- Voice messages
- Invoices
- Reports

Common malicious formats:
- `.html`
- `.hta`
- `.zip`

HTML/HTA files are dangerous because they execute scripts with **full user permissions**, bypassing browser sandboxing.

---

## Trending Phishing Techniques

As email security improves, attackers shift tactics.

### Current Trends

- Avoid direct malware delivery
- Use **legitimate services** (OneDrive, Google Docs, Dropbox)
- Redirect users to:
  - Fake login pages
  - Credential harvesters
  - Malicious downloads

Goal: **steal access**, not drop malware.

---

### Legitimate Services Abuse

Attackers hide behind trusted platforms:
- OneDrive
- Google Docs
- Dropbox

Common lures:
- Salary increases
- Laptop upgrades
- Shared IT documents

These messages often combine:
- Punycode domains
- Trusted services
- Attractive proposals

---

### Fake Login Pages

Credentials are the primary target.

Attackers clone:
- Microsoft 365
- Google login portals
- Internal company portals

**Red flags:**
- Suspicious domains
- Extra words or numbers in URLs
- Login pages reached via email links

---

### Side-Channel Communication

Attackers move conversations off email to:
- WhatsApp
- Telegram
- SMS
- Phone calls
- Shared document platforms

This bypasses corporate monitoring and increases trust.

---

## Key Takeaway

Phishing remains effective because it exploits **human trust**.  
By analysing **intent**, **sender identity**, **domains**, **attachments**, and **headers**, defenders can reliably distinguish spam from real threats and stop attackers before initial access is achieved.

---

## Task

We Proceed to analyse the provided emails and identify phishing indicators.



