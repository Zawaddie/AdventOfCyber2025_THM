# warming up for the Aoc

![warmUP](images/warmUP.png)

## Challenge 1: Password Pandemonium
While login into my new TBFC workstation, an alert pops up: **“Weak passwords detected on 73 TBFC accounts!”**

Strong passwords are one of the simplest yet most effective defences against cyber attacks. 
So the Objective of this challenge is  to **Create a password that passes all system checks and isn’t found in the leaked password list.**

![pass001c1](images/pass001c1.png)

The **challenge's flag** was obtained by following the below **good password hygene:**
- Enter a password with at least 12 characters.
- Include uppercase, lowercase, numbers, and symbols.
- Ensure it isn’t in the breach database.

![pass002c1](images/pass002c1.png)

---

## Challenge 2: The Suspicious Chocolate.exe

Checking suspicious files is a crucial skill for every defender. Thus the Objective here is to determine if **chocolate.exe** is safe or infected.

We run a simulated VirusTotal tool. The scan reports is 49 clean results, 1 malicious.

![choco001c2](images/choco001c2.png)

The **flag** was obtained by deciding that the file is not safe but malicious

![choco002c2](images/choco002c2.png)

---

## Challenge 3: Welcome to the AttackBox!

As a defenders getting comfortable with the command line is a first step toward cyber mastery.

The objective here is to find and read the hidden welcome message inside your linux-based AttackBox.

We use **ls** to list files, **cd challenges/** to change directories and **cat welcome.txt** to read the text file which has the **flag.**

![attackbox001c3](images/attackbox001c3.png)

---

## Challnge 4: The CMD Conundrum

When a workstation shows signs of tampering, suspicious files moved, logs wiped, and a strange folder (named mystery_data), the Windows Command Prompt can be used to uncover what’s hidden.
Learning windows commands helps a defender investigate systems and find what the GUI can’t.

The Objective is to find the hidden flag file using Windows commands.

We Use **dir** to list visible files, Try **dir /a** to reveal hidden ones and **type hidden_flag.txt** to read the flag.

![winCMD001c4](images/winCMD001c4.png)

---
## Challenge 5: Linux Lore

Linux powers most servers worldwide, and knowing how to search within it is a must for any defender.

Here the Objective is to Locate McSkidy’s hidden message in his Linux home directory.

We use **cd /home/mcskidy/** to enter his folder, run **ls -la** to show all files and then Use **cat .secret_message** to reveal the flag.

![linuxLore001c5](images/linuxLore001c5.png)

---

## Challenge 6: The Leak in the List

Defenders often use tools like **Have I Been Pwned** to check for compromised accounts. Early detection can stop an attack from spreading.

This challenge's objective is to check if McSkidy’s email has appeared in a breach.

We thus enter **mcskidy@tbfc.com** into the breach checker, review results for each domain and then identify the one marked **“Compromised.”**

![leak001c6](images/leak001c6.png)

Clicking the compromised domain reveals the flag.

![leak002c6](images/leak002c6.png)

---

## Challenge 7: WiFi Woes in Wareville

Securing WiFi is critical. Default passwords are like leaving the front gate wide open.

Thus the objective here is to log into the router and secure it with a strong new password.

As network admins, we log in with credentials **admin:admin**, Go to **“Security Settings.”** and Set a new strong password that passes validation.

![wifi001c7](images/wifi001c7.png)

---

## Challenge 8: The App Trap

When running a company's social account, always learn to review and manage app permissions  to help stop data leaks before they start.

Here, we find and remove the malicious connected app by looking for one with unusual permissions (like “password vault” access) and then **“Revoking Access.”**

![appTrap001c8](/images/appTrap001c8.png)

---

## Challenge 9: The Chatbot Confession

AI tools can be powerful. An AI assistant is meant to give positive assistance. However it can spill secrets and reveal internal URLs and even passwords. Thus **defenders must know how to prevent them from oversharing.**

Reading each line of the conversation, Selecting the ones containing private data and Submitting findings gives us the **flag.**

![chatbot001c9](/images/chatbott001c9.png)

## Challenge 10: The Bunny’s Browser Trail
 
User Agent strings like **“User Agent: BunnyOS/1.0 (HopSecBot)”**  in HTTP logs help defenders spot automated or suspicious visitors in network logs.

Reading the provided web log entries comparing them to common browsers (Chrome, Firefox, Edge), Identifying and selecting the suspicious entry gives the flag.

![bunnyBrowser001c10](/images/bunnyBrowser001c1.png)


