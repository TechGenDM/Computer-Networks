# 🌐 Module 8 — Topic 4: OSPF

Now we reach one of the **most important routing protocols**:

> **OSPF = Open Shortest Path First**

If RIP was:

> 🗣️ “Ask your neighbors how far destinations are.”

OSPF is:

> 🗺️ **“Build a map of the network, then calculate the shortest paths yourself.”**

---

# 1. What type of protocol is OSPF?

OSPF is a:

**Link State routing protocol**

And it uses:

**Dijkstra's Shortest Path First algorithm**

So remember:

```text
OSPF
 ↓
Link State
 ↓
Topology Map
 ↓
Dijkstra
 ↓
Shortest Paths
 ↓
Routing Table
```

---

# 2. Let's build a network

Suppose we have:

```text
             10
        A -------- B
        |          |
       5|          |2
        |          |
        C -------- D
             1
```

Each number represents an OSPF **cost**.

A wants to reach D.

There are two possible paths:

### Path 1

```text
A → B → D

10 + 2 = 12
```

### Path 2

```text
A → C → D

5 + 1 = 6
```

OSPF chooses:

```text
A → C → D
```

because:

```text
6 < 12
```

---

# 3. Where does OSPF get this information?

This is the important part.

Routers first discover their neighbors.

For example:

```text
A ─── B
│
C
```

A discovers:

```text
Neighbor B
Neighbor C
```

Then it learns information about those links.

For example:

```text
A → B = 10
A → C = 5
```

---

# 4. Link-State Advertisements (LSAs)

Routers generate **LSAs — Link-State Advertisements**.

Think of an LSA as a message saying:

> "Here are my links and their information."

For example, A might advertise:

```text
Router: A

Connected links:
B → cost 10
C → cost 5
```

Other OSPF routers receive this information.

---

# 5. Flooding

OSPF doesn't keep this information only between immediate neighbors.

It **floods** link-state information throughout the relevant OSPF area.

Imagine:

```text
A ── B ── C ── D
```

A generates an LSA.

It gets propagated:

```text
A → B → C → D
```

Eventually routers in that area can build a consistent view of the topology.

---

# 6. Link-State Database (LSDB)

All this information is stored in a:

> **Link-State Database (LSDB)**

Think of LSDB as the router's **network map**.

For example:

```text
A ──10── B
│        │
5        2
│        │
C ───1── D
```

The LSDB contains the topology information needed to represent this network.

---

# 7. Then Dijkstra does the work

Now router A has the map.

It runs **Dijkstra's algorithm**.

Starting from A:

```text
A = 0
```

Neighbors:

```text
B = 10
C = 5
```

Choose the smallest:

```text
C = 5
```

From C:

```text
C → D = 1
```

Therefore:

```text
A → C → D

5 + 1 = 6
```

D's cost becomes:

```text
6
```

So A's shortest-path tree looks roughly like:

```text
A
│
5
│
C
│
1
│
D
```

---

# 8. SPF Tree → Routing Table

Dijkstra produces the **Shortest Path Tree (SPT)**.

OSPF then uses that result to determine forwarding routes.

For example:

| Destination | Cost | Next Hop |
|---|---:|---|
| B | 10 | B |
| C | 5 | C |
| D | 6 | C |

Notice something important:

For D:

```text
Destination = D
Next Hop = C
```

The router doesn't put the entire path into the forwarding decision.

It knows:

> **"To get to D, send the packet to C."**

Then C makes the next decision.

That's exactly where OSPF connects to the **hop-by-hop forwarding** we learned earlier.

---

# 9. What exactly is "OSPF cost"?

OSPF doesn't simply use **hop count** like RIP.

It uses a **cost metric** associated with interfaces/links.

A simplified conceptual example:

```text
Fast link   → lower cost
Slow link   → higher cost
```

Therefore OSPF can distinguish between paths that have the same number of hops but different link costs.

This is a major advantage over RIP.

---

# 10. What happens when a link fails?

Suppose:

```text
A ── B
│
C ── D
```

And:

```text
C ── D ❌
```

C detects the link failure.

OSPF distributes updated link-state information.

Routers update their LSDBs.

Then they recalculate shortest paths.

Conceptually:

```text
Link failure
     ↓
Updated LSA
     ↓
Flooding
     ↓
Updated LSDB
     ↓
Dijkstra
     ↓
New routing table
```

This is why OSPF can react relatively quickly to topology changes.

---

# 11. OSPF Areas

Here's a more advanced but **very important** OSPF concept.

Imagine a huge network:

```text
                 OSPF
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
    Area 0      Area 1     Area 2
```

OSPF can divide a network into **areas**.

The central area is traditionally:

> **Area 0 — Backbone Area**

Other areas can connect through the backbone.

Why?

Because having every router maintain every detail of a gigantic network can become expensive.

Areas help improve:

- scalability
- routing-table/database management
- control of LSA flooding
- SPF calculation workload

---

# 12. OSPF vs RIP 🔥

This comparison is worth memorizing.

| Feature | RIP | OSPF |
|---|---|---|
| Type | Distance Vector | Link State |
| Metric | Hop count | Cost |
| Algorithm | Bellman-Ford concept | Dijkstra |
| Network knowledge | Neighbor information | Topology database |
| Updates | Periodic/triggered behavior | Link-state flooding |
| Convergence | Generally slower | Generally faster |
| Scalability | Smaller networks | Larger networks |
| Hierarchy | No OSPF-style areas | Supports areas |
| IPv6 | RIPng | OSPFv3 |

---

# 13. A subtle but important point

Don't say:

> ❌ "OSPF finds the physically shortest route."

Instead say:

> ✅ **"OSPF finds the lowest-cost path according to its configured OSPF metric."**

The lowest-cost path might not be geographically shortest.

For example:

```text
Mumbai ───── Delhi
   ↑            ↑
  10            5
```

A geographically longer link could still have a lower configured cost.

---

# 14. OSPF's three important databases/concepts

At a beginner level, focus on these:

### Neighbor relationship

```text
Who are my OSPF neighbors?
```

### LSDB

```text
What does my network topology look like?
```

### Routing table

```text
Which next hop should I use?
```

And the process is:

```text
Neighbors
    ↓
LSAs
    ↓
LSDB
    ↓
Dijkstra / SPF
    ↓
Routing Table
    ↓
Forwarding
```

---

# 🧠 The easiest way to remember OSPF

Imagine you're in a city.

### RIP:

You ask people:

> "How many intersections away is the airport?"

### OSPF:

Everyone gives you a road map.

You combine the information:

```text
🗺️ Network Map
      ↓
Dijkstra
      ↓
Best-cost paths
      ↓
Navigation instructions
```

---

# 🔥 OSPF Cheat Sheet

```text
OSPF
│
├── Link State
│
├── Builds topology database
│
├── Uses LSAs
│
├── Floods link-state information
│
├── Uses Dijkstra / SPF
│
├── Metric = Cost
│
├── Supports hierarchical areas
│
└── Area 0 = Backbone
```

### One-line definition:

> **OSPF is a link-state interior routing protocol that distributes link-state information, builds a topology database, and uses Dijkstra's SPF algorithm to calculate lowest-cost paths.**

### Module 8 progress

✅ Distance Vector Routing  
✅ Link State Routing  
✅ RIP  
✅ **OSPF**  
⬜ BGP (High Level)  
⬜ Convergence  
⬜ Routing Loops

**Next → BGP (High Level)** 🌍 — this is where we move from **routing inside an organization (OSPF)** to **routing between organizations/Autonomous Systems across the Internet**.