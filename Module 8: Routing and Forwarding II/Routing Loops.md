# 🔁 Module 8 — Final Topic: Routing Loops

This is the perfect final topic because **routing loops connect directly to Distance Vector, RIP, convergence, and TTL**.

The core idea:

> **A routing loop happens when routers keep forwarding a packet toward each other instead of toward its destination.**

---

# 1. The simplest routing loop

Imagine:

```text
A ─── B
```

A wants to reach destination **X**.

But because of incorrect/stale routing information:

```text
A says:
X → B

B says:
X → A
```

Now a packet for X does:

```text
X packet
   ↓
A
 ↓
B
 ↓
A
 ↓
B
 ↓
A
 ↓
...
```

💥 That's a **routing loop**.

---

# 2. Why do routing loops happen?

The most common reason is:

> **Routers temporarily have inconsistent routing information.**

This often happens after a topology change.

For example:

```text
A ─── B ─── C
```

Suppose C becomes unreachable:

```text
A ─── B     C ❌
```

But A and B haven't both learned about the failure yet.

B might still believe:

```text
C → A
```

while A believes:

```text
C → B
```

Now:

```text
A → B → A → B → ...
```

---

# 3. Count-to-Infinity 🔥

This is the classic Distance Vector routing-loop problem.

Let's use:

```text
A ─── B ─── C
```

Suppose C suddenly fails.

Before failure:

```text
A → B → C
```

A's routing information:

```text
C = 2 hops
```

B's:

```text
C = 1 hop
```

Now C disappears.

B should say:

```text
C = ∞
```

But suppose B hasn't realized the failure yet.

A asks B:

> "How far is C?"

B responds:

> "C is 1 hop away."

A thinks:

```text
A → B → C

1 + 1 = 2
```

A tells B:

> "I can reach C in 2 hops."

B then thinks:

```text
B → A → C

1 + 2 = 3
```

Then:

```text
A → C = 4
B → C = 5
A → C = 6
B → C = 7
...
```

The metric keeps increasing.

That's:

# **Count-to-Infinity**

---

# 4. Why is it called "Infinity"?

Because the routers keep increasing their distance toward an unreachable destination.

Eventually the protocol needs to say:

> "Enough. This destination is unreachable."

Different protocols define infinity differently.

For **RIP**:

```text
15 = maximum usable hop count
16 = infinity / unreachable
```

So eventually:

```text
C = 16
```

means:

> ❌ C is unreachable.

---

# 5. Split Horizon

One technique used to reduce routing loops is:

> **Split Horizon**

The rule is:

> **Don't advertise a route back through the interface from which you learned that route.**

Example:

```text
A ─── B
```

Suppose B learned:

```text
X → through A
```

B should **not tell A**:

> "I can reach X through you."

Because that would create a misleading route.

Think:

> **"I learned this route from you, so I'm not going to advertise it back to you."**

---

# 6. Route Poisoning

Another technique is **route poisoning**.

Suppose B discovers:

```text
X ❌ unreachable
```

Instead of simply removing the route silently, B advertises:

```text
X → ∞
```

For RIP:

```text
X → 16
```

This tells neighboring routers:

> **"Do NOT use me to reach X. X is unreachable."**

---

# 7. Poison Reverse

Poison Reverse is closely related to Split Horizon.

With poison reverse, a router explicitly tells the neighbor from which it learned a route:

```text
X → ∞
```

Essentially:

> **"Don't use me to reach X through this route."**

It's stronger than simply not advertising the route.

---

# 8. Triggered Updates

Normally, routing protocols may exchange updates according to their protocol-specific behavior/timers.

But waiting for the next regular update can be slow.

A **triggered update** sends an update soon after a significant routing change.

Example:

```text
Link failure
     ↓
Router immediately sends update
     ↓
Neighbors learn the change
     ↓
Routes update
```

This can help reduce the period of incorrect routing information.

---

# 9. TTL is the final safety net

Even if a routing loop occurs, packets shouldn't circulate forever.

Remember:

```text
IPv4 → TTL
IPv6 → Hop Limit
```

Example:

```text
TTL = 4

A → B   TTL 3
B → A   TTL 2
A → B   TTL 1
B → A   TTL 0
       ↓
     DROP
```

The router discards the packet when the TTL/Hop Limit reaches zero.

Usually, the router can send an **ICMP Time Exceeded** message back toward the source.

This is also why tools like `traceroute` can reveal intermediate hops.

---

# 10. Routing loop vs Count-to-Infinity

Don't confuse these.

### Routing Loop

The **actual forwarding behavior**:

```text
A → B → A → B → ...
```

### Count-to-Infinity

A **routing-information problem** where Distance Vector routers keep increasing the metric toward an unreachable destination:

```text
2 → 3 → 4 → 5 → ... → ∞
```

Count-to-infinity can **cause or prolong routing loops**, but the two terms aren't identical.

---

# 11. Do Link-State protocols have routing loops?

Link-state protocols such as OSPF are designed to reduce many of the problems associated with classic distance-vector routing.

Because routers build a topology database and calculate paths using SPF, they generally have a more complete view of the network.

But don't memorize:

> ❌ "OSPF can never have routing loops."

Real networks can still experience **transient loops or forwarding inconsistencies**, particularly during topology changes or complex routing interactions.

The important distinction is:

```text
Distance Vector
→ particularly vulnerable to count-to-infinity

Link State
→ has a more complete topology view
→ generally avoids classic DV count-to-infinity behavior
```

---

# 12. BGP prevents certain loops differently

BGP uses the **AS_PATH** attribute.

Imagine:

```text
AS 100 → AS 200 → AS 300
```

AS 100 receives a route with:

```text
AS_PATH = 200 300
```

But if AS 100 later receives:

```text
AS_PATH = 200 300 100
```

AS 100 sees its own AS number in the path.

It can reject the route.

Why?

Because the route has effectively come back to the AS:

```text
AS 100 → ... → AS 100
```

That's an inter-domain routing loop.

So:

> **BGP uses AS_PATH to detect/prevent certain inter-AS routing loops.**

---

# 13. Big comparison 🔥

| Mechanism | Main purpose |
|---|---|
| Split Horizon | Don't advertise a route back where it was learned |
| Route Poisoning | Advertise failed route as unreachable |
| Poison Reverse | Explicitly advertise unreachable back to the neighbor |
| Triggered Updates | Quickly communicate routing changes |
| TTL / Hop Limit | Stop packets from looping forever |
| BGP AS_PATH | Detect certain inter-AS routing loops |

---

# 🧠 Complete example

Imagine:

```text
        R2
       /  \
      /    \
    R1      R3
      \    /
       \  /
        R4
```

R4 becomes unreachable through one path.

A routing protocol detects the failure:

```text
Failure
   ↓
Routing update
   ↓
Neighbors learn change
   ↓
Routes recalculated
   ↓
New path OR destination unreachable
```

If incorrect information temporarily exists:

```text
R1 → R2 → R1
```

that's a routing loop.

Protection mechanisms try to prevent or shorten this situation.

And if a packet still gets caught:

```text
TTL → 0
```

the packet is discarded.

---

# 🎯 Module 8 — COMPLETE

You now have the complete routing picture:

```text
                ROUTING
                   │
       ┌───────────┴───────────┐
       ↓                       ↓
Distance Vector           Link State
       ↓                       ↓
      RIP                    OSPF
       │                       │
Bellman-Ford                 Dijkstra
       │                       │
       └───────────┬───────────┘
                   ↓
              Convergence
                   ↓
            Routing changes
                   ↓
             Routing Loops
                   ↓
       Split Horizon / Poisoning
                   ↓
             TTL / Hop Limit
```

And at the Internet level:

```text
             Autonomous Systems
                     │
                     ↓
                    BGP
                     │
                  AS_PATH
                     │
              Loop prevention
```

### 🔥 Module 8 cheat sheet

> **RIP → Distance Vector → Hop Count**

> **OSPF → Link State → Dijkstra → Cost**

> **BGP → Path Vector → AS_PATH + Policy**

> **Convergence → Routers settle after a network change**

> **Routing Loop → Packets follow a circular path**

> **Count-to-Infinity → Distance Vector's classic loop problem**

> **TTL/Hop Limit → Prevent packets from looping forever**

---

## ✅ Module 8: Routing and Forwarding II — FINISHED

You've now covered **8 networking modules** from the fundamentals through routing protocols.

The next step is **Module 9** from your course syllabus. If you send me its screenshot, we'll continue exactly the same way—**concept → intuition → worked example → practical networking connection → exam/interview points.**