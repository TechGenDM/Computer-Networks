# 3. Why Routing Uses Graphs? 🌐🕸️

This is the **final concept of Module 6**, and it connects everything we've learned.

The reason is simple:

> **A network naturally looks like a graph.**

---

## 🔗 Map a network to a graph

Imagine these routers:

```text id="2x1h2p"
       R2
      /  \
    5/    \2
    /      \
  R1 ──3── R3
   \       /
   4\     /1
     \   /
       R4
```

We can represent this as:

```text
Routers → Vertices (Nodes)
Links   → Edges
Cost    → Edge Weight
```

So:

```text id="x2a9so"
R1 ──3── R3
```

means:

- `R1` = node
- `R3` = node
- Connection = edge
- `3` = cost/weight

---

# 🚦 What does a router actually need to know?

Suppose:

```text id="8dhh36"
R1 → R4
```

There may be multiple possible paths:

```text id="xw9p9s"
R1 → R4

or

R1 → R3 → R4

or

R1 → R2 → R3 → R4
```

The router needs to determine:

> **Which path should I use?**

That's a **graph problem**.

---

# 🧮 Routing = Path Finding

Now our algorithms make sense.

### BFS

If every link has equal cost:

```text id="y9b3l3"
Find path with minimum number of hops
```

### Dijkstra

If links have different **non-negative costs**:

```text id="c6v9px"
Find minimum-cost path
```

### Bellman-Ford

Provides the mathematical foundation for shortest-path computation with more general edge weights and is closely related to **distance-vector routing**.

---

# 🌐 Real Routing Example

Imagine:

```text id="nq7d2f"
Laptop
   │
   ▼
R1
 / \
5   2
/     \
R2 ─1─ R3
 \     /
  4   3
   \ /
   R4
```

Suppose R1 needs to reach R4.

Possible paths:

```text id="h0t0zk"
R1 → R2 → R4
Cost = 5 + 4 = 9

R1 → R3 → R4
Cost = 2 + 3 = 5

R1 → R2 → R3 → R4
Cost = 5 + 1 + 3 = 9
```

So the lowest-cost route is:

```text id="ak2e7m"
R1 → R3 → R4
```

A shortest-path algorithm can help determine that.

---

# 🔥 What is "cost"?

Cost doesn't necessarily mean **money**.

A routing protocol can assign a metric based on things such as:

- Link capacity
- Delay
- Administrative configuration
- Hop count
- Other protocol-specific metrics

For example, **RIP** primarily uses hop count, while **OSPF** uses a cost metric associated with links.

So:

```text id="r3jyq4"
Network
   ↓
Graph
   ↓
Nodes + Edges + Metrics
   ↓
Shortest-path calculation
   ↓
Routing decision
```

---

# 🧠 Why not just choose the physically shortest path?

Because the physically shortest route isn't necessarily the **best network route**.

Imagine:

```text id="x8q7f3"
Path A → 100 km
Bandwidth = 10 Mbps

Path B → 200 km
Bandwidth = 1 Gbps
```

Depending on the routing metric and network design, Path B could be preferable.

So routing is about **optimizing according to a metric**, not simply geographical distance.

---

# 🎯 The complete connection

This is the key thing I want you to take away from this entire module:

```text
              COMPUTER NETWORK
                     │
                     ▼
              Represent as Graph
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
        Nodes       Edges      Weights
       Routers      Links       Cost
          │
          ▼
    Graph Algorithms
          │
    ┌─────┼──────┐
    ▼     ▼      ▼
   BFS   DFS  Dijkstra
          │
          ▼
      Routing
```

### One-line summary:

> **Networking uses graphs because routers and links form a graph, and routing is fundamentally the problem of choosing good paths through that graph.**

---

# 🎉 Module 6 Complete

You now have the basic algorithmic foundation behind networking:

**BFS → DFS → Dijkstra → Bellman-Ford → MST → Routing as a graph problem.**

The next module should take us back into **actual networking protocols**, where we'll start understanding **TCP/UDP, ports, sockets, connections, and how applications actually communicate over IP.**