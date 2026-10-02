<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 07: Routing and Forwarding I* • **Topic 05 of 07** (Global #045)

| [⬅️ Previous: Static Routing 🛣️](./04_Static_Routing.md) | 📑 [**Module Overview**](./README.md) | [Next: Packet Forwarding 📦➡️ ➡️](./06_Packet_Forwarding.md) |
| :--- | :---: | ---: |

---

</div>

# 5. Default Route `0.0.0.0/0` 🌍

We already saw this briefly with **Longest Prefix Match**. Now let's understand it properly.

## What is a Default Route?

A **default route** is the route a router uses when **no more specific route matches the destination**.

For IPv4, it's:

```text
0.0.0.0/0
```

Think of it as:

> **"I don't know exactly where this destination is, so send it here."**

---

## 🏠 Your home network example

Your laptop might have:

```text
IP:              192.168.1.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.1.1
```

Your home router might have a routing table conceptually like:

```text
Destination       Next Hop
192.168.1.0/24    Directly connected
0.0.0.0/0         ISP
```

Now you access:

```text
YouTube's IP
```

The router probably doesn't have a specific route for that individual destination.

So:

```text
Destination
     ↓
No specific route?
     ↓
Use 0.0.0.0/0
     ↓
Send toward ISP
```

---

# 🧠 Why does `/0` match everything?

Remember:

```text
IPv4 = 32 bits
```

`/0` means:

```text
Network bits = 0
Host bits    = 32
```

There are **zero required matching prefix bits**.

Therefore:

```text
0.0.0.0/0
```

can match any IPv4 destination.

For example:

```text
8.8.8.8       ✅
10.20.30.40   ✅
172.16.5.10   ✅
192.168.1.50  ✅
```

But remember:

> **It is only selected if a more specific matching route isn't preferred.**

---

# 🔥 Longest Prefix Match + Default Route

Suppose a router has:

```text
10.0.0.0/8       → R1
10.20.0.0/16     → R2
10.20.30.0/24    → R3
0.0.0.0/0        → ISP
```

Destination:

```text
10.20.30.40
```

All four technically match.

But:

```text
/24 > /16 > /8 > /0
```

So:

```text
10.20.30.0/24 → R3
```

wins.

Now destination:

```text
8.8.8.8
```

None of the `10.x.x.x` routes match.

So:

```text
0.0.0.0/0 → ISP
```

wins.

---

# 🛣️ Simple analogy

Imagine you're driving.

You know:

```text
"Go to Building 25, Street A"
```

You have a very specific direction → use it.

But if you don't know the specific destination:

```text
"Take the highway toward the city."
```

That's your **default route**.

---

## 🎯 Remember

```text
Specific route available?
        │
       YES
        ↓
Use most specific route
        │
       NO
        ↓
Use default route
0.0.0.0/0
```

### One-line memory:

> **Default route = "If nothing more specific matches, send it this way."**

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *Why is the default route designated as `0.0.0.0/0`?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> A mask of `/0` means zero bits must match. Because any IP address bitwise ANDed with 0 equals 0, `0.0.0.0/0` matches every possible destination IP address with the lowest possible prefix length (fallback match).
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: Static Routing 🛣️](./04_Static_Routing.md) | [**Module 07: Routing and Forwarding I**](./README.md) | [Next: Packet Forwarding 📦➡️ ➡️](./06_Packet_Forwarding.md) |

<div align="center">
  <br/>
  <code>[████░░░░░░] 40% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
