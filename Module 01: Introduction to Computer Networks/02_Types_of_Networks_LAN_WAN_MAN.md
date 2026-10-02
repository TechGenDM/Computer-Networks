<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 01: Introduction to Computer Networks* • **Topic 02 of 09** (Global #002)

| [⬅️ Previous: Why Computer Networks?](./01_Why_Computer_Networks.md) | 📑 [**Module Overview**](./README.md) | [Next: Internet Architecture ➡️](./03_Internet_Architecture.md) |
| :--- | :---: | ---: |

---

</div>

# 2. Types of Computer Networks

Networks are classified based on **geographical coverage**.

---

## Three Main Types

```
LAN  →  Local Area Network       (Small)
MAN  →  Metropolitan Area Network (Medium)  
WAN  →  Wide Area Network        (Large)
```

### Easy Memory Trick:
- **L** → **Local** → Small
- **M** → **Metropolitan** → City  
- **W** → **Wide** → Very large

---

## ① LAN — Local Area Network

A **LAN** covers a **small geographical area**.

**Characteristics:**
- Fast speeds
- Privately managed
- Low latency
- Limited to one location

**Examples:**
- Your home Wi-Fi
- College computer lab
- Office network
- Hostel network

### Visual Example:

```
        LAN
 ┌─────────────────┐
 │                 │
 │ Laptop ─┐       │
 │ Phone ──┼─ Wi-Fi│
 │ PC ─────┘       │
 │      Router     │
 └─────────────────┘
```

**Real Example:**
Your home setup:
```
Laptop
   │
Phone ──► Wi-Fi Router
   │
TV
```

All devices communicate through your local network.

---

## ② MAN — Metropolitan Area Network

A **MAN** covers a **larger area than a LAN**, typically a city or metropolitan region.

Think of it as **multiple LANs connected across a city**.

```
LAN ─────┐
         │
LAN ─────┼────► MAN
         │
LAN ─────┘
```

**Example:**
A company has offices in different parts of Bengaluru and connects those offices through a metropolitan network.

**Simple way to remember:**
```
MAN = Network across a city
```

---

## ③ WAN — Wide Area Network

A **WAN** covers a **very large geographical area**.

It can connect:
- Cities
- States
- Countries
- Continents

```
LAN
 │
 ▼
Bengaluru ─────── Mumbai
     │                │
     └──── WAN ───────┘
              │
              ▼
           London
```

The **biggest example is the Internet itself**.

⚠️ **Important:**
The Internet is **not technically just "one WAN."**
It is a **global network of interconnected networks.**

---

## 🔥 Comparison Table

| Network | Coverage | Example | Speed |
|---------|----------|---------|-------|
| **LAN** | Small area (single building/home) | Home, office | Usually fast |
| **MAN** | City or metropolitan region | City-wide company network | Medium |
| **WAN** | Large geographical area (states/countries) | Company across countries | Variable |
| **Internet** | Global (all countries/continents) | Worldwide interconnected networks | Varies widely |

---

## ⚠️ Important Distinction

**Don't confuse network SIZE with network SPEED.**

```
LAN is often faster than WAN, BUT:

LAN ≠ always fast
WAN ≠ always slow
```

**Actual performance depends on:**
- Technologies used
- Link quality
- Network congestion
- Distance involved
- Infrastructure quality

---

## 🧠 Real-World Picture

Suppose you're sitting at home and accessing your company's server:

```
Your Laptop
     ↓
 Home LAN
     ↓
   Router
     ↓
    ISP
     ↓
    WAN
     ↓
Company Network
     ↓
Company Server
```

**Key insight:** One request can travel through multiple types of networks.

---

## 🎯 Quick Check

**Question:** If you have:
```
Your laptop → home Wi-Fi router → YouTube
```

**Which part is the LAN, and which is the WAN/Internet?**

💭 Think about it before moving to Internet Architecture.

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *Why does a LAN often achieve lower latency and higher throughput than a WAN?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> LANs span short geographical distances under private administrative control with dedicated high-speed infrastructure (e.g. Cat6/Fiber switches), whereas WANs cross public ISP backbones, long-distance fiber, and multiple routing hops.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: Why Computer Networks?](./01_Why_Computer_Networks.md) | [**Module 01: Introduction to Computer Networks**](./README.md) | [Next: Internet Architecture ➡️](./03_Internet_Architecture.md) |

<div align="center">
  <br/>
  <code>[░░░░░░░░░░] 1% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
