<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 07: Routing and Forwarding I* • **Topic 01 of 07** (Global #041)

| [⬅️ Prev Module: Why Routing Uses Graphs? 🌐🕸️](../Module%2006%3A%20Graph%20Algorithms%20for%20Networking%20II/03_Why_Routing_Uses_Graphs.md) | 📑 [**Module Overview**](./README.md) | [Next: Routing Table 🗺️ ➡️](./02_Routing_Table.md) |
| :--- | :---: | ---: |

---

</div>

# 1. Router

We already introduced routers, but now let's understand what a router **actually does**.

A **router is a Layer 3 device that connects different IP networks and forwards packets between them.**

Imagine:

```text
Network A                  Network B
192.168.1.0/24             10.0.0.0/24

   PC ──► Router ◄──────────► Server
```

The router has interfaces connected to different networks:

```text
             Router
          ┌───────────┐
Network A │ Interface │
─────────►│           │
          │ Interface │◄──────── Network B
          └───────────┘
```

It examines the **destination IP address** of an incoming packet and determines the appropriate next step.

---

## 🧠 What does a router NOT do?

A router doesn't normally ask:

> "Where is the exact computer physically?"

Instead, it looks at the **destination IP** and its routing information.

For example:

```text
Destination IP:
10.0.0.25
```

The router checks:

> "Which route matches `10.0.0.25`?"

Then:

> "Which interface/next hop should I use?"

Then it forwards the packet.

---

# 🔥 Routing vs Forwarding

This distinction is extremely important.

### Routing

**Routing = determining the path/route.**

Example:

```text
A → B → D → Server
```

It involves building or selecting routes.

### Forwarding

**Forwarding = moving an individual packet to the appropriate next hop/interface.**

```text
Packet arrives
      ↓
Look at destination IP
      ↓
Find matching route
      ↓
Select interface
      ↓
Send packet
```

So:

> **Routing = control/decision process**

> **Forwarding = actual packet movement**

We'll keep coming back to this distinction.

---

## 🌐 Example

Suppose:

```text
Laptop
192.168.1.10
    │
    ▼
Router
    │
    ▼
Internet
```

You send a packet to:

```text
8.8.8.8
```

The router sees:

```text
Destination = 8.8.8.8
```

It consults its routing table and determines something like:

```text
"Send this toward my ISP."
```

Then it forwards the packet.

---

# 🧩 A router can have multiple interfaces

For example:

```text
                 Router
              ┌──────────┐
192.168.1.0 ──┤ Interface1
              │
10.0.0.0 ─────┤ Interface2
              │
172.16.0.0 ───┤ Interface3
              └──────────┘
```

The router acts as the connection point between these different networks.

---

# 🎯 Mental model

Think of a router like a **postal sorting center**:

```text
Packet arrives
     ↓
Read destination
     ↓
Check routing information
     ↓
Choose next direction
     ↓
Forward packet
```

But there's an important difference:

> A router usually doesn't determine the entire end-to-end path for every packet by itself.

It normally makes a **next-hop decision** based on its routing table.

That leads directly to our next topic:

# **Routing Table**

We'll open up the router's "map" and see exactly what information it stores to make these decisions.

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What is the fundamental architectural distinction between the Routing Control Plane and the Data Forwarding Plane?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> The **Control Plane** (software/brain) computes path tables and runs routing protocols (OSPF, BGP). The **Data Plane** (hardware/muscle) inspects arriving packet headers and switches them to the output port in nanoseconds.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Prev Module: Why Routing Uses Graphs? 🌐🕸️](../Module%2006%3A%20Graph%20Algorithms%20for%20Networking%20II/03_Why_Routing_Uses_Graphs.md) | [**Module 07: Routing and Forwarding I**](./README.md) | [Next: Routing Table 🗺️ ➡️](./02_Routing_Table.md) |

<div align="center">
  <br/>
  <code>[███░░░░░░░] 36% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
