# 🌍 Module 8 — Topic 5: BGP (High Level)

Now we reach **BGP**, the protocol that helps make the **Internet itself work**.

If OSPF is mainly about:

> 🏢 **“How do I route inside my network?”**

BGP is mainly about:

> 🌍 **“How do different networks reach each other?”**

---

# 1. What is BGP?

**BGP = Border Gateway Protocol**

It is the Internet's primary **inter-domain routing protocol**.

The key word is:

> **Inter-domain = between different Autonomous Systems (ASes).**

An **Autonomous System (AS)** is a network or collection of networks operated under a common administrative and routing policy.

Think:

```text
          INTERNET
             │
    ┌────────┼────────┐
    ↓        ↓        ↓
   AS 1     AS 2     AS 3
    │        │        │
  ISP A    ISP B    ISP C
```

BGP allows these systems to exchange information about which IP prefixes they can reach.

---

# 2. What is an Autonomous System?

Let's make it simple.

Imagine:

```text
Company A
   │
   └── AS 100

ISP B
   │
   └── AS 200

Cloud Provider C
   │
   └── AS 300
```

Each AS is administered independently.

BGP allows:

```text
AS 100 ↔ AS 200 ↔ AS 300
```

to exchange routing information.

---

# 3. OSPF vs BGP

This distinction is **extremely important**.

### OSPF

Usually used **inside an Autonomous System**.

```text
          AS 100
     ┌──────────────┐
     │              │
    R1 ── R2 ── R3  │
     │              │
     └──────────────┘
           ↑
          OSPF
```

### BGP

Used **between Autonomous Systems**.

```text
      AS 100             AS 200
   ┌─────────┐          ┌─────────┐
   │         │          │         │
   │ Network │ ← BGP →  │ Network │
   │         │          │         │
   └─────────┘          └─────────┘
```

### Memory trick:

> **OSPF = inside**  
> **BGP = between**

---

# 4. A real-world example

Imagine your request is going to a server hosted by another organization.

Conceptually:

```text
Your Network
     │
     │
   ISP A
     │
     │ BGP
     ↓
   ISP B
     │
     ↓
Server Network
```

ISP A needs to know:

> "Which network should I use to reach that server's IP prefix?"

BGP helps distribute that reachability information.

---

# 5. BGP works with IP prefixes

Remember subnetting?

For example:

```text
142.250.0.0/16
```

A network can advertise:

> **"I can reach 142.250.0.0/16."**

Another network might advertise:

```text
203.0.113.0/24
```

BGP exchanges these **prefixes/routes**.

So BGP isn't usually advertising:

```text
142.250.1.1
142.250.1.2
142.250.1.3
...
```

Instead, it works heavily with **aggregated prefixes**.

---

# 6. BGP isn't simply "shortest path"

This is one of the most important differences from what we've learned so far.

RIP:

> Lowest **hop count**

OSPF:

> Lowest **OSPF cost**

BGP:

> **Best route according to BGP attributes and routing policy**

For example:

```text
        ┌── ISP A ──┐
You ────┤           ├── Destination
        └── ISP B ──┘
```

Even if ISP B appears to have fewer hops, an organization may prefer ISP A because of:

- business agreements
- traffic-engineering policy
- reliability
- provider preference
- route attributes

So BGP is heavily about **policy**, not merely mathematical shortest path.

---

# 7. The idea of Path Vector

Here's another important concept.

BGP is generally described as a **Path Vector** protocol.

Instead of simply saying:

> "Destination X is 3 hops away."

BGP can advertise information about the **AS path**.

Example:

```text
AS 100 → AS 200 → AS 300
```

AS 100 might learn:

```text
Destination Prefix
      ↓
AS Path = 200 300
```

Meaning:

> "To reach this prefix, go through AS 200 and then AS 300."

---

# 8. Why AS Path is useful

Suppose:

```text
       AS 200
      /      \
AS 100        AS 300
      \      /
       AS 400
```

AS 100 could potentially reach AS 300 through:

```text
AS 100 → AS 200 → AS 300
```

or:

```text
AS 100 → AS 400 → AS 300
```

BGP can see the AS paths.

If a route comes back with an AS already present in its AS_PATH, BGP can reject it, helping prevent certain inter-domain routing loops.

That's a major advantage of the **path-vector** approach.

---

# 9. BGP route selection

BGP can have multiple routes to the same destination.

It evaluates attributes and policy to select a **best path**.

Some important BGP concepts/attributes include:

- **LOCAL_PREF**
- **AS_PATH**
- **MED**
- **NEXT_HOP**
- **COMMUNITY**

You don't need to memorize all their details yet.

At this level, remember:

> **BGP uses route attributes and policies to choose among possible paths.**

---

# 10. eBGP and iBGP

You may see these two terms frequently.

### eBGP

**External BGP**

Used between different ASes.

```text
AS 100
   │
 eBGP
   │
AS 200
```

### iBGP

**Internal BGP**

Used to distribute BGP information **within the same AS**.

```text
          AS 100
    ┌───────────────┐
    │               │
   R1 ←── iBGP ──→ R2
    │               │
    └───────────────┘
```

So:

```text
eBGP → between ASes
iBGP → within an AS
```

---

# 11. How BGP fits with OSPF

This is a **very realistic Internet architecture**.

Imagine an ISP:

```text
              ISP / AS 100
        ┌─────────────────────┐
        │                     │
       R1 ─── R2 ─── R3 ─── R4
        │                     │
        └─────────────────────┘
                 │
                BGP
                 │
              AS 200
```

Inside AS 100:

```text
OSPF
```

Between AS 100 and AS 200:

```text
BGP
```

So an organization can use **OSPF internally** while using **BGP externally**.

---

# 12. BGP and the Internet 🌍

Think about the Internet as thousands of independently operated networks:

```text
        Internet
           │
 ┌─────────┼─────────┐
 ↓         ↓         ↓
AS 1      AS 2      AS 3
 │         │         │
 ↓         ↓         ↓
AS 4 ←── AS 5 ──→ AS 6
```

BGP helps these networks exchange reachability information.

When a network says:

> "I can reach this IP prefix."

Other networks can use that information when making routing decisions.

That's why BGP is so fundamental to Internet connectivity.

---

# 13. What happens if a route disappears?

Suppose:

```text
AS 100 → AS 200 → AS 300
```

and the connection between AS 200 and AS 300 fails.

BGP can withdraw or update the affected route information.

Other routers then recalculate/select alternative paths according to their policies.

Conceptually:

```text
Failure
   ↓
Route update/withdrawal
   ↓
Other BGP routers process it
   ↓
Best-path selection
   ↓
New route
```

This contributes to **Internet-wide convergence**.

---

# 14. BGP vs OSPF vs RIP

🔥 This table is worth remembering.

| | RIP | OSPF | BGP |
|---|---|---|---|
| Type | Distance Vector | Link State | Path Vector |
| Main scope | Internal | Internal | Inter-domain |
| Metric/selection | Hop count | Cost | Attributes + policy |
| Knowledge | Neighbor routes | Topology | AS paths + routes/policy |
| Algorithm | Bellman-Ford concept | Dijkstra | Best-path decision process |
| Typical scale | Small | Large internal networks | Internet |
| Example role | Legacy/simple IGP | Modern IGP | Internet EGP |

**IGP** = Interior Gateway Protocol  
**EGP** = Exterior Gateway Protocol

---

# 🧠 The easiest mental model

Imagine three companies:

```text
🏢 Company A
      │
      │
      🌐 ISP
      │
      │
🏢 Company B
```

### OSPF

Inside Company A:

> "Which internal router should I use?"

### BGP

Between Company A's/ISP's network and another AS:

> "Which network can reach this IP prefix, and what route/policy should I use?"

---

# 🔥 BGP Cheat Sheet

```text
BGP
│
├── Border Gateway Protocol
│
├── Inter-domain routing
│
├── Works between Autonomous Systems
│
├── Path Vector
│
├── Uses AS_PATH
│
├── Uses routing policies/attributes
│
├── eBGP → between ASes
│
└── iBGP → within an AS
```

### The golden comparison:

> **RIP → "How many hops?"**

> **OSPF → "What does the topology map say?"**

> **BGP → "Which path does policy say we should use?"**

---

## Module 8 progress

✅ Distance Vector Routing  
✅ Link State Routing  
✅ RIP  
✅ OSPF  
✅ **BGP (High Level)**  
⬜ Convergence  
⬜ Routing Loops

**Next → Convergence** 🔄 — we'll connect RIP, OSPF, and BGP together and understand exactly what happens throughout a network when a link or router suddenly fails.