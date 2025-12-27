# Race Conditions - Toy to The World
Learn how to exploit a race condition attack to oversell the limited-edition SleighToy.


The Best Festival Company (TBFC) has launched its limited-edition SleighToy, with only ten pieces available at midnight. Within seconds, thousands rushed to buy one, but something strange happened. More than ten lucky customers received confirmation emails stating that their orders were successful. Confusion spread fast. How could everyone have bought the "last" toy? McSkidy was called in to investigate.  

She quickly noticed that multiple buyers purchased at the exact moment, slipping through the system’s timing flaw. Sir Carrotbane’s mischievous Bandit Bunnies had found a way to exploit this chaos, flooding the checkout with rapid clicks. By morning, TBFC faced angry shoppers, missing stock, and a mystery that revealed just how dangerous a few milliseconds could be during the holiday rush.

## Learning Objectives
- Understand what race conditions are and how they can affect web applications.
- Learn how to identify and exploit race conditions in web requests.
- How concurrent requests can manipulate stock or transaction values.
- Explore simple mitigation techniques to prevent race condition vulnerabilities.

---

Below is a **clean, copy-paste-ready THM GitHub write-up**, organised in a standard **README.md** style with clear sections and concise explanations.

---

# Race Condition Vulnerability

**TryHackMe – Web Exploitation**

## Overview

A **race condition** occurs when two or more actions execute at the same time and the final outcome depends on the order in which they complete. In web applications, this typically happens when multiple requests simultaneously access or modify shared resources such as inventory counts, balances, or user data.

If proper synchronisation is not implemented, race conditions can result in:

* Duplicate transactions
* Oversold inventory
* Unauthorized or inconsistent data changes

---

## Types of Race Conditions

### 1. Time-of-Check to Time-of-Use (TOCTOU)

This occurs when an application checks a condition and later uses the result, but the data changes in between.

**Example:**
Two users attempt to purchase the *last item* at the same time. The stock is checked before being updated, allowing both purchases to succeed.

---

### 2. Shared Resource Race Condition

Multiple users or processes modify the same resource simultaneously without proper locking or control.

**Example:**
Two cashiers update the same inventory record at the same time, causing one update to overwrite the other.

---

### 3. Atomicity Violation

An operation that should execute entirely (all-or-nothing) is split into multiple steps, allowing other requests to interfere.

**Example:**
A payment is recorded, but before order confirmation completes, another request interrupts the process, causing inconsistent system state.

---

## Exploitation Scenario – TBFC Shopping Cart

### Objective

Exploit a race condition in the TBFC shopping cart application to purchase more items than available stock.

---

## Environment Setup

### Configure Firefox with Burp Suite

1. Open **Firefox** on the AttackBox
2. Click the **FoxyProxy** icon
3. Select the **Burp** profile to route all traffic through Burp Suite

### Launch Burp Suite

1. Open **Burp Suite** from the desktop
2. Select **Temporary project (in memory)**
3. Click **Start Burp**

### Disable Intercept

* Navigate to **Proxy → Intercept**
* Ensure **Intercept is OFF** so requests pass through normally

---

## Making a Legitimate Request

1. Open Firefox and visit:

   ```
   http://MACHINE_IP
   ```

2. Log in using:

   * **Username:** attacker
   * **Password:** attacker@123

3. On the dashboard, observe the limited-edition **SleighToy** with only **10 units available**

4. Click **Add to Cart**

5. Proceed to **Checkout**

6. Click **Confirm & Pay**

 A success message confirms a legitimate purchase

---

## Exploiting the Race Condition

### Capture the Checkout Request

1. Go to **Burp Suite → Proxy → HTTP history**
2. Locate the **POST** request to:

   ```
   /process_checkout
   ```
3. Right-click → **Send to Repeater**

---

### Prepare Parallel Requests

1. Switch to the **Repeater** tab
2. Right-click the request tab → **Add tab to group**
3. Create a group named:

   ```
   cart
   ```
4. Right-click the request → **Duplicate tab**
5. Create **15 copies**

---

### Trigger the Race Condition

1. In Repeater, click the **Send** dropdown
2. Select:

   ```
   Send group in parallel (last-byte sync)
   ```
3. Click **Send group (parallel)**

 All checkout requests are sent simultaneously, maximising timing overlap.

---

## Result

* Multiple checkout requests succeed
* Inventory count drops below zero
* Multiple confirmed orders appear

This confirms a **race condition vulnerability**, allowing the attacker to bypass stock controls.

---

## Impact

* Overselling limited inventory
* Financial loss
* Data integrity issues
* Potential abuse at scale using automation

---

## Mitigation Strategies

To prevent race condition vulnerabilities:

* Use **atomic database transactions** to ensure stock deduction and order creation occur as one operation
* Perform **final stock validation** immediately before committing the transaction
* Implement **idempotency keys** to prevent duplicate checkout processing
* Apply **rate limiting and concurrency controls** on sensitive endpoints

---

## Conclusion

By replaying a legitimate checkout request multiple times in parallel using Burp Suite, we exploited a race condition caused by improper request synchronisation. This allowed purchases to succeed before inventory updates were applied, resulting in negative stock values.

Race conditions remain a critical but often overlooked vulnerability in modern web applications and require careful backend design to mitigate effectively.


