## XSS - Merry XSSMas

Learn about types of XSS vulnerabilities and how to prevent them.

---

After last year's automation and tech modernisation, Santa's workshop got a new makeover. McSkidy has a secure message portal where you can contact her directly with any questions or concerns. However, lately, the logs have been lighting up with unusual activity, ranging from odd messages to suspicious search terms. Even Santa's letters appear to be scripts or random code. Your mission, should you choose to accept it: dig through the logs, uncover the mischief, and figure out who's trying to mess with McSkidy. 

## Learning Objectives

Understand how XSS works
Learn to prevent XSS attacks

---
# Equipment Check – XSS Basics

## Lab Setup

For this room, we interact with the web application hosted at:


Access the site using the **AttackBox browser**. You should see the main portal page.

This task focuses on understanding and exploiting **Cross-Site Scripting (XSS)** vulnerabilities observed in the application logs.

---

## Cross-Site Scripting (XSS)

XSS is a web vulnerability that allows attackers to inject **malicious JavaScript** into a web application. When user input is not properly validated or escaped, browsers may interpret it as executable code.

XSS is a web application vulnerability that lets attackers (or evil bunnies) inject malicious code (usually JavaScript) into input fields that reflect content viewed by other users (e.g., a form or a comment in a blog). When an application doesn't properly validate or escape user input, that input can be interpreted as code rather than harmless text. This results in malicious code that can steal credentials, deface pages, or impersonate users. Depending on the result, there are various types of XSS.  In today’s task, we focus on Reflected XSS and Stored XSS.


Impact of XSS includes:
- Session hijacking
- Credential theft
- Page defacement
- User impersonation

This room focuses on:
- **Reflected XSS**
- **Stored XSS**

---

## Reflected XSS

Reflected XSS occurs when user input is **immediately reflected** in the server’s response without sanitisation.

### Example

A normal search request: **https://trygiftme.thm/search?term=gift**


Malicious version: **https://trygiftme.thm/search?term=
<script>alert(atob("VEhNe0V2aWxfQnVubnl9"))</script>**


If a victim clicks the malicious link, the injected script executes in their browser.

### Impact

- Executes code in the victim’s session
- Commonly delivered via phishing
- Allows attackers to act as the victim

---

## Stored XSS

Stored XSS occurs when malicious input is **saved on the server** and executed whenever any user loads the affected page.

### Normal Comment Submission

```http
POST /post/comment HTTP/1.1
Host: tgm.review-your-gifts.thm

postId=3
name=Tony Baritone
email=tony@normal-person-i-swear.net
comment=This gift set my carpet on fire but my kid loved it!

```

## Malicious Comment Submission

```http
comment=<script>alert(atob("VEhNe0V2aWxfU3RvcmVkX0VnZ30="))</script>
```


Because the input is stored, every visitor triggers the script.

## Impact

- Steals cookies

- Displays fake login prompts

- Defaces pages

- Executes attacker code automatically

### Exploiting Reflected XSS

The portal includes a search field, making it a good test target.

Test Payload

```html
<script>alert('Reflected Meow Meow')</script>
```

### Steps

1. Paste the payload into the search box

2. Click Search Messages

3. If an alert appears, reflected XSS is confirmed

<img width="1355" height="434" alt="image" src="https://github.com/user-attachments/assets/e81e6044-e9dc-47c5-8a73-3adee7e4aaa0" />


## Why It Works

- Input is reflected without encoding

- Browser executes injected JavaScript

- Alert confirms code execution

System behaviour can be observed in the System Logs section.
<img width="792" height="357" alt="image" src="https://github.com/user-attachments/assets/bc03009c-862e-4379-8f80-d3faa0e3867f" />

<img width="1301" height="503" alt="image" src="https://github.com/user-attachments/assets/cb1ffbad-f655-4de2-9f08-342001a2df1c" />



### Exploiting Stored XSS

Stored XSS requires a persistent input field.

The message form allows messages to be saved on the server.

Payload

```html
<script>alert('Stored Meow Meow')</script>

```
Steps

1. Navigate to the message form

2. Submit the payload

3. Reload or revisit the page

The alert triggers every time, confirming stored XSS.

###  Protecting Against XSS

Each service is different, and requires a well-thought-out, secure design and implementation plan, but key practices you can implement are:

Disable dangerous rendering raths: Instead of using the innerHTML property, which lets you inject any content directly into HTML, use the textContent property instead, it treats input as text and parses it for HTML.
Make cookies inaccessible to JS: Set session cookies with the HttpOnly, Secure, and SameSite attributes to reduce the impact of XSS attacks.
Sanitise input/output and encode:
In some situations, applications may need to accept limited HTML input—for example, to allow users to include safe links or basic formatting. However it's critical to sanitize and encode all user-supplied data to prevent security vulnerabilities. Sanitising and encoding removes or escapes any elements that could be interpreted as executable code, such as scripts, event handlers, or JavaScript URLs while preserving safe formatting.
To exploit XSS vulnerabilities, we need some type of input field to inject code. There is a search section, let's start there.

**Key defensive measures include:**

- Avoid dangerous rendering paths

Use textContent instead of innerHTML

- Harden cookies

Set HttpOnly, Secure, and SameSite

- Sanitise and encode input/output

Strip scripts, event handlers, and JS URLs

Allow only safe HTML where required

### Wrap-Up

The application is vulnerable to both Reflected and Stored XSS, explaining the suspicious payloads observed in the logs. The development team must now harden input handling to prevent future exploitation.


