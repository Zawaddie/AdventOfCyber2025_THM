# CyberChef - Hoperation Save McSkidy
The story continues, and the elves mount a rescue and will try to breach the Quantum Fortress's defenses and free McSkidy.

---
McSkidy is imprisoned in King Malhare's Quantum Warren. Sir BreachBlocker III was put in charge of securing the fortress and implemented several access controls to prevent any escape. His defenses are worthy of his name.

However, McSkidy managed to send vital clues to his team using harmless bunny pictures. One message revealed that five locks needed to be disabled to secure an escape route. The locks can be broken by examining their logic and leveraging the system's built-in chat for the guards. They can be eluded in revealing vital details or even passwords. However, you will need to speak their language.

## Learning Objectives
- Introduction to encoding/decoding
- Learn how to use CyberChef
- Identify useful information in web applications through HTTP headers

---

# TASK 1: Important Concepts

#  Encoding and Decoding

## Overview

**Encoding** is the process of transforming data to ensure **compatibility and usability** across different systems. It is commonly used when data needs to be transmitted or stored in a standardized format.

**Decoding** reverses this process, converting encoded data back into its original, readable form.

> ⚠️ Encoding is **not** a security mechanism and should not be confused with encryption.

---

##  Encoding vs Encryption

| Feature  | Encoding                  | Encryption                 |
| -------- | ------------------------- | -------------------------- |
| Purpose  | Compatibility & usability | Security & confidentiality |
| Process  | Standardized format       | Algorithm + secret key     |
| Security |  No                       | Yes                      |
| Speed    | Fast                      | Slower                     |
| Examples | Base64                    | TLS                        |

---

##  CyberChef Overview

**CyberChef** is commonly referred to as the **Cyber Swiss Army Knife**. It allows analysts to encode, decode, encrypt, decrypt, and analyze data using chained operations called **recipes**.

### CyberChef Interface Components

| Area           | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| **Operations** | Collection of available actions (encode, decode, hash, etc.) |
| **Recipe**     | Workspace to chain multiple operations                       |
| **Input**      | Data provided for processing                                 |
| **Output**     | Result after operations are applied                          |

---

##  Simple CyberChef Example

### Objective

Demonstrate encoding and decoding using Base64.

### Steps

1. Open **CyberChef**:

   * Online version (browser)
   * Offline version (AttackBox bookmarks)
2. Drag **To Base64** from **Operations** to **Recipe**
3. Enter the following into the **Input** field:

   ```
   IamRoot
   ```
4. Add **From Base64** to the recipe to decode the output
5. Observe the original input restored in the output

 You can enable or disable any operation by toggling the button on the right side of the operation.

✔️ This demonstrates **chaining operations** within a CyberChef recipe.

---

## 🕸️ Inspecting Web Pages

Browsers receive much more data than what is visually rendered on a web page. This hidden information can be extremely useful during investigations and CTF-style challenges.

### Accessing Developer Tools

| Browser        | Menu Path                                                         |
| -------------- | ----------------------------------------------------------------- |
| Chrome         | More tools → Developer tools                                      |
| Firefox        | ☰ Menu → More tools → Web Developer Tools                         |
| Microsoft Edge | Settings and more (…) → More tools → Developer tools              |
| Opera          | Developer → Developer tools                                       |
| Safari         | Develop → Show Web Inspector *(Enable in Preferences → Advanced)* |

>  Tip: Dock the console to the **right side** using the three-dot menu for better visibility.

---

##  Key Takeaways

* Encoding ensures **data compatibility**, not security
* Base64 is a common encoding format in web traffic
* CyberChef simplifies data transformation through chained recipes
* Developer tools reveal valuable hidden page data

 *Mastering encoding, decoding, and inspection tools is essential for web analysis and blue-team investigations.*

--- 

# TASK 2: First Lock

```markdown
# 🔑 Key Information & First Lock — Outer Gate

## 🚀 Accessing the Web Application

1. Start the **target machine** and allow a few minutes for it to fully boot.
2. From the **AttackBox**, access the web application at:

```

[http://10.67.129.122:8080](http://10.67.129.122:8080)

````
<img width="1357" height="568" alt="image" src="https://github.com/user-attachments/assets/65948c0d-72e3-45ee-aae3-3af01246e308" />

McSkidy has provided several critical clues. Every piece of information matters and will be reused throughout the locks.

---

## 🧠 Key Clues to Look Out For

### 📨 Encoded Chat
- All Bunnygram chat messages are **Base64 encoded**
- Decode messages using **CyberChef**
- From **Lock 3 onward**, guards respond more slowly

---

### 🧍 Guard Name
- Each level has a unique **guard name**
- The guard’s name is always required
- This pattern persists across all locks

---

### 🧾 HTTP Headers
- Inspect the page using **Developer Tools**
- Switch to the **Network** tab
- Refresh the page
- Select the **first request** to view response headers

Headers often reveal the **magic question** needed for the lock.

---

### 🔍 Login Logic
- Inspect the page and switch to the **Debugger** tab
- Each lock uses different login logic
- Helpful comments often indicate what needs to be “cooked” in CyberChef

---

## 🏰 First Lock — Outer Gate

<img width="1344" height="557" alt="image" src="https://github.com/user-attachments/assets/ca447e18-7671-4cb9-864d-b95b5f495b4c" />


Time to breach the fortress.

### Step-by-Step Solution

1. **Identify the guard name**
   - Encode the guard’s name in **Base64**
   - Use this encoded value as the **username**

<img width="1361" height="441" alt="image" src="https://github.com/user-attachments/assets/4a54abeb-9cf8-4c91-b3f9-12e3db23f41c" />

``plain text
plain text username: CottonTail

Base64 Encoded username: Q290dG9uVGFpbA==

```

2. **Find the magic question**
   - Inspect the **HTTP headers**
   - Magic question found:

     ```
     What is the password for this level?
     ```
<img width="1365" height="477" alt="image" src="https://github.com/user-attachments/assets/f45083c0-0801-4f0e-9aa1-4f14fcda701f" />

   - Encode the magic question in **Base64**

``plain text
plain text Magic question: What is the password for this level?

Base64 Encoded Magic question:V2hhdCBpcyB0aGUgcGFzc3dvcmQgZm9yIHRoaXMgbGV2ZWw/
```

<img width="1089" height="378" alt="image" src="https://github.com/user-attachments/assets/a74f9515-631e-49a9-932d-5cc7fd4f6f49" />


3. **Send the encoded question in chat**
   - Paste the encoded question into Bunnygram
   - The guard responds with a **Base64-encoded password**

 <img width="1358" height="451" alt="image" src="https://github.com/user-attachments/assets/9a380696-f62a-4f9c-9172-c952cc36fc22" />

``plain text
Base64 Encoded password: SWFtc29mbHVmZnk=

plain text Pasword: Iamsofluffy

```
<img width="1362" height="488" alt="image" src="https://github.com/user-attachments/assets/96f65e37-2bde-4a7f-acd1-18af1d1f4df2" />

4. **Inspect the login logic**
   - Open the **Debugger** tab
   - Logic shows the password is **Base64 encoded**


5. **Decode the guard’s response**
   - Decode the Base64 password using CyberChef
   - This reveals the **plaintext password**

6. **Log in**
   - **Username:** Base64-encoded guard name  
   - **Password:** Decoded plaintext password



---

## ✅ Result

🎉 **First lock successfully breached!**  
Only **four locks** remain.

Onward to the next challenge.

````

----

# Task 2: Second Lock

<img width="1359" height="426" alt="image" src="https://github.com/user-attachments/assets/fc9ca417-d178-4b33-9677-1686fd6f72db" />


``plain text
plain text username: CarrotHelm

Base64 Encoded username: Q2Fycm90SGVsbQ==
```
<img width="1365" height="565" alt="image" src="https://github.com/user-attachments/assets/21f4e103-0d3d-4daa-9fac-80d55c78983e" />

``plain text
plain text Magic question:  Did you change the password?

Base64 Encoded Magic question: RGlkIHlvdSBjaGFuZ2UgdGhlIHBhc3N3b3JkPw==
```
<img width="1363" height="427" alt="image" src="https://github.com/user-attachments/assets/a69d4419-de9a-49d4-b0da-afdc504d2fcf" />

<img width="1358" height="623" alt="image" src="https://github.com/user-attachments/assets/ad550b8c-b34c-4a57-b44d-f0338f3f4b81" />


Looking again at the login logic, you see that the encoding is applied twice this time. That means you have to decode from Base64 twice.

<img width="1365" height="471" alt="image" src="https://github.com/user-attachments/assets/841473af-b580-4596-b2da-e86f2e923e65" />


```plain text
Base64 Encoded password: U1hSdmJHUjViM1YwYjJOb1lXNW5aV2wwSVE9PQ==

plain text Pasword: Itoldyoutochangeit!

---

# Task 3: Third Lock

```markdown
# 🏰 Third Lock — Guard House

## 📌 Overview

As expected, the login logic is now more complex and introduces **chained operations**. This pattern will continue for the remaining locks.

For this level, you must carefully collect and process:
- **Encoded username**
- **Encoded password from the guard**
- **XOR key**

---

## 🔍 Key Information

- **XOR Key:**  
```

cyberchef

```

- From this lock onward:
- ❌ No magic question
- ✅ You can politely ask the guard for the password
- 🕒 Guards may respond slowly (2–3 minutes)
- 💡 Keep messages short (e.g., *“Password please.”*)

---

## 🔐 Login Logic

Inspection of the **Debugger** tab reveals the following password flow:

1. Password is **XOR’ed with a key**
2. Result is then **Base64 encoded**

To authenticate successfully, this process must be **reversed**.

---

## 🧠 Theory — XOR Explained

**XOR (Exclusive OR)** is a reversible operation that uses a **key**.

### Key Property

> XOR’ing data twice with the same key returns the original value.

In practice:
```

plaintext → XOR key → XOR key → plaintext

```

This property allows us to reverse the login logic.

---

## CyberChef Recipe (Reverse Logic)

To recover the plaintext password, build the following recipe in **CyberChef**:

1. **From Base64**
2. **XOR**
   - Key: `cyberchef`

This reverses:
```

XOR → Base64

```
back to:
```

Base64 → XOR

```

---

## Step-by-Step Solution

1. **Identify the guard name**
   - Encode it in **Base64**
   - Use it as the **username**

2. **Request the password from the guard**
   - Keep the message short
   - Receive a **Base64-encoded response**

3. **Decode the password**
   - Apply **From Base64**
   - Apply **XOR** using key `cyberchef`
   - Retrieve the **plaintext password**

4. **Log in**
   - **Username:** Base64-encoded guard name  
   - **Password:** Decoded plaintext password

---


``plain text
plain text username: 

Base64 Encoded username: 
```


``plain text
plain text Magic question: 

Base64 Encoded Magic question:
```
``plain text
Base64 Encoded password: 

plain text Pasword: 

---

## ✅ Result

🔓 **Third lock unlocked successfully!**  
The chained logic continues—onward to the next challenge.

---
