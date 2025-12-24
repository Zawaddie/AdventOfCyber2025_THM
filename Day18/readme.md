# Obfuscation - The Egg Shell File
McSkidy keeps her focus on a particular alert that caught her interest: an email posing as northpole-hr.

---

WareVille has not felt right since the wormhole appeared. Everyone in TBFC is on high alert: Systems are going haywire, dashboards are spiking, and SOC alerts have been firing nonstop. Amid the chaos, McSkidy keeps her focus on a particular alert that caught her interest: an email posing as northpole-hr. It’s littered with carrot emojis, but the weird thing is this: there is no North Pole human resources department. TBFC’s HR is at theSouth Pole.

Digging further she found a tiny PowerShell file from that email was downloaded. Among the code are random strings of characters, all gibberish, nothing readable at a glance.

McSkidy knows malicious actors often hide code and data using a technique called obfuscation. But what is it, really? And how can we decipher it?

# Learning Objectives
- Learn about obfuscation, why and where it is used.
- Learn the difference between encoding, encryption, and obfuscation.
- Learn about obfuscation and the common techniques.
- Use CyberChef to recover plaintext safely.

---

# Understanding Obfuscation (Gibberish)

## What is Obfuscation?
Obfuscation is the practice of making data difficult to read or analyze. Attackers use it to evade detection and slow down investigations.

---

## Simple Ciphers

### ROT Ciphers
ROT ciphers shift letters forward in the alphabet.

- **ROT1**: `a → b`, `b → c`
  - Example:  
    `carrot coins go brr` → `dbsspu dpjot hp css`
- **ROT13**: Shifts letters by 13 positions.
  - Common indicators:
    - `the` → `gur`
    - `and` → `naq`
- Spaces remain unchanged, making ROT ciphers easy to spot.

---

## Obfuscation in the Real World

### XOR Obfuscation
- Each character (byte) is combined with a key using the XOR operation.
- Output often contains unreadable or unusual characters.
- Hard to do manually, commonly done with tools.

---

## Using CyberChef (XOR Example)

1. Open **CyberChef**.
2. Paste input text: `carrot supremacy`
3. Add **XOR** operation to the Recipe.
4. Set:
   - **Key**: `a`
   - **Key format**: `HEX`
5. View output (click **BAKE!** if needed).

<img width="1360" height="477" alt="image" src="https://github.com/user-attachments/assets/5bfe956e-e63e-478e-8d98-28d17f2fb8d8" />

Example result:
```

ikxxe~*yzxogkis!

```

---

## Detecting Obfuscation Patterns

You can use these quick visual clues to guess the obfuscation technique used:

| Technique | Indicators |
|---------|-----------|
| ROT1    | Words look one letter off, spaces unchanged |
| ROT13   | Familiar 3-letter words (`the → gur`) |
| Base64  | Alphanumeric strings, may end with `=` or `==` |
| XOR     | Random symbols, same length as original, possible repeating patterns |

Once identified, reverse the process in CyberChef (e.g., **From Base64** instead of **To Base64**).

---

## Unfamiliar Patterns

If unsure:
- Try multiple CyberChef operations.
- Use **Magic** operation to auto-detect common encodings.
- Enable **Intensive mode** for broader testing.

> Magic is a helper, not a guarantee—custom XOR keys and layered techniques may fail.

<img width="764" height="411" alt="image" src="https://github.com/user-attachments/assets/ebf80622-0737-43e4-9f20-a3a01f06a293" />


---

## Layered Obfuscation

Attackers often combine techniques.

Example workflow:
1. Gzip compress
2. XOR with key `x`
3. Base64 encode

Sample output:
```

H4sIADKZ42gA/32PT2sqQRDE7/MpitGDgrPEJJcXyOGha1xwVaLwyLvI6rbuhP3HTHswm/3uzmggIQfrUD3VzI/u7iDXljepNkFth6KDmYsWnBF2R2OoZOyrPCUDXSKB1UWdEzjZ5hQI8c9oJrU4cn1kyPXbMgRW0X/nF8WLcTSJwvFX9Jr/jUP5i1NOgPeLfjxvtKQQL8RqlOk8jZgKfGJSmTDZZWqxfacdoxFAl0814Rl6j153EyxXkR1VJSe6JNNHAzmOXiHRgnJLPk+iWeiyR63+uIm6Hb5B92NG5YGzK5sm7FnfTSxfzl3rgoJ1tWKjy0NPnpxUHKs0xXT6VBSy7zjZ3A3UY4tmOPjTAs29t4dWQu2vpwyua7niJ7iyCeZJQaIVZ/xwdy/JAQAA

````

### Deobfuscation Order (Reverse)
1. From Base64  
2. XOR with key `x`  
3. Gunzip  

<img width="1359" height="605" alt="image" src="https://github.com/user-attachments/assets/e7a3ddcb-441e-4619-8444-f65f278e19e4" />

---

## LAB TASK:  Unwrapping the Easter Egg 

1. Open **SantaStealer.ps1** on the VM Desktop.
2. Review the **“Start here”** section.
3. Follow the comments to deobfuscate the C2 URL.

<img width="1365" height="553" alt="image" src="https://github.com/user-attachments/assets/3deb32bb-2a88-41a3-82de-16740813b76c" />


<img width="1364" height="436" alt="image" src="https://github.com/user-attachments/assets/a537dcb3-c61c-4e15-9f0d-da0d372c7fa3" />

<img width="867" height="148" alt="image" src="https://github.com/user-attachments/assets/57fd64cf-f527-419b-b307-28229748b2b8" />


5. Run the script:

```powershell
cd .\Desktop\
.\SantaStealer.ps1
````
<img width="683" height="463" alt="image" src="https://github.com/user-attachments/assets/8ae0d303-f501-4b29-86bc-200bbed6df57" />

### Part 2

* Obfuscate the API key using XOR as instructed in the script.
* Run the script again to retrieve the final flag.

---
<img width="1139" height="219" alt="image" src="https://github.com/user-attachments/assets/19bc3c80-f84b-424f-9b4c-0300289670ff" />


<img width="1365" height="421" alt="image" src="https://github.com/user-attachments/assets/c6b6ce4b-67b3-4e14-a317-e5113019f646" />

<img width="972" height="134" alt="image" src="https://github.com/user-attachments/assets/7cbff396-e6c2-4808-a26e-eb6a738b4966" />



## Further Reading

* TryHackMe: **Obfuscation Principles** room



