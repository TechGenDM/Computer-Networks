# 🌐 Distance Vector Routing — Deep Dive

Let's learn this properly. Once you understand **one routing-table update**, Distance Vector becomes very easy.

---

## 1. The core idea

In Distance Vector routing, every router maintains a table like:

| Destination | Distance/Cost | Next Hop |
|---|---:|---|
| A | 0 | — |
| B | 2 | B |
| C | 5 | B |

The router is basically saying:

> **"To reach destination X, my cost is Y, and I should send the packet to Z."**

The important part:

**A router learns routes from its neighbors.**

It doesn't need the entire network map.

---

# 2. Our worked example

Consider this network:

```text
        1
   A -------- B
   |          |
  4|          |2
   |          |
   C -------- D
        1
```

Link costs:

```text
A ↔ B = 1
A ↔ C = 4
B ↔ D = 2
C ↔ D = 1
```

We want every router to eventually learn the cheapest route to every other router.

---

# 3. Initial routing tables

Initially, each router knows only:

1. Itself → cost `0`
2. Direct neighbors → direct link cost
3. Everything else → `∞` (unknown/unreachable)

### Router A

| Destination | Cost | Next Hop |
|---|---:|---|
| A | 0 | — |
| B | 1 | B |
| C | 4 | C |
| D | ∞ | — |

A doesn't initially know how to reach D.

---

### Router B

| Destination | Cost | Next Hop |
|---|---:|---|
| A | 1 | A |
| B | 0 | — |
| C | ∞ | — |
| D | 2 | D |

---

### Router C

| Destination | Cost | Next Hop |
|---|---:|---|
| A | 4 | A |
| B | ∞ | — |
| C | 0 | — |
| D | 1 | D |

---

### Router D

| Destination | Cost | Next Hop |
|---|---:|---|
| A | ∞ | — |
| B | 2 | B |
| C | 1 | C |
| D | 0 | — |

---

# 4. Now the magic happens 🔥

Routers exchange their distance vectors with neighbors.

For example, B tells A:

> "Here is what I know."

B's vector:

```text
A = 1
B = 0
C = ∞
D = 2
```

A receives this information.

A already knows:

```text
A → B = 1
```

So A asks:

> "If I go through B, how much would it cost to reach D?"

Formula:

```text
Cost(A → D through B)
=
Cost(A → B)
+
Cost(B → D)
```

Therefore:

```text
1 + 2 = 3
```

A discovers:

```text
D = 3
Next Hop = B
```

A's table becomes:

| Destination | Cost | Next Hop |
|---|---:|---|
| A | 0 | — |
| B | 1 | B |
| C | 4 | C |
| D | **3** | **B** |

🎯 A just learned a new route!

---

# 5. The Bellman-Ford idea

This update follows a fundamental formula:

\[
D_A(D) = \min_{neighbor\ V}\{c(A,V) + D_V(D)\}
\]

Don't let the formula scare you.

It simply means:

> **"For every neighbor, calculate the cost of going through that neighbor, and choose the cheapest one."**

For A → D:

Through B:

```text
A → B = 1
B → D = 2

Total = 3
```

Through C:

```text
A → C = 4
C → D = 1

Total = 5
```

Therefore:

```text
A → B → D
```

with cost:

```text
3
```

wins.

---

# 6. Another update

Now C receives information from D.

D says:

```text
B = 2
C = 1
D = 0
```

C already knows:

```text
C → D = 1
```

So:

```text
C → D → B
```

cost:

```text
1 + 2 = 3
```

C previously had:

```text
B = ∞
```

Now:

| Destination | Cost | Next Hop |
|---|---:|---|
| A | 4 | A |
| B | **3** | **D** |
| C | 0 | — |
| D | 1 | D |

---

# 7. More interesting: A finds a better route to C

Initially:

```text
A → C = 4
```

But A has learned:

```text
A → B = 1
B → D = 2
D → C = 1
```

Therefore:

```text
A → B → D → C
```

Total:

```text
1 + 2 + 1 = 4
```

That's **equal** to A's existing cost of 4.

So A doesn't necessarily need to change its route.

---

# 8. Eventually the network converges

After enough exchanges, routers can learn:

### A

| Destination | Cost | Next Hop |
|---|---:|---|
| A | 0 | — |
| B | 1 | B |
| C | **4** | C/D* |
| D | **3** | B |

### B

| Destination | Cost | Next Hop |
|---|---:|---|
| A | 1 | A |
| B | 0 | — |
| C | **3** | D |
| D | 2 | D |

### C

| Destination | Cost | Next Hop |
|---|---:|---|
| A | **4** | D/B* |
| B | 3 | D |
| C | 0 | — |
| D | 1 | D |

### D

| Destination | Cost | Next Hop |
|---|---:|---|
| A | 3 | B |
| B | 2 | B |
| C | 1 | C |
| D | 0 | — |

\*There can be equal-cost alternatives depending on the protocol's tie-breaking behavior.

At this point, the routing information has **converged**.

---

# 9. The really important part: what happens when a link fails?

🔥 This is where Distance Vector becomes interesting.

Suppose:

```text
B -------- D
     ❌
```

The B-D link fails.

Before failure:

```text
A → B → D
```

Cost:

```text
1 + 2 = 3
```

But now B can no longer directly reach D.

The network might need to find:

```text
B → A → C → D
```

Cost:

```text
1 + 4 + 1 = 6
```

So after the routing protocol converges again, B may learn:

```text
D = 6
Next Hop = A
```

This process of updating routes after a topology change is part of **convergence**.

---

# 10. The big problem: Routing Loops

Distance Vector has a famous problem called:

## Count-to-Infinity

Imagine:

```text
A -------- B -------- C
```

C becomes unreachable.

But A and B haven't learned about the failure correctly yet.

B might think:

> "A can reach C."

A might think:

> "B can reach C."

So:

```text
A → B → A → B → ...
```

Each router may keep increasing the advertised metric:

```text
2
3
4
5
6
7
...
```

This is called **count-to-infinity**.

Different protocols define "infinity" differently. In RIP, for example:

```text
16 = unreachable
```

So RIP eventually stops the count.

---

# 11. How Distance Vector tries to prevent loops

Several techniques exist.

### Split Horizon

A router doesn't advertise a route back through the interface from which it learned that route.

In simple terms:

> "I learned this route from you, so I'm not going to tell you that you can reach it through me."

---

### Route Poisoning

If a route becomes unreachable, advertise it with an infinite metric.

For RIP:

```text
Destination = unreachable
Metric = 16
```

---

### Poison Reverse

A stronger variation:

> Explicitly advertise the route back to the original neighbor with an infinite metric.

These mechanisms help reduce routing loops.

---

# 🧠 The mental model

Imagine four friends:

```text
A 🤝 B 🤝 C 🤝 D
```

Each friend doesn't have the whole map.

Instead, they ask their neighbors:

> "How far are you from D?"

B says:

> "I'm 2 away."

A knows B is 1 away:

```text
A → B = 1
B → D = 2

Therefore:

A → D = 3
```

That's **Distance Vector**.

---

# 🔥 What you should remember

### Distance Vector

**Routers exchange routing information with neighbors.**

### Distance

The cost/metric to reach a destination.

### Vector

The direction/next hop toward that destination.

### Update

```text
New Cost =
Cost to Neighbor
+
Neighbor's Cost to Destination
```

Then:

```text
Choose the minimum.
```

### Classic algorithm

**Bellman-Ford**

### Famous protocol

**RIP**

### Major problem

**Routing loops / Count-to-Infinity**

### Important protections

**Split Horizon, Route Poisoning, Poison Reverse**

---

## One-line definition

> **Distance Vector Routing is a routing approach where routers learn destination costs from their neighbors and choose the lowest-cost route based on those neighbor advertisements.**

Next in the module is **Link State Routing** — and the key difference is beautiful:

> **Distance Vector:** *"Tell me what you know."*  
> **Link State:** *"Give me the map."*