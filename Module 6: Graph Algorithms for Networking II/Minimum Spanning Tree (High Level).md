# 2. Minimum Spanning Tree (MST) 🌳

Now let's learn **MST**, but only at the high level as your syllabus says.

The first thing to understand:

> **MST is NOT a shortest-path algorithm.**

That's very important.

---

## 🌐 What problem does MST solve?

Imagine you need to connect **all routers/buildings/computers** together.

You have several possible connections, each with a cost:

```text
        4
   A ─────── B
   │ \       │
  2│  \5    1│
   │   \     │
   C ─────── D
        3
```

You want:

> **Connect every node while using the minimum possible total edge cost.**

That's the **Minimum Spanning Tree**.

---

# 🌳 Why "Tree"?

A tree is a graph that:

- Connects all nodes
- Has **no cycles**

For example:

```text
A ─── B ─── D
│
C
```

All nodes are connected and there is no loop.

If there are `V` vertices, an MST always contains:

\[
V-1
\]

edges.

So if we have 5 routers:

```text
V = 5
```

MST has:

\[
5-1=4
\]

edges.

---

# 🧠 Example

Suppose:

```text id="t1bq5v"
A ──1── B
│      / \
4    2   3
│  /       \
C ───────── D
      5
```

We want to connect:

```text
A, B, C, D
```

while minimizing total cost.

We might select:

```text id="y8l8xa"
A ──1── B
     │
     2
     │
     C

B ──3── D
```

Total:

\[
1+2+3=6
\]

All nodes are connected, and there's no cycle.

That's an MST.

---

# 🔥 MST vs Dijkstra

This is where students often get confused.

### Dijkstra asks:

> **"What's the cheapest path from A to each destination?"**

Example:

```text
A → B → C
```

You're concerned about **paths from a source**.

---

### MST asks:

> **"What's the cheapest way to connect ALL nodes together?"**

You're concerned about the **total cost of the entire network**.

---

### Think about it this way

**Dijkstra:**

```text
One source
     ↓
Find cheapest paths
     ↓
Destinations
```

**MST:**

```text
All nodes
     ↓
Connect everything
     ↓
Minimum total cost
```

---

# 🌐 Networking connection

Imagine a company wants to build physical connections between offices:

```text
Bengaluru
Mumbai
Delhi
Chennai
Hyderabad
```

Different fiber connections have different costs.

An MST-like optimization can help answer:

> **What set of links connects all locations with minimum total construction cost?**

However, here's an important distinction:

> **Real Internet routing does not simply run an MST algorithm to decide packet routes.**

Routing and physical network design are different problems.

---

# 🧠 Famous MST algorithms

You don't need to deeply study these for this module, but know their names:

### Kruskal's Algorithm

Sort edges by weight and keep adding the cheapest edge that doesn't create a cycle.

### Prim's Algorithm

Start from one node and repeatedly add the cheapest edge that expands the connected tree.

```text
MST Algorithms
     │
 ┌───┴────┐
Prim    Kruskal
```

---

# 🎯 Remember this distinction

| Algorithm | Question |
|---|---|
| **BFS** | Can I explore/reach nodes? |
| **DFS** | Can I explore deeply? |
| **Dijkstra** | Cheapest path from a source? |
| **Bellman-Ford** | Shortest paths with possible negative edges? |
| **MST** | Cheapest way to connect all nodes? |

The single most important sentence:

> **Shortest path ≠ Minimum spanning tree.**

And that brings us to the final topic of Module 6:

**Why does networking use graphs at all?**

Once you understand that, you'll see how **routers, links, costs, routing tables, Dijkstra, Bellman-Ford, and routing protocols** all fit together.