# DAY 01: Linux CLI - Shells Bells

A gamified exploration of the Linux CLI but the Learning objectives are awesome:)

## Learning Objectives
- Learn the basics of the Linux command-line interface (CLI)
- Explore its use for personal objectives and IT administration
- Apply your knowledge to unveil the Christmas mysteries

Mostly all server administration will require use of the Command Line Interface.

---
1. Basic linux CLI commands
   
![CLI01](images/CLI01)

---   
2. Navagating the file system.

![CLI02](images/CLI02)

---

3.Grepping the Logs: As guides, check **/var/log/** and **grep** inside, let the logs become your guide.
Look for eggs that want to hide, check their shells for what's inside!

**grep** is a command to look for a specific text inside a file.


![CLI03](images/CLI03)

---

4. Finding files in a directory using the find command.

![CLI04](images/CLI04)

Files with the **.sh** extension contain CLI commands and are called shell scripts. Such scripts are used both by IT teams to automate things and by attackers to quickly run malicious commands.

![CLI05](images/CLI05)

- The lines starting with # are just comments and are not the actual commands.
- The cat wishlist.txt | sort | uniq lists unique items from the wishlist.txt.
- The command then sends the output (unique orders) to the /tmp/dump.txt file.
- The rm wishlist.txt deletes the wishlist file (containing Christmas wishes).
- The mv eastmas.txt wishlist.txt replaces the original file with eastmas.txt.

---

5. System Utilities
  
There are hundreds of CLI commands to view and manage your system. For example, uptime to see how much time your system is running, ip addr to check your IP address, and ps aux to list all processes. You may also check the usernames and hashed passwords of users, such as McSkidy, by running cat /etc/shadow. However, you'd need root permissions to do that.

![CLI06](images/CLI06)

---

Bash History

Did you know that every command you run is saved in a hidden history file, also called Bash history? It is located at every user's home directory:

Every command you run is saved in a hidden history file, also called **Bash history**. It is located at every user's home directory: **/home/mcskidy/.bash_history** for McSkidy, and **/root/.bash_history** for root, and you can check it with a convenient history command, or just read the files directly with cat. Let's check if Sir Carrotbane with his bad bunnies left their traces in history!

Familiarize yourself with Bash history by running the history command.
Note that your commands are also saved to a file (cat .bash_history).


---
![CLI07](images/CLI07)










