<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 09: Routing Protocols* • **Topic 01 of 03** (Global #055)

| [⬅️ Prev Module: 🔁 Module 8 — Final Topic: Routing Loops](../Module%2008%3A%20Routing%20and%20Forwarding%20II/07_Routing_Loops.md) | 📑 [**Module Overview**](./README.md) | [Next: 🌐 OSPF — Open Shortest Path First ➡️](./02_OSPF.md) |
| :--- | :---: | ---: |

---

</div>

# 1. RIP — Routing Information Protocol 🚦

## First: What is a Routing Protocol?

A **routing protocol** is a set of rules that routers use to **exchange routing information and learn which paths to use**.

Without routing protocols, routers would have to rely entirely on manually configured routes.

Think:

```text
Router A
   │
   │ "What networks can you reach?"
   ↓
Router B
   │
   │ "I can reach 10.0.0.0/24 through me."
   ↓
Router A learns route
```

So:

> **Routing protocol = how routers learn and exchange routes.**

---

# The 3 protocols in this module

They solve routing at different scales and use different approaches:

| Protocol | Type | Main idea | Typical use |
|---|---|---|---|
| **RIP** | Distance Vector | "How many hops?" | Small/simple networks |
| **OSPF** | Link State | "Give me the topology; I'll calculate paths." | Inside an organization/AS |
| **BGP** | Path Vector | "Which networks can you reach, and under what policy/path?" | Between autonomous systems / Internet |

The most important mental model:

```text
RIP   → Hop count
OSPF  → Network map + shortest path
BGP   → Paths + routing policy
```

---

# 1️⃣ RIP — Routing Information Protocol

RIP is a **distance-vector routing protocol**.

A router basically tells its neighbors:

> **"Here are the networks I can reach, and how many hops away they are."**

Example:

```text
A ── B ── C
```

If B tells A:

```text
C = 1 hop away from B
```

A calculates:

```text
A → B → C
    1 + 1 = 2 hops
```

So A records:

```text
Destination    Metric
C              2 hops
```

---

## RIP's metric

RIP primarily uses:

> **Hop count**

For example:

```text
A ─ B ─ C ─ D
```

From A:

```text
D = 3 hops
```

It doesn't primarily care that one link is 10 Gbps and another is 100 Mbps.

That's one of RIP's major limitations.

---

## RIP's maximum hop count

This is important:

```text
1–15 → reachable
16   → unreachable
```

So RIP is unsuitable for large networks.

---

## RIP and Bellman-Ford

RIP is based on the **distance-vector/Bellman-Ford concept** we studied earlier.

The basic calculation is:

```text
Cost through neighbor
=
cost to neighbor
+
neighbor's advertised cost
```

Then choose the minimum.

---

## RIP example

```text
A ─1─ B ─1─ C
```

Initially A knows:

```text
A → A = 0
A → B = 1
A → C = ∞
```

B advertises:

```text
C = 1
```

A calculates:

```text
A → B → C
= 1 + 1
= 2
```

Therefore:

```text
A → C = 2 hops via B
```

---

# 🧠 RIP mental model

Imagine asking your neighbor:

> **"How far is the restaurant?"**

They say:

> "It's 3 streets from me."

You are one street from them, so:

```text
1 + 3 = 4 streets
```

That's essentially the distance-vector idea.

---

## 🔥 RIP in one sentence

> **RIP chooses routes primarily using hop count and learns routing information from neighboring routers.**

---

### Module progress

✅ Routing Protocols — Introduction  
✅ RIP  

⬜ OSPF  
⬜ BGP (High Level)

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What happens when a RIP route's Invalid Timer (180 seconds) expires without an update?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> The router marks the route as unreachable by setting its metric to 16, and begins the Flush Timer (typically 240 seconds) to remove it from the table.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Prev Module: 🔁 Module 8 — Final Topic: Routing Loops](../Module%2008%3A%20Routing%20and%20Forwarding%20II/07_Routing_Loops.md) | [**Module 09: Routing Protocols**](./README.md) | [Next: 🌐 OSPF — Open Shortest Path First ➡️](./02_OSPF.md) |

<div align="center">
  <br/>
  <code>[████░░░░░░] 49% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
