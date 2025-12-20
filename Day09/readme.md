# DAY 9

---

# How Attackers Recover Weak Passwords

## Overview

Attackers rarely attempt to **break modern encryption algorithms directly**—doing so would be computationally infeasible. Instead, they focus on **guessing the password protecting the encrypted file**. If the password is weak or predictable, recovering it is often trivial.

The two most common approaches are:

- **Dictionary attacks**
- **Brute-force and mask attacks**

Understanding these techniques is critical for both **offensive security testing** and **defensive detection**.

---

## Password Guessing Techniques

### Dictionary Attacks

A **dictionary attack** uses a predefined list of potential passwords (a *wordlist*) and tests each entry until the correct password is found.

Wordlists commonly include:

- Passwords leaked from previous data breaches
- Common passwords (`password`, `123456`, `qwerty`)
- Predictable variations (`password123`, `admin2024`)
- Names, dates, and simple substitutions (`@` for `a`, `0` for `o`)

Because many users reuse or slightly modify common passwords, dictionary attacks are:

- **Fast**
- **Highly effective**
- Often successful on the first attempt

**Common wordlists:**

- `rockyou.txt`
- `common-passwords.txt`
- SecLists collections

---

### Brute-Force and Mask Attacks

Brute-force attacks attempt **every possible character combination** until the correct password is found. While guaranteed to succeed eventually, the required time grows **exponentially** with password length and complexity.

#### Mask Attacks (Optimised Brute-Force)

Mask attacks reduce cracking time by limiting guesses to a **known or suspected format**.

Example mask:

