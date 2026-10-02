<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 12: TCP, UDP and Socket Programming II* • **Topic 01 of 11** (Global #079)

| [⬅️ Prev Module: 🌊 TCP Flow Control](../Module%2011%3A%20TCP%2C%20UDP%20and%20Socket%20Programming/08_Flow_Control.md) | 📑 [**Module Overview**](./README.md) | [Next: 🔌 Socket Programming Concepts ➡️](./02_Socket_Programming_Concepts.md) |
| :--- | :---: | ---: |

---

</div>

# 🪟 Topic 1 — Sliding Window (Conceptual)

The **sliding window** is a TCP mechanism that allows the sender to have **multiple bytes of data in flight** before waiting for individual acknowledgements.

Instead of:

```text
Send → ACK → Send → ACK → Send → ACK
```

TCP can do:

```text
Send ────────→
Send ────────→
Send ────────→
Send ────────→
     ← ACKs
```

This improves network utilization.

### Simple example

Suppose the sender is allowed to have:

```text
4 segments
```

outstanding.

```text
[1][2][3][4]  ← window
```

It can send all four.

When the receiver acknowledges the first two:

```text
[1][2][3][4]
 ↑  ↑
 ACK
```

the window can move forward:

```text
       [3][4][5][6]
        ← window →
```

Hence the name:

> **Sliding Window** — the allowed range of unacknowledged data moves forward as acknowledgements arrive.

---

## Why is it useful?

Because waiting for an ACK after every single segment would waste time, especially on high-latency networks.

The sender can keep several segments **in flight** at once.

```text
Sender
  │
  ├── Segment 1 ──→
  ├── Segment 2 ──→
  ├── Segment 3 ──→
  └── Segment 4 ──→
                     ↓
                  Receiver
```

So:

> **Sliding window improves throughput while also helping TCP control how much unacknowledged data is outstanding.**

---

# 🔥 Important distinction

The window concept is related to both:

```text
Flow Control → rwnd
Congestion Control → cwnd
```

and TCP's effective sending window is conceptually limited by:

```text
min(rwnd, cwnd)
```

You already learned this in the previous module.

---

## 🧠 One-line mental model

> **Sliding window = "I can send several pieces before waiting, and the allowed range moves forward as data is acknowledged."**

---

### Module 12 Progress

✅ **Sliding Window (Conceptual)**  
⬜ Socket Programming Concepts  
⬜ Client-Server Model  
⬜ ARP  
⬜ NAT  
⬜ PAT  
⬜ DHCP  
⬜ Lease Process  
⬜ Default Gateway

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What is the Bandwidth-Delay Product (BDP), and what does it represent?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> $\text{BDP} = \text{Bandwidth} \times \text{Round-Trip Time}$. It represents the total volume of in-flight data required to saturate the network pipe without stalling.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Prev Module: 🌊 TCP Flow Control](../Module%2011%3A%20TCP%2C%20UDP%20and%20Socket%20Programming/08_Flow_Control.md) | [**Module 12: TCP, UDP and Socket Programming II**](./README.md) | [Next: 🔌 Socket Programming Concepts ➡️](./02_Socket_Programming_Concepts.md) |

<div align="center">
  <br/>
  <code>[███████░░░] 70% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
