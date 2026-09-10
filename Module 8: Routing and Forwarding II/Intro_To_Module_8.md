# 🌐 Module 8 — Routing and Forwarding II

Perfect. This module goes deeper into **how routers actually learn routes and react when the network changes**.

Your syllabus has 7 topics:

1. **Distance Vector Routing**
2. **Link State Routing**
3. **RIP**
4. **OSPF**
5. **BGP (High Level)**
6. **Convergence**
7. **Routing Loops**

Let's build this from the ground up.

---

# 1. Distance Vector Routing

Imagine this network:

```text
A ── 2 ── B ── 3 ── C
 \       /
  └─ 5 ─┘
```

Each number represents the **cost** of a link.

Router A wants to reach C.

Initially, A might know:

```text
A → B = 2
A → C = unknown
```

B tells A:

> "I can reach C with cost 3."

A thinks:

```text
A → B → C
2 + 3 = 5
```

So A learns:

```text
C → cost 5, next hop B
```

That's the basic idea of **Distance Vector Routing**.

### What does "Distance Vector" mean?

**Distance** = how expensive/far the destination is according to the routing metric.

**Vector** = which direction/next hop to use.

So a router essentially maintains:

```text
Destination | Cost | Next Hop
-------------|------|---------
B            | 2    | B
C            | 5    | B
```

---

## 🧠 Key idea

> **Routers tell their neighbors what they know about destinations.**

They don't necessarily know the entire network topology.

This is why Distance Vector is sometimes described as **"routing by rumors."** 😄

Router A learns about C **through B**, rather than having a complete map of the network.

---

# 2. Link State Routing

Now imagine doing something completely different.

Instead of:

> "Hey B, what do you know?"

Every router builds a **map of the network**.

For example:

```text
      2
 A ─────── B
 |         |
5|         |3
 |         |
 C ─────── D
      1
```

Each router learns:

```text
A connected to B with cost 2
A connected to C with cost 5
B connected to D with cost 3
C connected to D with cost 1
```

Then the router has a topology database.

It can run a shortest-path algorithm, typically **Dijkstra**, to calculate the best routes.

---

# Distance Vector vs Link State

| Distance Vector | Link State |
|---|---|
| Learns from neighbors | Builds topology map |
| Doesn't need full topology | Knows topology within its routing domain |
| Simpler concept | More complex |
| Can converge more slowly | Usually faster convergence |
| Example: RIP | Example: OSPF |

### Easy memory trick:

**Distance Vector:**

> "Tell me what you know."

**Link State:**

> "Give me the map."

---

# 3. RIP

**RIP = Routing Information Protocol**

RIP is a classic **Distance Vector** routing protocol.

Its famous metric is:

> **Hop count**

Example:

```text
A → B → C → D
```

A reaches D in:

```text
3 hops
```

Another route:

```text
A → E → F → D
```

Also:

```text
3 hops
```

RIP considers them equal based on hop count.

It doesn't inherently care that one physical path might be faster.

---

### RIP's major limitation

RIP has a relatively small maximum usable hop count: **15**.

A metric of **16 means unreachable**.

So:

```text
1–15 hops → usable
16 hops   → unreachable
```

That's one reason RIP isn't suitable for large modern networks.

---

# 4. OSPF

Now we move to the more powerful one.

**OSPF = Open Shortest Path First**

OSPF is a **Link State** routing protocol.

Remember:

> RIP → Distance Vector  
> OSPF → Link State

OSPF routers exchange information about links and build a topology database.

Then they use **Dijkstra's Shortest Path First algorithm** to calculate routes.

---

### Example

```text
       2
 A ─────── B
 |         |
 5         2
 |         |
 C ─────── D
       1
```

A can calculate:

```text
A → B → D = 2 + 2 = 4

A → C → D = 5 + 1 = 6
```

Therefore:

```text
A → B → D
```

is preferred.

---

# 5. BGP — High Level

Now comes the **BIG one**. 🌍

**BGP = Border Gateway Protocol**

BGP is the protocol used for routing **between autonomous systems (ASes)** on the Internet.

An **Autonomous System** is basically a network/network collection under one administrative organization and routing policy.

For example, conceptually:

```text
        Internet
           |
    ┌──────┴──────┐
    ↓             ↓
   AS 1          AS 2
    |             |
  Network       Network
```

BGP allows these different networks to exchange reachability information.

---

### OSPF vs BGP

Think:

```text
OSPF
 ↓
Inside an organization/network

BGP
 ↓
Between different autonomous systems
```

For example:

```text
Company Network
      ↓
    OSPF
      ↓
Company Border Router
      ↓
    BGP
      ↓
     ISP
      ↓
   Internet
```

BGP is not simply:

> "Find the shortest physical path."

It considers **routing policies and attributes**.

That's extremely important.

---

# 6. Convergence

Imagine:

```text
A ── B ── C
```

Everything is working.

Then:

```text
B ── C
  ❌
```

The routers must discover the failure and update their routing information.

The process by which routers **reach a consistent view of the network after a change** is called:

> **Convergence**

Before convergence:

```text
Router A → old information
Router B → new information
Router C → maybe old information
```

After convergence:

```text
Routers agree on the new usable routes
```

### Faster convergence = generally better

Because packets are less likely to encounter:

- stale routes
- black holes
- temporary routing loops

---

# 7. Routing Loops

This is where things get interesting.

Suppose:

```text
A → B → C
```

But because of stale routing information:

```text
A thinks:
C → B

B thinks:
C → A
```

Now:

```text
A → B
↑   ↓
└───┘
```

The packet can circulate:

```text
A → B → A → B → A → B...
```

That's a **routing loop**.

---

## TTL saves us

Remember TTL from the previous module?

```text
TTL = 5

A → B   TTL 4
B → A   TTL 3
A → B   TTL 2
B → A   TTL 1
A → B   TTL 0 → DROP
```

So the packet cannot loop forever.

Routing protocols also use various mechanisms to **prevent or reduce routing loops**.

---

# 🧠 Module 8 — Big Picture

Put everything together:

```text
                 ROUTING
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
   Distance Vector       Link State
          │                   │
          ↓                   ↓
         RIP                 OSPF
          │                   │
       neighbors          topology map
          │                   │
          └─────────┬─────────┘
                    ↓
               Routing Info
                    ↓
               Convergence
                    ↓
             Network changes
                    ↓
             Routing Loops
```

And at the Internet level:

```text
        Inside AS
           ↓
         OSPF
           ↓
      Border Router
           ↓
          BGP
           ↓
     Another AS / ISP
```

### 🔥 Remember these 5 lines

> **Distance Vector → "What do my neighbors know?"**

> **Link State → "What does the network map look like?"**

> **RIP → Distance Vector + hop count**

> **OSPF → Link State + Dijkstra**

> **BGP → Routing between Autonomous Systems + policy**

---

We've now covered the **conceptual foundation of all 7 topics** in Module 8. Next, we can go **deep topic-by-topic**, starting with **Distance Vector Routing**, including the actual routing-table updates and a worked example.