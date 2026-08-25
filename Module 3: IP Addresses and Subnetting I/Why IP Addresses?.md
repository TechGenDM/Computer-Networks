Perfect. We've completed **Module 2**, so let's start **Module 3: IP Addresses and Subnetting I**.

This module is extremely important because **IP addressing is the foundation of routing and networking**. We'll take it slowly, especially subnetting.

### 🌐 Module 3 Roadmap

1. **Why IP Addresses?**
2. **IPv4**
3. **Binary Refresher**
4. **Network ID**
5. **Host ID**
6. **Subnet Mask**
7. **CIDR Notation**
8. **Public vs Private IP**
9. **Loopback Address**

Let's start with the most fundamental question.

# 1. Why IP Addresses?

We already learned that a **MAC address** identifies a network interface on a local network.

But imagine the Internet has **billions of devices**.

We need a system that allows routers to answer:

> **"Where is this destination located, and which direction should I forward the packet?"**

That's what **IP addressing** provides.

---

## 🏙️ Think about a postal address

Imagine sending a letter:

```text
Name: Devasish
House: 25
Street: ABC Road
City: Bengaluru
Country: India
```

The postal system uses this hierarchy to get your letter to the right location.

IP addressing works similarly.

An IPv4 address might look like:

```text
192.168.1.25
```

The address contains information that can be used to determine:

> **Which network?**

and

> **Which host inside that network?**

We'll learn exactly how this division works shortly.

---

# 🔥 Why can't we just use MAC addresses?

Because MAC addresses aren't designed for **hierarchical routing across the Internet**.

Imagine every router having to maintain a table containing the MAC address of every device on Earth.

That's obviously impractical.

Instead, IP addresses provide a **hierarchical structure**.

For example:

```text
Internet
   │
   ├── Network A
   │     ├── Host 1
   │     ├── Host 2
   │     └── Host 3
   │
   └── Network B
         ├── Host 1
         └── Host 2
```

Routers can reason about **networks and prefixes**, rather than maintaining a route for every individual device.

---

# 🧠 MAC vs IP — Now properly

|              | MAC Address         | IP Address                 |
| ------------ | ------------------- | -------------------------- |
| Layer        | Layer 2             | Layer 3                    |
| Main purpose | Local/link delivery | Network-to-network routing |
| Scope        | Local link          | Across networks            |
| Used by      | Switches            | Routers                    |
| Example      | `AA:BB:CC:DD:EE:FF` | `192.168.1.25`             |

A simple mental model:

> **MAC tells the local network "which interface."**

> **IP tells the network "which destination/network."**

---

# 📦 Example: Your laptop → YouTube

Suppose:

```text
Laptop
IP: 192.168.1.10
MAC: AA:AA:AA:AA:AA:AA
```

You want to reach a YouTube server:

```text
YouTube IP: X.X.X.X
```

Your laptop creates an IP packet:

```text
Source IP      → 192.168.1.10
Destination IP → YouTube's IP
```

The packet is then carried inside an Ethernet/Wi-Fi frame for the **current local hop**.

```text
Laptop
   │
   │ IP packet
   ▼
Router
   │
   ▼
ISP
   │
   ▼
Internet
   │
   ▼
YouTube
```

The routers use the **destination IP** to decide where the packet should go next.

---

## 🎯 The key idea

If you remember only one thing from this topic:

> **IP addresses provide a logical, hierarchical addressing system that allows routers to deliver packets between different networks.**

And now we're ready for **IPv4**, where we'll take apart an address like:

`192.168.1.25`

and understand exactly what every part means.
