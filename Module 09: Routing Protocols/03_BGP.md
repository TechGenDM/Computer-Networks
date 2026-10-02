<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 09: Routing Protocols* • **Topic 03 of 03** (Global #057)

| [⬅️ Previous: 🌐 OSPF — Open Shortest Path First](./02_OSPF.md) | 📑 [**Module Overview**](./README.md) | [Next Module: Why DNS? (Domain Name System) ➡️](../Module%2010%3A%20DNS%20and%20Internet%20Applications/01_Why_DNS.md) |
| :--- | :---: | ---: |

---

</div>

# 🌍 BGP — Border Gateway Protocol

This is the final topic in your **Routing Protocols** module.

RIP and OSPF mostly deal with routing **within a network**. BGP deals with routing **between different networks**—which is why it is fundamental to how the Internet works.

---

# 1. What is BGP?

**BGP (Border Gateway Protocol)** is the protocol used to exchange routes between **Autonomous Systems (ASes)**.

An **Autonomous System** is a network or group of networks operated under a common routing administration and policy.

For example:

```text
        AS 100
     ISP / Network A
          │
          │ BGP
          │
        AS 200
     ISP / Network B
          │
          │ BGP
          │
        AS 300
     ISP / Network C
```

BGP allows these independently operated networks to tell each other:

> **"I can reach these IP prefixes through me."**

---

# 2. Why doesn't OSPF handle the whole Internet?

Imagine trying to run one giant OSPF topology containing:

```text
Google
Cloudflare
Airtel
Jio
Amazon
Microsoft
Universities
Banks
Millions of networks...
```

That would be a fundamentally different scaling and administrative problem.

The Internet is made up of many independently operated networks.

So we have a useful separation:

```text
Inside an AS
     ↓
OSPF / IS-IS / RIP etc.
     ↓
Between ASes
     ↓
BGP
```

For example:

```text
             AS 100
        ┌──────────────┐
        │ OSPF         │
        │              │
        │ R1──R2──R3   │
        └──────┬───────┘
               │
              BGP
               │
        ┌──────┴───────┐
        │ AS 200       │
        │              │
        │ OSPF         │
        │ R4──R5──R6   │
        └──────────────┘
```

So a single organization might use **OSPF internally** while using **BGP to connect to other ASes**.

---

# 3. What does BGP advertise?

BGP primarily advertises **IP prefixes**.

For example:

```text
203.0.113.0/24
```

means:

> "I have a route to this network."

It doesn't need to advertise every individual device:

```text
203.0.113.1
203.0.113.2
203.0.113.3
...
```

Instead, networks are generally represented as prefixes.

Think:

```text
BGP:
"I can reach 203.0.113.0/24."

rather than:

"I know where every computer inside that network is."
```

---

# 4. BGP is a Path-Vector Protocol

This is one of the most important terms.

```text
RIP  → Distance Vector
OSPF → Link State
BGP  → Path Vector
```

With BGP, route advertisements carry path information, including the sequence of ASes a route has traversed.

Example:

```text
AS 100 → AS 200 → AS 300
```

AS 300 might advertise a prefix back toward AS 100 with an AS path such as:

```text
300 200 100
```

This gives routers information about the **AS-level path**.

---

# 5. AS_PATH

One particularly important BGP attribute is:

```text
AS_PATH
```

Suppose:

```text
AS 100
   │
AS 200
   │
AS 300
```

AS 300 advertises:

```text
Prefix: 10.10.0.0/16
AS_PATH: 300
```

AS 200 receives it and advertises toward AS 100:

```text
Prefix: 10.10.0.0/16
AS_PATH: 200 300
```

AS 100 now knows:

```text
10.10.0.0/16
via AS 200 → AS 300
```

---

# 6. AS_PATH Helps Prevent Loops 🔄

Suppose:

```text
AS 100 → AS 200 → AS 300
```

Later the route somehow comes back toward AS 100.

If AS 100 sees:

```text
AS_PATH:
100 200 300
```

it can see its own AS number in the path.

It can reject that route rather than accepting a path that loops back through itself.

So:

> **BGP uses AS_PATH to help detect and prevent certain inter-AS routing loops.**

---

# 7. BGP Doesn't Simply Choose the Shortest Path

This is a **very important difference** from how you've been thinking about Dijkstra.

OSPF:

```text
Find lowest-cost path
```

BGP:

```text
Consider routing attributes
+
routing policy
+
path information
```

For example, an organization may prefer one provider over another for business or engineering reasons.

So BGP is not simply:

```text
"Fewest routers = best."
```

or even:

```text
"Shortest physical distance = best."
```

Policy matters.

---

# 8. BGP Attributes

At a high level, BGP uses attributes to help select routes.

Some important ones you'll encounter are:

```text
AS_PATH
LOCAL_PREF
MED
NEXT_HOP
COMMUNITY
```

You do **not** need to master the selection process yet.

Just understand:

> **BGP routes carry attributes, and routers use those attributes and local policy when selecting routes.**

---

# 9. eBGP vs iBGP

You'll see these two terms frequently.

### eBGP

**External BGP**

Used between different autonomous systems.

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
 │ R1──iBGP──R2  │
 │               │
 └───────────────┘
```

Mental shortcut:

```text
eBGP → External AS
iBGP → Internal AS
```

---

# 10. BGP + OSPF Together

This is a very realistic architecture.

Imagine an ISP:

```text
                 Internet
                    │
                  BGP
                    │
             ┌──────┴──────┐
             │ ISP / AS 100 │
             │              │
             │ OSPF         │
             │              │
             │ R1──R2──R3   │
             └──────────────┘
```

Inside the ISP:

```text
OSPF
↓
Find paths between internal routers
```

At the edge:

```text
BGP
↓
Exchange routes with other ASes
```

So the protocols are not necessarily competitors.

They can work together.

---

# 11. Internet Example 🌐

Imagine:

```text
Your ISP
   │
   │ BGP
   ↓
Another network
   │
   │ BGP
   ↓
Google's network
```

At a high level, BGP helps the different networks determine how traffic can reach the advertised prefixes.

Inside each network, internal routing protocols can determine how packets get from one internal router to another.

---

# 12. BGP vs RIP vs OSPF

This table is worth remembering:

| | RIP | OSPF | BGP |
|---|---|---|---|
| Type | Distance Vector | Link State | Path Vector |
| Main idea | Neighbor distances | Topology map | AS paths + policy |
| Metric/selection | Hop count | Cost | Attributes + policy |
| Algorithm concept | Bellman-Ford | Dijkstra/SPF | BGP path selection |
| Scope | Inside network | Inside network | Between ASes |
| Major use | Small/simple networks | Enterprise/ISP internal routing | Internet/inter-AS routing |

The mental model:

```text
RIP
"How far away?"

OSPF
"Give me the map."

BGP
"Which networks can you reach,
through what AS path,
and according to what policy?"
```

---

# 🧠 One complete picture

Let's connect everything you've learned:

```text
                    INTERNET

      AS 100                     AS 200
 ┌─────────────┐             ┌─────────────┐
 │             │    BGP      │             │
 │ OSPF        │◄───────────►│ OSPF        │
 │             │             │             │
 │ R1──R2──R3  │             │ R4──R5──R6  │
 │             │             │             │
 └─────────────┘             └─────────────┘
```

Inside AS 100:

```text
OSPF → determines internal routes
```

Between AS 100 and AS 200:

```text
BGP → exchanges reachable prefixes
```

Then forwarding happens:

```text
Routing information
       ↓
Routing table
       ↓
Forwarding table
       ↓
Packet forwarded hop-by-hop
```

---

# 🔥 What you should remember for exams/interviews

### 1. What is BGP?

> **BGP is a path-vector routing protocol used to exchange routes between Autonomous Systems.**

### 2. What is an AS?

> **A collection of IP networks under a common routing administration and policy.**

### 3. What does BGP advertise?

> **IP prefixes/routes.**

### 4. What is AS_PATH?

> **A BGP attribute that records the sequence of ASes associated with a route and helps with loop prevention and route selection.**

### 5. eBGP vs iBGP?

```text
eBGP → between ASes
iBGP → within an AS
```

### 6. Biggest conceptual difference

```text
RIP  → Distance Vector
OSPF → Link State
BGP  → Path Vector
```

---

## 🎯 Routing Protocols module complete

✅ RIP  
✅ OSPF  
✅ **BGP (High Level)**

You now have the big picture:

```text
                 ROUTING PROTOCOLS

        ┌─────────────┬─────────────┬─────────────┐
        │    RIP      │    OSPF     │    BGP      │
        ├─────────────┼─────────────┼─────────────┤
        │ Distance    │ Link State  │ Path Vector │
        │ Vector      │             │             │
        │             │             │             │
        │ Hop Count   │ Cost        │ Policy +    │
        │             │             │ Attributes  │
        │             │             │             │
        │ Internal    │ Internal    │ Inter-AS    │
        └─────────────┴─────────────┴─────────────┘
```

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What is the difference between eBGP and iBGP peering?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> eBGP peers connect border routers belonging to **different** Autonomous Systems (ASes) to exchange external routes. iBGP peers connect routers **within the same** AS to distribute external BGP routes internally.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🌐 OSPF — Open Shortest Path First](./02_OSPF.md) | [**Module 09: Routing Protocols**](./README.md) | [Next Module: Why DNS? (Domain Name System) ➡️](../Module%2010%3A%20DNS%20and%20Internet%20Applications/01_Why_DNS.md) |

<div align="center">
  <br/>
  <code>[█████░░░░░] 50% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
