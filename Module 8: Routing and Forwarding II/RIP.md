# 🌐 Module 8 — Topic 3: RIP

**RIP = Routing Information Protocol**

Now we're going to connect everything we learned about **Distance Vector Routing** to a real protocol.

The easiest way to remember it:

> **RIP is a Distance Vector routing protocol that uses hop count as its routing metric.**

---

# 1. What is RIP?

Imagine this network:

```text
A ── B ── C ── D
```

If A wants to reach D:

```text
A → B → C → D
```

That's:

```text
3 hops
```

RIP essentially asks:

> **"How many routers/hops away is the destination?"**

The route with the lowest hop count is preferred.

---

# 2. RIP's routing table

Suppose Router A has learned:

| Destination | Metric | Next Hop |
|---|---:|---|
| A | 0 | — |
| B | 1 | B |
| C | 2 | B |
| D | 3 | B |

Here **metric = hop count**.

So:

```text
A → B = 1 hop
A → B → C = 2 hops
A → B → C → D = 3 hops
```

---

# 3. RIP uses Distance Vector

Remember our previous topic?

```text
Distance Vector
       ↓
Routers exchange information
with their neighbors
```

RIP does exactly this.

Suppose:

```text
A ── B ── C
```

Initially A knows:

```text
B = 1
C = ∞
```

B knows:

```text
A = 1
C = 1
```

B tells A:

> "C is 1 hop away from me."

A calculates:

```text
A → B = 1
B → C = 1

Total = 2
```

So A updates:

```text
C = 2 hops
Next Hop = B
```

---

# 4. Why is it called "distance vector"?

Because every router maintains something like:

```text
Destination → Distance + Direction
```

For example:

```text
C → 2 hops → through B
```

That's the **distance vector**.

---

# 5. The BIG limitation of RIP

Here's where RIP becomes less impressive.

RIP only cares about **hop count**.

Imagine:

```text
Route 1:

A → B → C → D
     3 hops
```

and:

```text
Route 2:

A → E → F → G → D
     4 hops
```

RIP chooses:

```text
Route 1
```

because:

```text
3 < 4
```

But imagine Route 1 has extremely slow links:

```text
A ──slow── B ──slow── C ──slow── D
```

while Route 2 has extremely fast links:

```text
A ──fast── E ──fast── F ──fast── G ──fast── D
```

RIP still chooses Route 1.

Why?

> **RIP doesn't measure bandwidth or latency as its primary metric. It uses hop count.**

---

# 6. RIP's maximum hop count

This is an important exam fact.

RIP supports a maximum usable metric of:

> **15 hops**

And:

> **16 = unreachable**

So:

```text
1–15 → reachable
16    → unreachable
```

This is one reason RIP is unsuitable for large networks.

---

# 7. RIP updates

Classic RIP periodically exchanges routing information with neighbors.

A simplified view:

```text
A                    B
│                    │
│  "Here are my      │
│   routes."         │
│ ─────────────────> │
│                    │
│  "Here are mine."  │
│ <───────────────── │
```

The routers use these advertisements to update their routing tables.

---

# 8. Worked example 🔥

Consider:

```text
       B
      / \
     /   \
    A     D
     \   /
      \ /
       C
```

Every link has cost **1 hop**.

A initially knows:

```text
A = 0
B = 1
C = 1
D = ∞
```

B knows:

```text
B = 0
A = 1
D = 1
```

C knows:

```text
C = 0
A = 1
D = 1
```

---

### Round 1

A receives B's information:

```text
B → D = 1
```

A reaches B in 1 hop:

```text
A → B → D

1 + 1 = 2
```

A learns:

```text
D = 2
Next Hop = B
```

A could also learn through C:

```text
A → C → D = 2
```

So there are two equal-cost paths.

Depending on implementation/configuration, RIP may support **equal-cost multipath**.

---

# 9. What happens when a route fails?

Suppose:

```text
A ── B ── D
     ❌
```

The B-D connection fails.

B detects:

```text
D = unreachable
```

It needs to communicate this information to neighboring routers.

This is where mechanisms such as **route poisoning** and **triggered updates** help speed up failure propagation.

For RIP:

```text
D = 16
```

means:

> "D is unreachable."

---

# 10. Count-to-Infinity problem

This is one of RIP's famous weaknesses.

Imagine:

```text
A ── B ── C
```

Suppose C becomes unreachable.

Because of stale information, A and B may incorrectly believe the other still has a route to C.

They could advertise:

```text
A: C = 2
B: C = 3
A: C = 4
B: C = 5
...
```

The metric keeps increasing.

This is:

> **Count-to-Infinity**

RIP defines:

```text
16 = Infinity
```

So eventually the route is declared unreachable.

---

# 11. RIP versions

You may encounter these names:

### RIPv1

Older version.

- Classful
- Doesn't carry subnet mask information in route advertisements
- Limited compared with modern protocols

### RIPv2

Improved version.

- Classless
- Supports CIDR/VLSM
- Carries subnet mask information
- Supports authentication
- Uses multicast for updates

### RIPng

RIP for **IPv6**.

---

# 12. RIP vs OSPF

This is extremely important.

| | RIP | OSPF |
|---|---|---|
| Type | Distance Vector | Link State |
| Metric | Hop count | Cost |
| Algorithm | Bellman-Ford concept | Dijkstra |
| Maximum path metric | 15 hops | No RIP-style 15-hop limit |
| Network knowledge | Neighbor-based | Topology database |
| Convergence | Generally slower | Generally faster |
| Scalability | Small networks | Much larger networks |
| IPv6 | RIPng | OSPFv3 |

---

# 🧠 The simplest mental model

Think of RIP as asking:

> **"How many people do I have to pass through to reach that person?"**

It doesn't care whether the road is:

```text
🚗 1 Mbps
```

or:

```text
🚀 10 Gbps
```

If both paths have 3 hops:

```text
3 hops = 3 hops
```

That's RIP's fundamental simplicity.

---

# 🔥 RIP Cheat Sheet

Remember these:

```text
RIP
│
├── Distance Vector
│
├── Metric = Hop Count
│
├── 15 = maximum usable
│
├── 16 = unreachable
│
├── Bellman-Ford concept
│
├── Count-to-Infinity problem
│
├── Split Horizon / Poison Reverse
│
└── RIPv2 supports CIDR/VLSM
```

### One-line definition:

> **RIP is a Distance Vector routing protocol that uses hop count to select routes, with 16 representing unreachable.**

### Module 8 progress

✅ Distance Vector Routing  
✅ Link State Routing  
✅ **RIP**  
⬜ OSPF  
⬜ BGP  
⬜ Convergence  
⬜ Routing Loops

**Next → OSPF**, where we'll see how a real Link State protocol builds its topology database and uses **Dijkstra** to calculate routes.