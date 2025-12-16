# Day 5: IDOR - Santa’s Little IDOR

## Scenario

Learn about IDOR while helping pentest the TrypresentMe website.The elves of Wareville are on high alert since McSkidy went missing. Recently, the support team has been receiving many calls from parents who can't activate vouchers on the TryPresentMe website. They also mentioned they are receiving many targeted phishing emails containing information that is not public. The support team is wary and has enlisted the help of the TBFC staff. When looking into this peculiar case, they discovered a suspiciously named account named Sir Carrotbane, which has many vouchers assigned to it. For now, they have deleted the account and retrieved the vouchers. But something is going on. Can you help the TBFC staff investigate the TryPresentMe website and fix the vulnerabilities?

## Learning Objectives

- Understand the concept of authentication and authorization
- Learn how to spot potential opportunities for Insecure Direct Object References (IDORs)
- Exploit IDOR to perform horizontal privilege escalation
- Learn how to turn IDOR into SDOR (Secure Direct Object Reference)

## **Introduction**

**Insecure Direct Object Reference (IDOR)**, is an access control vulnerability where applications expose internal object identifiers (like user IDs or package numbers) without properly checking whether the requester is authorized to access the related data.

Using a package-tracking example, it shows how sequential or predictable IDs (e.g., `packageID=1001`) can be easily modified to view other users’ sensitive information if the server does not enforce authorization checks. This often happens because applications trust user-supplied references and fail to verify ownership.

The text clarifies key concepts:

* **Authentication** confirms who a user is.
* **Authorization** determines what that user is allowed to access.
* IDOR is typically a form of **horizontal privilege escalation**, where users access data belonging to others at the same privilege level.

## Practical IDOR examples include:

* Directly changing numeric IDs (e.g., `user_id=10` → `11`)
* Obfuscated references using Base64 encoding
* Hashed identifiers (e.g., MD5)
* Predictable algorithms like **UUID version 1**, which can be brute-forced if generation time is known

---

## Practical

We start th target machine and access the given Ip address, **http://10.66.139.58** in a Web Browser.

**Accsing the TryPresentMe website:**

<img width="1352" height="583" alt="image" src="https://github.com/user-attachments/assets/17d1a9ca-b09d-40c9-b1a7-4ddb1c5f5c68" />


We authenticate to the Website through the given credentials:
    username: niels
    password: TryHackMe#2025

<img width="1355" height="548" alt="image" src="https://github.com/user-attachments/assets/e1875705-e2dc-4c1c-b353-dfbc4924c555" />


<img width="1358" height="550" alt="image" src="https://github.com/user-attachments/assets/b92a96d4-3047-46ab-8778-b5c2ef98351d" />

Let's start by using the **Developer Tools** of our browser to better understand what is happening in the background. Right-click on the page and click Inspect, then click on the Network tab as shown below:

We then re-load the page to see what happens:

<img width="1365" height="509" alt="image" src="https://github.com/user-attachments/assets/7a4afa54-8875-4a7e-90fe-01f13b3dd017" />

Let's take a closer look at the view_accountinfo request. 

<img width="1363" height="455" alt="image" src="https://github.com/user-attachments/assets/72ce75e0-6365-4d74-84f4-e57fd16ddcbe" />

In the above image we can see that the user_id with the value of 10 was used for the request. If we click and expand the Response tab, we can see that this user_id corresponds to our user:

<img width="1364" height="461" alt="image" src="https://github.com/user-attachments/assets/627c5a98-7552-4ea0-9151-da8557c40132" />

This tells us that the application is using our user_id as the reference for getting details. Let's see what happens when we change this. In the Developer Tools, navigate to the Storage tab and 

<img width="1362" height="458" alt="image" src="https://github.com/user-attachments/assets/3b0bee05-2707-4b32-878b-b4dd8eeab48d" />


Expand the Local Storage dropdown on the left side and click the URL inside it:

<img width="1365" height="452" alt="image" src="https://github.com/user-attachments/assets/b4de65ab-495d-45ce-a7b0-d392a928cce9" />

et's change the user_id to 11 and see what happens. Double-click on the Value field of the auth_user data entry, update the user_id to 11 and save it by pressing Enter. Now refresh the page. All of a sudden it seems like you are a completely different user!

We refresh the page with the user_id as 11

<img width="1362" height="554" alt="image" src="https://github.com/user-attachments/assets/c1a2b9e3-fed9-42d9-ac66-e39791995f6c" />


This is the simplest form of IDOR. Simply changing the user_id to something else means we can see other users' data. 

Some IDORs might be slightly more hidden. Just because you don't see a direct number doesn't mean it doesn't exist! 

Let's dive deeper. To continue onto the next challenges of the task, go and change the id back to 10 using the same steps you followed above. Alternatively, you can log out of the application and log back in using the username and password.

## In Disguise: Obvious References

Sometimes, IDOR may not be as simple as just a number. In certain cases, encoding may have been used. 

On the loaded profile, we click the eye icon next to the first. Now go back to the Network tab and take a look at the requests being made; you should see a request like this:

<img width="1365" height="362" alt="image" src="https://github.com/user-attachments/assets/711c7f65-1e98-4143-a513-74745be12d57" />


Simply put, the Mg== is just the base64 encoded version of the number 2. You could still perform IDOR using this request, but you would have to base64 encode the number first.
In Digests, Objects Remain

Encoding isn't the only thing that can be used to hide potential IDOR vulnerabilities. Sometimes the values may look like a hash, such as MD5 or SHA1. If you want to see what that would look like, take a look at the request that happens when you click the edit icon next to a child.

<img width="1365" height="371" alt="image" src="https://github.com/user-attachments/assets/1a24e44e-a8f6-473c-8dca-c9403daf0129" />

While the string may look random, upon further investigation, you can see that it is a type of hash. If we understand what value was used to generate the hash, we can perform an IDOR attack by simply replicating the hashing function. Using something like a hash identifier can help you quickly understand what hashing algorithm is being used and can often tell you what data was hashed.

## It's Deterministic, Obviously Reproducible

Sometimes you have to dig quite deep for IDOR. Sometimes IDOR is not as clear. Sometimes the IDOR stems from the actual algorithm being used. In this last case, let's take a look at our vouchers. 

While the values may look random, we need to investigate what algorithm was used to generate them. Their format looks like a UUID, so let's use a website such as UUID Decoder to try to understand what UUID format was used. 

<img width="1079" height="478" alt="image" src="https://github.com/user-attachments/assets/305d92ad-629c-4526-b4b3-f884671ec016" />

<img width="1304" height="460" alt="image" src="https://github.com/user-attachments/assets/76b955fb-cbbd-4530-9ca0-d6f30de8c0ac" />

Copy one of the vouchers to the website for decoding,

<img width="1363" height="344" alt="image" src="https://github.com/user-attachments/assets/2c35aff1-494e-4487-a4e2-0fb302078660" />

<img width="1147" height="343" alt="image" src="https://github.com/user-attachments/assets/c72b2ef6-a049-466a-9512-3dc131e93eae" />


and you should see something like this:

<img width="1110" height="314" alt="image" src="https://github.com/user-attachments/assets/22d1db35-2705-4f15-821c-f0b4634da443" />

<img width="838" height="310" alt="image" src="https://github.com/user-attachments/assets/e9dfaf41-089d-4c13-9728-8637b08741df" />


While these look completely random, we can see that the UUID version 1 was used. The issue with UUID 1 is that if we know the exact date when the code was generated, we can recover the UUID. For example, suppose we knew the elves always generated vouchers between 20:00 - 21:00. In that case, we can create UUIDs for that entire time period (3600 UUIDs since we have 60 minutes, and 60 seconds in a minute), which we could use in a brute force attack to aim to recover a valid voucher and get more gifts.

Now that we have seen the various IDORs that can be found, let's discuss how to fix them and avoid them!
Improve Design, Obliterate Risk

Now that we learned about what IDOR is, let's discuss how to fix it. The best way to stop IDOR is to make sure the server checks who is asking for the data every time. It's not enough to hide or change the ID number; the system must confirm that the logged-in user is authorized to see or change that information.

Don't rely on tricks like Base64 or hashing the IDs; those can still be guessed or decoded. Instead, keep all the real permission checks on the server. Whenever a request comes in, check: "Does this user own or have permission to view this item?"

Use random or hard-to-guess IDs for public links, but remember that random IDs alone don't make your app safe. Always test your app by trying to open another user's data and making sure it's blocked. Finally, record and monitor failed access attempts; they can be early signs of someone trying to exploit an IDOR.

<img width="569" height="331" alt="image" src="https://github.com/user-attachments/assets/7e0d4b76-67a1-41b4-9b75-30e34d4e40e1" />






























## Conclusion 

**Hiding or encoding IDs is not a fix for IDOR**. Proper mitigation requires **server-side authorization checks on every request**, ensuring users can only access resources they own or are permitted to view. Using unpredictable IDs, thorough testing, and monitoring failed access attempts further helps reduce IDOR risk.

