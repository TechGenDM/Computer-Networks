# 🌐 Module 8 — Topic 2: Link State Routing

We just learned **Distance Vector**, where routers basically ask their neighbors:

> 🗣️ “What do you know about the network?”

Now Link State takes a different approach:

> 🗺️ **“Give me the network map, and I'll calculate the best path myself.”**

---

## 1. What is Link State Routing?

In **Link State Routing**, routers learn information about the links in the network and build a **topology database**.

For example:

```text
        2
   A -------- B
   |          |
  5|          |3
   |          |
   C -------- D
        1
```

A router can learn:

```text
A ↔ B = 2
A ↔ C = 5
B ↔ D = 3
C ↔ D = 1
```

It can therefore build a map:

```text
A ──2── B
│       │
5       3
│       │
C ──1── D
```

Then it runs a shortest-path algorithm—typically **Dijkstra**—to calculate the best routes.

---

# 2. What is a "Link State"?

A **link** is a connection between two routers.

**State** describes information about that connection.

For example:

```text
Router A
   |
   | cost = 10
   |
Router B
```

A can advertise:

```text
"I am connected to B.
The cost of this link is 10."
```

That information is called **link-state information**.

---

# 3. How does it actually work?

There are roughly **4 important steps**.

```text
1. Discover neighbors
        ↓
2. Learn link information
        ↓
3. Flood that information through the routing domain
        ↓
4. Build topology map + run Dijkstra
        ↓
5. Create routing table
```

Let's understand each.

---

# 4. Step 1 — Discover neighbors

Suppose:

```text
A ─── B ─── C
```

A discovers:

```text
Neighbor = B
Link cost = 2
```

B discovers:

```text
Neighbors = A, C
```

C discovers:

```text
Neighbor = B
```

So routers first figure out:

> **Who am I connected to?**

---

# 5. Step 2 — Create Link-State Advertisements

A router creates information describing its links.

For example, A might advertise:

```text
A says:

I am connected to:
B → cost 2
C → cost 5
```

This information is commonly carried in a **Link-State Advertisement (LSA)** in OSPF terminology.

---

# 6. Step 3 — Flood the information

Here's the major difference from Distance Vector.

The information isn't simply kept between immediate neighbors.

Routers **flood** link-state information throughout the relevant routing domain/area.

Imagine:

```text
A ── B ── C ── D
```

A generates information about its links.

It gets propagated:

```text
A → B → C → D
```

Now C can learn about A's links even though:

```text
A ↛ directly connected to C
```

Eventually routers have enough information to build the same topology database for their area.

---

# 7. Step 4 — Build the topology database

Suppose the network is:

```text
       2
  A -------- B
  |          |
 5|          |3
  |          |
  C -------- D
       1
```

A's topology database can represent:

```text
A-B = 2
A-C = 5
B-D = 3
C-D = 1
```

Now A has a **map of the network**.

This is the big advantage.

---

# 8. Step 5 — Run Dijkstra

Now A asks:

> "What is the cheapest path from me to every destination?"

Dijkstra calculates:

```text
A → B = 2
A → C = 5
A → B → D = 2 + 3 = 5
A → C → D = 5 + 1 = 6
```

Therefore:

```text
A → B → D
```

is the preferred path to D.

A can then construct its routing table.

---

# 9. Worked example 🔥

Let's make it slightly bigger:

```text
             2
        A -------- B
        |          |
       5|          |3
        |          |
        C -------- D
             1
```

Starting from A:

### Step 1

```text
A = 0
```

Neighbors:

```text
B = 2
C = 5
```

So:

```text
A
├── B = 2
└── C = 5
```

### Step 2

Choose the smallest unvisited node:

```text
B = 2
```

From B:

```text
B → D = 3
```

Therefore:

```text
A → B → D

2 + 3 = 5
```

So:

```text
D = 5
```

### Step 3

C has:

```text
A → C = 5
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

But D already has:

```text
5
```

So we keep:

```text
D = 5
```

Final shortest paths from A:

| Destination | Cost | Path |
|---|---:|---|
| A | 0 | A |
| B | 2 | A → B |
| C | 5 | A → C |
| D | 5 | A → B → D |

---

# 10. Distance Vector vs Link State

This is **VERY important for exams/interviews**.

| | Distance Vector | Link State |
|---|---|---|
| Basic idea | Learn from neighbors | Build network map |
| Knowledge | Destination costs via neighbors | Topology information |
| Communication | Neighbor-based updates | Flood link-state information |
| Algorithm | Bellman-Ford concept | Dijkstra |
| Example | RIP | OSPF |
| Convergence | Generally slower | Generally faster |
| Complexity | Simpler | More complex |
| Loop issues | More susceptible | Better controlled |

### Memory trick 🧠

```text
Distance Vector
      ↓
"Ask neighbors"
      ↓
Bellman-Ford
      ↓
RIP


Link State
      ↓
"Build the map"
      ↓
Dijkstra
      ↓
OSPF
```

---

# 11. What happens when a link fails?

This is where Link State shines.

Suppose:

```text
A ── B
     │
     │ ❌
     │
     D
```

B detects:

> "My link to D is down."

B generates updated link-state information.

That information is flooded.

Routers update their topology databases.

Then they run their shortest-path calculations again.

Eventually:

```text
New topology
      ↓
New shortest paths
      ↓
New routing tables
```

This is part of **convergence**.

---

# 12. One important distinction

Don't confuse:

### Topology Database

```text
A-B = 2
A-C = 5
B-D = 3
C-D = 1
```

This is the **map**.

### Routing Table

```text
Destination | Next Hop | Cost
D           | B        | 5
```

This is the **decision derived from the map**.

So:

> **Topology database → Dijkstra → Routing table**

That's a beautiful mental model.

---

# 🔥 Final mental model

Imagine you're driving through a city.

### Distance Vector

You don't have a map.

You ask people:

> "How far is the airport from here?"

They tell you what they know.

### Link State

Everyone gives you the road map.

You put the whole map together and use an algorithm to calculate the best route yourself.

---

## One-line definition

> **Link State Routing is a routing approach where routers learn the network topology, distribute link-state information, build a topology database, and independently calculate shortest paths—typically using Dijkstra.**

### Module 8 progress

✅ Distance Vector Routing  
✅ **Link State Routing**  
⬜ RIP  
⬜ OSPF  
⬜ BGP  
⬜ Convergence  
⬜ Routing Loops

**Next: RIP — we'll take the concepts we've just learned and see exactly how a real Distance Vector protocol works.**