# 🌐 OSPF — Open Shortest Path First

Now we move from **RIP** to a much more sophisticated routing protocol.

The core difference is:

> **RIP asks neighbors for distances. OSPF builds a map of the network and calculates the shortest paths itself.**

---

# 1. What is OSPF?

**OSPF (Open Shortest Path First)** is a **Link-State Interior Gateway Protocol (IGP)**.

It is primarily used **inside a single Autonomous System (AS)**, such as a company's internal network.

Its goal is to determine efficient paths to destinations based on **OSPF cost**.

---

# 2. RIP vs OSPF — the key difference

Imagine this network:

```text
        B
       / \
      /   \
     A     D
      \   /
       \ /
        C
```

### RIP

A router basically learns from neighbors:

```text
"B says D is 1 hop away."
"C says D is 1 hop away."
```

It uses hop count to decide.

### OSPF

A router learns the network topology:

```text
A ─ B ─ D
 \     /
  \ C /
```

It builds a **topology database**, then runs **Dijkstra's shortest-path algorithm**.

So:

```text
RIP  → "Tell me your distance."
OSPF → "Give me the network map."
```

That's the most important concept.

---

# 3. How OSPF works

There are roughly **5 major steps**:

```text
1. Discover neighbors
        ↓
2. Exchange link-state information
        ↓
3. Flood LSAs
        ↓
4. Build LSDB
        ↓
5. Run Dijkstra / SPF
        ↓
Routing Table
```

Let's break them down.

---

# 4. Step 1 — Discover Neighbors 🤝

Suppose:

```text
A ─ B ─ C
```

Router A discovers:

```text
A has neighbor B
```

Router B discovers:

```text
B has neighbors A and C
```

OSPF routers establish neighbor relationships so they can exchange routing information.

---

# 5. Step 2 — Link-State Information

A router needs to know things such as:

```text
Who are my neighbors?
Which links exist?
What is the cost of those links?
```

For example:

```text
A → B = cost 10
A → C = cost 5
```

This information forms the basis of OSPF's view of the network.

---

# 6. Step 3 — LSA

OSPF uses **LSAs — Link-State Advertisements**.

An LSA contains information about links/topology that routers use to construct their view of the network.

Routers **flood** relevant link-state information through the OSPF routing domain.

Think:

```text
A says:
"I am connected to B with cost 10."

       ↓ flood

Other OSPF routers learn this information.
```

---

# 7. Step 4 — LSDB 🗺️

After receiving LSAs, routers build a:

> **Link-State Database (LSDB)**

You can think of the LSDB as a **map of the OSPF topology**.

For example:

```text
A ─10─ B
│      │
5      2
│      │
C ─────D
    1
```

The router now has enough information to reason about possible paths.

---

# 8. Step 5 — Dijkstra Algorithm

Now OSPF uses **Shortest Path First (SPF)**, based on Dijkstra's algorithm.

Suppose:

```text
       B
     10│
       │
       D
      / 
     1
    C
```

and:

```text
A → B = 10
A → C = 5
B → D = 2
C → D = 1
```

From A:

### Path through B:

```text
A → B → D
10 + 2 = 12
```

### Path through C:

```text
A → C → D
5 + 1 = 6
```

OSPF therefore calculates:

```text
A → C → D = 6
```

The resulting information is used to build the routing table.

---

# 9. What is OSPF Cost?

This is important:

**OSPF does not use simple hop count like RIP.**

It uses a **cost associated with interfaces/links**.

A simplified way to think about it:

```text
Lower cost = more preferred path
Higher cost = less preferred path
```

The exact cost calculation depends on the OSPF implementation/configuration.

So two paths with the same number of routers could have different OSPF costs.

Example:

```text
Path 1: 2 hops → cost 20
Path 2: 3 hops → cost 8
```

OSPF can select Path 2 because:

```text
8 < 20
```

That's one major difference from RIP.

---

# 10. Why OSPF is called "Link State"

Because the router learns the **state of links in the topology**, rather than merely receiving a destination distance from each neighbor.

```text
Distance Vector:
"Destination X is 4 hops away."

Link State:
"Here is the topology and link information."
```

Then the router performs the path calculation itself.

---

# 11. OSPF Areas

For large networks, OSPF can divide the network into **areas**.

A very important one is:

```text
Area 0
```

which is the **backbone area**.

Conceptually:

```text
          Area 1
         /      \
        /        \
     Area 0 ───── Area 2
        \
         \
        Area 3
```

Areas help OSPF scale by limiting some topology information and SPF work rather than requiring every router to maintain the entire topology of a very large OSPF domain.

For now, remember:

> **Area 0 = OSPF backbone.**

---

# 12. OSPF vs RIP

| Feature | RIP | OSPF |
|---|---|---|
| Type | Distance Vector | Link State |
| Main metric | Hop count | Cost |
| Algorithm | Bellman-Ford concept | Dijkstra / SPF |
| Network view | Neighbor information | Topology database |
| Max hop limitation | 15 usable hops | No RIP-style 15-hop limit |
| Scaling | Smaller networks | Larger networks |
| Convergence | Generally slower | Generally faster |
| Areas | ❌ | ✅ |

---

# 🧠 Best mental model

Imagine you're traveling through a city.

### RIP

You ask nearby people:

> "How many streets away is the destination?"

They tell you their distances.

```text
Neighbor → "5 streets."
```

### OSPF

You receive a **map of the roads**:

```text
🗺️ Full-ish topology view
      ↓
Calculate possible routes
      ↓
Dijkstra
      ↓
Choose lowest-cost path
```

That's why OSPF can make more informed routing decisions.

---

# 🔥 OSPF flow — memorize this

```text
Neighbors
   ↓
Link-State Information
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

And the one-line definition:

> **OSPF is a link-state IGP that builds a topology database and uses the SPF/Dijkstra algorithm to calculate lowest-cost routes.**

---

## Module progress

✅ Routing Protocols — Introduction  
✅ RIP  
✅ **OSPF**  
⬜ BGP (High Level)

**Next → BGP (High Level) 🌍** — this is the protocol that takes us from routing **inside one network** to routing **between the networks that make up the Internet**.