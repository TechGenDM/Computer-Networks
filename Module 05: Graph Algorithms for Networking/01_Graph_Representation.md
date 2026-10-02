<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 05: Graph Algorithms for Networking* • **Topic 01 of 04** (Global #034)

| [⬅️ Prev Module: NAT — Network Address Translation 🔄](../Module%2004%3A%20IP%20Addresses%20and%20Subnetting%20II/06_Introduction_to_NAT.md) | 📑 [**Module Overview**](./README.md) | [Next: BFS — Breadth-First Search 🌳 ➡️](./02_BFS.md) |
| :--- | :---: | ---: |

---

</div>

# 1. Graph Representation in Networking 🌐🕸️

The big idea is:

> **A computer network can be represented as a graph.**

For example:

```text
A ─── B ─── C
│     │
D ─── E ─── F
```

Here:

* **Vertices (nodes)** → routers, computers, servers, etc.
* **Edges** → network connections/links

Once we represent a network as a graph, we can use graph algorithms to answer questions like:

> "How can I reach this destination?"

> "What's the shortest path?"

> "Can I reach every node?"

And that's exactly where **BFS, DFS, and Dijkstra** come in.

---

# 1. Graph Representation

Before running an algorithm, we need to **store the graph in memory**.

The two most important representations are:

### Adjacency Matrix

Suppose:

```text
A ─ B
│   │
C ─ D
```

We can create a matrix:

```text
      A B C D
A     0 1 1 0
B     1 0 0 1
C     1 0 0 1
D     0 1 1 0
```

`1` means an edge exists.

`0` means no edge.

For a weighted network, instead of `1`, we can store the **link cost**:

```text
A ──5── B
```

would store `5`.

### Adjacency List

Usually much more practical for sparse graphs.

```text
A → B, C
B → A, D
C → A, D
D → B, C
```

In code, this might look like:

```text
A: [B, C]
B: [A, D]
C: [A, D]
D: [B, C]
```

### 🧠 When to think about which?

**Matrix:**

> "I want to quickly check whether two nodes are directly connected."

**List:**

> "I want to efficiently see all neighbors of a node."

For most real-world networks, **adjacency lists are generally preferred** because networks are often sparse.

---

# 🌐 Why graphs matter in networking

Imagine:

```text
Laptop
  │
Router A
 /      \
B        C
 \      /
   Router D
      │
    Server
```

This is naturally a graph:

```text
Laptop → A → B → D → Server
           ↘ C ↗
```

Now we can run algorithms on it.

### BFS

Useful for exploring the network **level by level**.

### DFS

Useful for exploring deeply along paths and for graph traversal tasks.

### Dijkstra

Useful for finding the **shortest/lowest-cost path** when edge weights are non-negative.

---

## 🎯 Mental model

Think of a network as:

```text
NETWORK

Devices/Routers = Nodes
Connections      = Edges

        ↓

GRAPH

        ↓

Algorithms

BFS
DFS
Dijkstra
```

And there's one especially important networking connection:

> **Routing is fundamentally about finding paths through a graph.**

We'll start with **BFS** next, where you'll see exactly how a graph gets explored **level by level**, using a **queue**.

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *Why do large network simulation tools and routers prefer Adjacency Lists over Adjacency Matrices?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> Network graphs are sparse ($|E| \ll |V|^2$); each router connects to only a few neighbors. Adjacency lists require $O(V + E)$ memory, whereas an adjacency matrix consumes $O(V^2)$ memory, mostly wasted on zeros.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Prev Module: NAT — Network Address Translation 🔄](../Module%2004%3A%20IP%20Addresses%20and%20Subnetting%20II/06_Introduction_to_NAT.md) | [**Module 05: Graph Algorithms for Networking**](./README.md) | [Next: BFS — Breadth-First Search 🌳 ➡️](./02_BFS.md) |

<div align="center">
  <br/>
  <code>[███░░░░░░░] 30% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
