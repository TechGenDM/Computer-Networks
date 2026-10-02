<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 02: Network Packets and Layered Communication* • **Topic 10 of 10** (Global #019)

| [⬅️ Previous: IP Fragmentation 📦✂️](./09_Fragmentation.md) | 📑 [**Module Overview**](./README.md) | [Next Module: Why IP Addresses? ➡️](../Module%2003%3A%20IP%20Addresses%20and%20Subnetting%20I/01_Why_IP_Addresses.md) |
| :--- | :---: | ---: |

---

</div>

# 9. MAC Addresses 🔗

Now let's understand one of the most important concepts at the **Data Link Layer**.

## What is a MAC address?

**MAC = Media Access Control address**

It's an address used to identify a **network interface on a local network**.

A typical MAC address looks like:

```text
3C:52:82:AB:19:F4
```

It is usually represented as **48 bits (6 bytes)** in Ethernet.

```text
3C : 52 : 82 : AB : 19 : F4
 └───────────────┬───────────────┘
              48 bits
```

---

# 🆚 MAC vs IP

This is extremely important.

### MAC address

Used for **local/link-level communication**.

```text
Laptop ─────► Home Router
```

### IP address

Used for **communication across networks**.

```text
Laptop ──► ISP ──► Internet ──► YouTube
```

Think:

> **MAC = Who is this interface on this local link?**

> **IP = Where is this device/network in the larger network?**

---

# 🔄 What happens when you access YouTube?

Suppose:

```text
Your Laptop
IP: 192.168.1.10
MAC: AA:AA:AA:AA:AA:AA
```

Your home router:

```text
IP: 192.168.1.1
MAC: BB:BB:BB:BB:BB:BB
```

Your laptop wants to send something outside the local network.

The Ethernet frame on the local link might look conceptually like:

```text
┌─────────────────────────────────────┐
│ Dest MAC: BB:BB:BB:BB:BB:BB         │
│ Source MAC: AA:AA:AA:AA:AA:AA       │
│                                     │
│ IP Packet                           │
│ Source IP: 192.168.1.10             │
│ Destination IP: YouTube             │
└─────────────────────────────────────┘
```

Notice something interesting:

### MAC destination

```text
Router's MAC
```

### IP destination

```text
Remote server's IP
```

Your laptop doesn't need YouTube's MAC address.

Why?

Because YouTube isn't on your **local network**.

---

# 🧠 This is the key idea

Suppose the journey is:

```text
Laptop
  ↓
Router
  ↓
ISP Router
  ↓
Internet Router
  ↓
YouTube
```

At every hop, the **Layer 2 MAC addresses are relevant to that local link**.

Conceptually:

```text
Hop 1:

MAC A ─────► MAC B
IP X  ─────────────────────► IP Y


Hop 2:

MAC C ─────► MAC D
IP X  ─────────────────────► IP Y


Hop 3:

MAC E ─────► MAC F
IP X  ─────────────────────► IP Y
```

So:

> **MAC addresses change hop-by-hop.**

> **IP addresses provide the end-to-end network addressing.**

There are nuances such as NAT and IP header fields changing, but this is the core model.

---

# 🔍 How does the laptop know the router's MAC?

This is where **ARP** comes in for IPv4.

Your laptop knows:

```text
Router IP = 192.168.1.1
```

But it needs:

```text
Router MAC = ?
```

It can use **ARP — Address Resolution Protocol**.

Conceptually:

```text
Laptop:
"Who has 192.168.1.1?"

        ↓

Router:
"That's me.
My MAC is BB:BB:BB:BB:BB:BB."
```

Then the laptop can construct the Ethernet frame.

For IPv6, **Neighbor Discovery (ND)** performs the corresponding address-resolution function rather than ARP.

---

# 🔥 MAC Address + Switch

Now you can understand how a switch works.

Suppose:

```text
PC1 ──┐
PC2 ──┼── Switch
PC3 ──┘
```

The switch learns which MAC address is reachable through which port.

Conceptually:

| MAC Address | Switch Port |
|---|---|
| AA:AA:AA... | Port 1 |
| BB:BB:BB... | Port 2 |
| CC:CC:CC... | Port 3 |

If PC1 sends a frame to PC3:

```text
PC1
 ↓
Switch
 ↓
Port 3
 ↓
PC3
```

The switch doesn't need to send the frame to everyone when it knows the destination's port.

---

# 🎯 Final mental model

```text
MAC
 ↓
Layer 2
 ↓
Local/link delivery
 ↓
Switch
```

```text
IP
 ↓
Layer 3
 ↓
Network-to-network delivery
 ↓
Router
```

And the relationship:

```text
Application Data
      ↓
TCP Segment
      ↓
IP Packet
      ↓
Ethernet Frame
      ↓
MAC addresses handle the local hop
```

That completes the major concepts in this module.

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *Why can't the global Internet route packets using physical MAC addresses alone?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> MAC addresses are flat and geographically random (tied to NIC manufacturers), requiring global routers to store billions of unaggregatable entries. IP addresses are hierarchical, allowing millions of devices to be represented by a single summary route.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: IP Fragmentation 📦✂️](./09_Fragmentation.md) | [**Module 02: Network Packets and Layered Communication**](./README.md) | [Next Module: Why IP Addresses? ➡️](../Module%2003%3A%20IP%20Addresses%20and%20Subnetting%20I/01_Why_IP_Addresses.md) |

<div align="center">
  <br/>
  <code>[█░░░░░░░░░] 16% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
