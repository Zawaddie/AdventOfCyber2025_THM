# AoC pre-prep

## Challenge 1: Password Pandemonium
While login into my new TBFC workstation, an alert pops up: **“Weak passwords detected on 73 TBFC accounts!”**

Strong passwords are one of the simplest yet most effective defences against cyber attacks. 
So the Objective of this challenge is  to **Create a password that passes all system checks and isn’t found in the leaked password list.**

![pass001c1](images/pass001c1)

The **challenge's flag** was obtained by following the below **good password hygene:**
- Enter a password with at least 12 characters.
- Include uppercase, lowercase, numbers, and symbols.
- Ensure it isn’t in the breach database.

![pass002c1](images/pass002c1)

---

## Challenge 2: The Suspicious Chocolate.exe

Checking suspicious files is a crucial skill for every defender. Thus the Objective here is to determine if **chocolate.exe** is safe or infected.

We run a simulated VirusTotal tool. The scan reports is 49 clean results, 1 malicious.

![choco001c2](images/choco001c2)

The **flag** was obtained by deciding that the file is not safe but malicious

![choco002c2](images/choco002c2)

---

## Challenge 3: Welcome to the AttackBox!

As a defenders getting comfortable with the command line is a first step toward cyber mastery.

The objective here is to find and read the hidden welcome message inside your linux-based AttackBox.

We use **ls** to list files, **cd challenges/** to change directories and **cat welcome.txt** to read the text file which has the **flag.**

![attackbox001c3](images/attackbox001c3)

---

## Challnge 4: The CMD Conundrum

When a workstation shows signs of tampering, suspicious files moved, logs wiped, and a strange folder (named mystery_data), the Windows Command Prompt can be used to uncover what’s hidden.
Learning windows commands helps a defender investigate systems and find what the GUI can’t.

The Objective is to find the hidden flag file using Windows commands.

We Use **dir** to list visible files, Try **dir /a** to reveal hidden ones and **type hidden_flag.txt** to read the flag.

![winCMD001c4](images/winCMD001c4)

---
## Challenge 5: Linux Lore

Linux powers most servers worldwide, and knowing how to search within it is a must for any defender.

Here the Objective is to Locate McSkidy’s hidden message in his Linux home directory.

We use **cd /home/mcskidy/** to enter his folder, run **ls -la** to show all files and then Use **cat .secret_message** to reveal the flag.

![linuxLore001c5](images/linuxLore001c5)

---

## Challenge 6: The Leak in the List

Defenders often use tools like **Have I Been Pwned** to check for compromised accounts. Early detection can stop an attack from spreading.

This challenge's objective is to check if McSkidy’s email has appeared in a breach.

We thus enter **mcskidy@tbfc.com** into the breach checker, review results for each domain and then identify the one marked **“Compromised.”**

![leak001c6](images/leak001c6)

---

## Challenge 7:

