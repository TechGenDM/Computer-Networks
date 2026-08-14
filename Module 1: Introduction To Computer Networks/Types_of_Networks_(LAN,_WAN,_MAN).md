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

**Next:** We'll explore **Internet Architecture** to understand how your device connects to YouTube's servers across the globe.
