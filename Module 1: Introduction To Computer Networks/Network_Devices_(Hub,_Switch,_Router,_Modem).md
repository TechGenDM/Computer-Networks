# 8. Network Devices

Now let's understand the **hardware that makes networking possible**.

The four important devices are:
1. **Hub**
2. **Switch**
3. **Router**
4. **Modem**

---

## ① Hub

A **hub** is the **simplest device**.

When it receives data, it sends it to **every connected device**.

```
       Hub
     /  |  \
    PC1 PC2 PC3

PC1 sends → Hub → PC2 + PC3 + (everyone else)
```

**Problem:** If 10 devices are connected, everyone receives the transmission.

### Why it's bad:
- Creates unnecessary traffic
- Causes collisions
- Inefficient bandwidth use

> **Hub = Broadcast everything to everyone**

💡 Hubs are **largely obsolete** in modern Ethernet networks.

---

## ② Switch

A **switch** is **much smarter than a hub**.

It learns the **MAC addresses** of devices connected to its ports.

```
        Switch
      /   |   \
    PC1  PC2  PC3
```

### How it works:

If PC1 wants to send something to PC3:
```
PC1 → Switch → PC3 (not PC2)
```

The switch forwards the frame **only toward the appropriate port** instead of broadcasting everywhere.

### Key Ideas:

> **Switch = Connects devices within a LAN**

It primarily operates at **Layer 2 (Data Link Layer)**.

**Advantages:**
- Reduces unnecessary traffic
- More efficient than hubs
- Learns MAC addresses automatically
- Forward frames intelligently

---

## ③ Router

Now we move **outside the LAN**.

A **router connects different networks**.

```
Home LAN (192.168.x.x)
   │
   ▼
Router ← Gateway
   │
   ▼
ISP Network
   │
   ▼
Internet
```

### How it works:

Your router decides where packets should go **based on IP addressing and routing information**.

```
Packet with destination IP: 8.8.8.8
      │
      ▼
   Router
      │
   (Looks up routing table)
      │
      ▼
   Decides next hop
      │
      ▼
   Forwards packet
```

### Key Ideas:

| Device | Primary Role |
|--------|--------------|
| **Switch** | Connects devices/networks within a LAN (Layer 2) |
| **Router** | Connects different networks (Layer 3) |

---

## ④ Modem

**Modem** stands for: **Modulator-Demodulator**

**Its job:** Convert between:
- Signaling used by your **local digital equipment**
- Signaling/technology used by the **ISP access connection**

```
Home Network
     │
     ▼
  Router
     │
     ▼
 Modem / ONT  ← Interfaces with ISP
     │
     ▼
    ISP
```

### Modern Setup:

Today, home equipment often **combines several functions into one box**.

Your ISP-provided "Wi-Fi router" may actually contain:

- **Router** (Layer 3 routing)
- **Switch** (Layer 2 switching)
- **Wi-Fi access point** (wireless transmission)
- **Modem/ONT** (ISP interface)

All in one device! 📦

---

## 🔥 Quick Comparison

| Device | Main Job | Operates at | Range |
|--------|----------|-------------|-------|
| **Hub** | Broadcast data to all ports | Layer 1 (Physical) | LAN only |
| **Switch** | Forward frames to correct port | Layer 2 (Data Link) | LAN only |
| **Router** | Forward packets between networks | Layer 3 (Network) | WANs & Internet |
| **Modem** | Interface with ISP technology | Layer 1 (Physical) | ISP link |

### Simple Device Chain:

```
Devices ──► Switch ──► Router ──► Modem ──► ISP ──► Internet
           (LAN)       (LAN→WAN)  (ISP link)
```

---

## 🧠 Important Distinction

People often call the box at home **"the router."**

**But technically**, that device may be doing multiple jobs:

```
        ISP
         │
     Modem/ONT
    (ISP interface)
         │
       Router
    (Route packets)
         │
       Switch
  (Connect devices)
      /      \
   Laptop    PC
```

**But your physical device combines all three** into one box! 

---

## 🎯 Quick Test

**Scenario:**
```
Laptop A ──┐
Laptop B ──┼── [ ? ] ── Internet
Phone ─────┘
```

**Question:**
Which device is primarily responsible for:

**A)** Connecting these devices together inside the LAN?
- **Answer: Switch**

**B)** Connecting that LAN to the Internet?
- **Answer: Router**

Think **Switch vs Router**.

---

## Key Takeaways

1. **Hub** = Broadcast (outdated)
2. **Switch** = Smart local network device (Layer 2)
3. **Router** = Connects different networks (Layer 3)
4. **Modem** = ISP interface device
5. **Modern boxes combine all of them**

---

**Next:** We finish this introductory module with **OSI vs TCP/IP**, which gives us the framework for understanding how all these networking concepts fit together.
