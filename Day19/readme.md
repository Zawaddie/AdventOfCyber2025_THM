# DAY 19:


---

# What is SCADA?

SCADA **(Supervisory Control and Data Acquisition)** systems are the **command centres of industrial operations**. They bridge human operators and machines, monitoring processes and sending control commands.

At **TBFC**, SCADA manages the entire **drone delivery operation**—monitoring drones, routing packages, and ensuring deliveries reach the correct destinations.

---

## Core Components of a SCADA System

### 1. Sensors & Actuators
- **Sensors**: Measure real-world data (temperature, weight, position).
- **Actuators**: Perform actions (motors, valves, robotic arms).
- Example: Sensors detect packages on a conveyor; actuators load drones.

### 2. PLCs (Programmable Logic Controllers)
- Execute automation logic.
- Read sensor data and control actuators.
- Example: *If package = chocolate egg and zone = 5 → load Drone 7.*

### 3. Monitoring Systems
- Interfaces like dashboards, alarms, and CCTV.
- Provide real-time visibility of operations.
- Example: TBFC CCTV feeds show the warehouse floor live.

### 4. Historians
- Databases that store operational data.
- Used for troubleshooting, trend analysis, and incident response.

---

## SCADA in TBFC’s Drone Delivery System

The compromised SCADA system controls:

- **Package selection** – Chooses gift types via numeric control values.
- **Delivery routing** – Zones 1–9 for delivery, Zone 10 for disposal.
- **Visual monitoring** – CCTV for real-time observation.
- **Inventory checks** – Verifies stock before loading.
- **Security protections** – Prevent unauthorised changes (misused by the attacker).
- **Audit logging** – Tracks logins and configuration changes (disabled by attacker).

---

## Why SCADA Systems Are Targeted

SCADA and other ICS/OT systems are high-value targets because:

- **Legacy software** with known vulnerabilities.
- **Default credentials** often left unchanged.
- **Designed for reliability, not security**.
- **Control physical processes**, enabling real-world impact.
- **Connected to corporate networks**, increasing attack surface.
- **Insecure protocols** (e.g., Modbus has no authentication).

---

## Modbus & Real-World Attacks

- **Modbus TCP** allows read/write access without authentication.
- Commonly exposed on **TCP port 502**.
- In early 2024, **FrostyGoop**, the first ICS/OT malware, exploited Modbus TCP to manipulate industrial systems.

King Malhare uses similar tactics—manipulating SCADA via Modbus to sabotage Christmas deliveries.

---

## Question

**What port is commonly used by Modbus TCP?**

➡️ **502**

---


# What is a PLC?

A **PLC (Programmable Logic Controller)** is an industrial computer used to control machinery and physical processes. Unlike general-purpose computers, PLCs are built for **reliability, real-time control, and harsh environments**.

## Key PLC Characteristics
- **Harsh environment tolerant** – Operates under extreme temperatures, vibration, dust, moisture, and EMI.
- **Always-on operation** – Runs 24/7 for years without rebooting.
- **Real-time control** – Responds to inputs within milliseconds.
- **Direct hardware interfacing** – Connects directly to sensors and actuators.

---

# What is Modbus?

**Modbus** is a widely used industrial communication protocol created in 1979. It enables devices like PLCs to communicate using a simple **request–response** model.

Example:
```

Client: Read register 0
PLC: Register 0 = 1

```

### Why Modbus Is Insecure
- No authentication
- No encryption
- No authorisation
- Plaintext communication

Anyone who can reach the Modbus port can read or write values.

---

## Modbus Data Types

| Type | Purpose | Values | Example |
|----|-------|-------|--------|
| **Coils** | Digital outputs | 0 / 1 | Motor on/off |
| **Discrete Inputs** | Digital inputs | 0 / 1 | Sensor triggered |
| **Holding Registers** | Analogue outputs | 0–65535 | Speed, zone |
| **Input Registers** | Analogue inputs | 0–65535 | Temperature |

> **Writable**: Coils, Holding Registers  
> **Read-only**: Discrete Inputs, Input Registers

---

## TBFC Modbus Layout

### Holding Registers
- **HR0** – Package type (0=Gifts, 1=Eggs, 2=Baskets)
- **HR1** – Delivery zone (1–9, 10=Disposal)
- **HR4** – System signature

### Coils
- **C10** – Inventory verification
- **C11** – Protection mechanism
- **C12** – Emergency dump
- **C13** – Audit logging
- **C14** – Christmas restore flag
- **C15** – Self-destruct status

> The maintenance note documented these exact addresses.

---

## Modbus Addressing

- **Zero-indexed** (starts at 0, not 1)
- Each coil/register has a unique address

Examples:
- HR0 → Package selector
- HR1 → Delivery zone
- C11 → Protection enabled
- C15 → Self-destruct

---

## Modbus TCP vs Serial

### Serial Modbus
- Uses RS-232 / RS-485
- Requires physical access

### Modbus TCP
- Runs over TCP/IP
- Default port: **502**
- Enables remote access but increases attack surface

King Malhare exploited an exposed **Modbus TCP (502)** service with no authentication.

---

## The Core Security Problem

Modbus provides:
-  No authentication
-  No encryption
-  No permissions
-  No integrity verification

Security relies on external controls (firewalls, VPNs), often missing in legacy environments.

---

## Connecting the Dots

- OpenPLC UI was bypassed
- Attacker directly modified **registers and coils**
- The warning *“Never change HR0 whilst C11=True”* hints at a trap mechanism

---

## What’s Next?

Next task: use **Python + pymodbus** to interact with the PLC exactly like the attacker—by reading and writing Modbus values directly.

