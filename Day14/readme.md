# Day 14: Containers - DoorDasher's Demise

Continue your Advent of Cyber journey and learn about container security.

## 📌 Room Overview

This room introduces containerisation, explains how containers differ from virtual machines, and demonstrates a Docker container escape leading to recovery of a defaced web service.

---

It seemed as the sun rose this morning, it had already been decided that today would be another day of chaos in Wareville. At least that’s the feeling all the folks at “DoorDasher” got. DoorDasher is Warevilles local food delivery site, a favourite of the workers in The Best Festival Company, especially on long days when they get home from work and just can’t bring themself to make dinner. We’ve all been there, I’m sure.

Well, one Wareville resident was feeling particularly tired this morning and so decided to order breakfast. Only to find King Malhare and his bandit bunny battalions had seized control of yet another festive favourite. DoorDasher had been completely rebranded as Hopperoo. All of the ware’s favourite dishes had been changed as well. Reports started flooding into the DoorDasher call centre. And not just from customers. The health and safety food org was on the line too, utterly panicked. Apparently, multiple Wareville residents were choking on what turned out to be fragments of Santa’s beard. Wareville authorities were left tangled in confusion today as Hopperoo faced mounting backlash over reports of “culinary impersonation.” Customers across the region claim to have been served what appears to be authentic strands of Santa’s beard in place of traditional noodles.

A spokesperson for the Health & Safety Food Bureau confirmed that several diners required “gentle untangling” and one incident involved a customer “achieving accidental facial hair synchronisation.”

Beardgate news coverage

Immediately, one of the security engineers managed to log on and make a script that would restore DoorDasher to its original state, but just before he was able to run it, Sir CarrotBaine caught wind of his attempt and locked him out of the system. All was lost, until the SOC team realised they still had access to the system via their monitoring pod, an uptime checker for the site. Your job? As a SOC team member of DoorDasher, can you escape the container and escalate your privileges so you can finish what your team started and save the site!

## Learning Objectives

- Learn how containers and Docker work, including images, layers, and the container engine
-  Explore Docker runtime concepts (sockets, daemon API) and common container escape/privilege-escalation vectors
- Apply these skills to investigate image layers, escape a container, escalate privileges, and restore the DoorDasher service
- DO NOT order “Santa's Beard Pasta”

---

## 🧠 What Are Containers?
Modern applications often face challenges such as:

- **Installation issues** due to environment differences
- **Troubleshooting difficulties** caused by dependency mismatches
- **Software conflicts** (e.g., multiple Python versions)


✅ **Solution: Containerisation**
Containers solve these problems by packaging applications and all dependencies into a single isolated environment. This ensures consistent behavior across systems while remaining lightweight and portable.

## ⚖️ Containers vs Virtual Machines

| Feature      | Virtual Machines            | Containers                     |
|-------------|-----------------------------|--------------------------------|
| OS          | Full guest OS per VM         | Share host OS kernel            |
| Performance | Heavy, slower startup        | Lightweight, fast startup       |
| Isolation   | Strong (OS-level)            | Process-level                   |
| Use Case    | Multiple OSs, legacy apps    | Scalable microservices          |

➡️ Containers are ideal for modern, scalable applications.

---

## 📈 Applications at Scale (Microservices)

- Traditional apps were monolithic (single codebase)
- Modern apps are broken into microservices
- Each service can scale independently
- Containers are ideal due to their low overhead and fast deployment

---

## 🛠️ Container Engines & Docker
A container engine:
- Builds, runs, and manages containers
- Uses OS features like namespaces and cgroups


### 🐋 Docker
Docker is a popular open-source container engine that:
- Uses Dockerfiles to define environments
- Ensures consistency across deployments
- Is the engine used by DoorDasher in this lab

---

## 🚨 Container Escape & Docker Sockets

**What Is a Container Escape?**
A **container escape** occurs when code inside a container gains access to:
- The host system
- Other containers
- Privileged resources


**Docker Socket Abuse**

- Docker uses a client-server model
- The Docker daemon exposes an API via:

```arduino
/var/run/docker.sock
```

- If a container can access this socket, it can:
  - Control Docker
  - Spawn privileged containers
  - Escape isolation



---

## practical

### 🎯 Challenge Objective
Restore the defaced DoorDasher website (now showing Hopperoo) by exploiting Docker misconfigurations and recovering the original service.

🔍 **Enumeration**
List Running Containers

```bash
docker ps
```

Main service:

``cpp
http://MACHINE_IP:5001
```

➡️ Website is defaced. Time to investigate backend containers.

---

### 🔓 Docker Socket Access

**Access the uptime-checker container** 

```bash
docker exec -it uptime-checker sh
```

**Check Docker socket permissions**

```bash
ls -la /var/run/docker.sock
```

➡️ Socket is accessible → **Docker Escape possible**

**Confirm Docker access from inside container**

```bash
docker ps
```

✔️ Successful execution confirms API access to Docker daemon

---

## 🚀 Privileged Container Access

**Enter the deployer container**

```bash
docker exec -it deployer bash
```

Check privileges:

```bash
whoami
```

➡️ Privileged container confirmed

---

## 🛠️ Service Recovery
Inside the deployer container:

**Run recovery script**

```bash
sudo /recovery_script.sh
```

**Verify restoration**
Refresh:

```cpp
http://MACHINE_IP:5001
```

✔️ DoorDasher service restored

🏁 Flag
The flag is located in the root directory:

```bash
cat /flag
```


## ✅ Key Takeaways
- Containers are lightweight alternatives to VMs
- Docker socket exposure is extremely dangerous
- Misconfigured containers can lead to full host compromise
- Container escapes are a real-world attack vector



📂 Room completed — service restored and flag captured!

