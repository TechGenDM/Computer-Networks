## Module 7 — Final Topic: Hop-by-Hop Routing 🚀

You already know **packet forwarding**. Now let's understand the most important idea behind how packets actually travel across the Internet.

### 1. What does “Hop-by-Hop” mean?

A **hop** = one router-to-router step.

Suppose:

```text
Your Laptop
    ↓
Router R1
    ↓
Router R2
    ↓
Router R3
    ↓
YouTube Server
```

The packet does **not** magically know:

> “I will go R1 → R2 → R3.”

Instead, each router decides **its own next step**.

```text
Laptop → R1
          ↓
        "Where should I send this next?"
          ↓
        R2
          ↓
        "Where should I send this next?"
          ↓
        R3
          ↓
       Server
```

That's **hop-by-hop routing**.

---

### 2. What does each router know?

Imagine R1 receives a packet:

```text
Destination IP = 142.250.x.x
```

R1 checks its forwarding table:

```text
142.250.0.0/16 → R2
```

So R1 says:

> “My job isn't to reach the server directly. I just need to send this packet to R2.”

Then R2 makes **its own decision**.

---

### 3. MAC changes at every hop

This is extremely important.

Suppose:

```text
Laptop → R1 → R2 → Server
```

At the first link:

```text
Source MAC = Laptop
Destination MAC = R1
```

After R1 forwards it:

```text
Source MAC = R1
Destination MAC = R2
```

After R2:

```text
Source MAC = R2
Destination MAC = Server
```

So:

**MAC addresses → change at every hop**

**Destination IP → generally remains the end destination**  
(except mechanisms such as NAT can modify IP headers).

---

### 4. What if a router doesn't know the complete path?

That's completely fine.

R1 doesn't need to know:

```text
R1 → R2 → R3 → R4 → R5 → Server
```

It only needs to know:

```text
Destination → Next Hop
```

This makes the Internet scalable.

---

### 5. What happens if a route breaks?

Imagine:

```text
Laptop → R1 → R2 → R3 → Server
                 ❌
```

If R2's route to R3 fails, a **dynamic routing protocol** may learn another path:

```text
             → R3 → Server
R2 → R4
             → R5 → Server
```

The routing system can update its routes, and packets can eventually follow the new path.

This is one reason protocols such as **OSPF** and **BGP** are so important.

---

### 6. TTL prevents packets from looping forever

Imagine a routing mistake causes:

```text
R1 → R2 → R3 → R1 → R2 → R3 → ...
```

That would be terrible.

So IPv4 packets have a **TTL (Time To Live)**.

Example:

```text
TTL = 5

R1 → TTL 4
R2 → TTL 3
R3 → TTL 2
R1 → TTL 1
R2 → TTL 0 → DROP
```

IPv6 has the equivalent field called **Hop Limit**.

This prevents packets from circulating forever.

---

## 🧠 The complete picture

When you send a packet to YouTube:

```text
Application
     ↓
TCP/UDP
     ↓
IP Packet
     ↓
Ethernet Frame
     ↓
Router
     ↓
[Routing Table]
     ↓
Next Hop
     ↓
New Ethernet Frame
     ↓
Router
     ↓
Next Hop
     ↓
...
     ↓
YouTube
```

### The golden rule:

> **Routing decides where the packet should go; forwarding sends it to the next hop.**

And:

> **The Internet moves packets hop-by-hop, with each router making the next forwarding decision.**