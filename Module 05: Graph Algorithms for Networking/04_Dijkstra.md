<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 05: Graph Algorithms for Networking* • **Topic 04 of 04** (Global #037)

| [⬅️ Previous: DFS — Depth-First Search 🌊](./03_DFS.md) | 📑 [**Module Overview**](./README.md) | [Next Module: Bellman-Ford ➡️](../Module%2006%3A%20Graph%20Algorithms%20for%20Networking%20II/01_Bellman_Ford.md) |
| :--- | :---: | ---: |

---

</div>

# 4. Dijkstra's Algorithm 🚦

Now we reach the **most networking-relevant algorithm** in this module.

The core question is:

> **"What is the lowest-cost path from one router to another?"**

---

## 🌐 Imagine this network

```text id="3joqwr"
        4
   A ─────── B
   │         │
  2│         │1
   │         │
   C ─────── D
        3
```

The numbers represent **cost/weight** of each link.

We want the shortest path from:

```text id="w7w5tq"
A → D
```

There are two obvious routes:

```text id="3io0u0"
A → B → D
Cost = 4 + 1 = 5

A → C → D
Cost = 2 + 3 = 5
```

Both have cost `5`.

But let's change one value:

```text id="z2px8a"
        4
   A ─────── B
   │         │
  2│         │1
   │         │
   C ─────── D
        10
```

Now:

```text
A → B → D = 5
A → C → D = 12
```

So the best path is:

> **A → B → D**

That's what Dijkstra helps us find.

---

# 🧠 What makes Dijkstra different from BFS?

BFS asks:

> **"What's the path with the fewest edges?"**

Dijkstra asks:

> **"What's the path with the smallest total weight?"**

Example:

```text
A ──1── B ──1── C
 \             /
  ───────5─────
```

BFS sees:

```text
A → C
```

as only **1 edge**, so it prefers it.

But Dijkstra calculates:

```text
A → C = 5

A → B → C = 1 + 1 = 2
```

So Dijkstra chooses:

```text
A → B → C
```

🔥 This is the key difference.

---

# ⚙️ How Dijkstra works

Let's use:

```text id="gk3uzt"
A ──4── B
│       │
2       1
│       │
C ──3── D
```

Start at `A`.

### Step 1 — Start

```text
A = 0
B = ∞
C = ∞
D = ∞
```

Distance means:

> "Best known cost from A to this node."

---

### Step 2 — Process A

From A:

```text
A → B = 4
A → C = 2
```

So:

```text id="hkzk9d"
A = 0
B = 4
C = 2
D = ∞
```

Choose the unvisited node with the **smallest known distance**:

```text
C = 2
```

---

### Step 3 — Process C

C connects to D with cost 3.

So:

```text
A → C → D
2 + 3 = 5
```

Update:

```text id="l0wz2w"
D = 5
```

Current distances:

```text
A = 0
B = 4
C = 2
D = 5
```

Choose:

```text
B = 4
```

---

### Step 4 — Process B

B → D costs 1.

Potential route:

```text
A → B → D
4 + 1 = 5
```

That's equal to the existing distance to D.

So D remains:

```text
D = 5
```

One shortest path is:

```text
A → B → D
```

Another equally cheap one is:

```text
A → C → D
```

---

# 🔥 The core idea

Dijkstra repeatedly does this:

> **Pick the unvisited node with the smallest tentative distance, then relax its neighbors.**

"Relax" simply means:

> **"Can I reach this neighbor more cheaply through the current node?"**

---

# 🌐 Why this matters in networking

A network can be represented as:

```text id="wxn1cx"
Router = Node

Link = Edge

Link cost = Weight
```

For example:

```text id="ih1n8a"
Router A
   │ 10
   ▼
Router B
   │ 5
   ▼
Router C
```

A routing system can use graph algorithms to reason about paths.

One famous example is **OSPF**, an IP routing protocol that uses a **link-state database** and a shortest-path calculation based on Dijkstra's algorithm.

So this isn't just a DSA exercise:

> **Graph theory → shortest paths → routing**

That's a major connection between your DSA and networking knowledge.

---

# ⚠️ Important limitation

Classic Dijkstra assumes:

> **Edge weights are non-negative.**

It does not correctly handle negative edge weights.

For computer networking, link costs are normally non-negative, so this isn't usually a practical issue.

---

# ⏱️ Complexity

With a simple implementation:

$$
O(V^2)
$$

With an adjacency list + min-priority queue:

$$
O((V+E)\log V)
$$

You don't need to memorize the exact complexity yet; the important thing is understanding **why the priority queue helps**.

---

# 🧠 Final comparison

| Algorithm    | Main idea           | Shortest path?         |
| ------------ | ------------------- | ---------------------- |
| **BFS**      | Level-by-level      | ✅ Unweighted           |
| **DFS**      | Go deep + backtrack | ❌ Not generally        |
| **Dijkstra** | Minimum total cost  | ✅ Non-negative weights |

### Mental picture

```text
BFS
A → nearest → next level → next level

DFS
A → deep → deep → backtrack

Dijkstra
A → cheapest known route → cheapest next route
```

And with that, **Module 5 is complete**. 🎉

You've now connected:

**Networks → Graphs → Traversal → Shortest-path routing.**

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *Why does standard Dijkstra's algorithm fail if a graph contains negative edge weights?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> Dijkstra is greedy: once a node is marked visited with the minimum tentative distance, it assumes no shorter path can exist. A negative edge encountered later could lower an already finalized distance, breaking Dijkstra's assumption.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: DFS — Depth-First Search 🌊](./03_DFS.md) | [**Module 05: Graph Algorithms for Networking**](./README.md) | [Next Module: Bellman-Ford ➡️](../Module%2006%3A%20Graph%20Algorithms%20for%20Networking%20II/01_Bellman_Ford.md) |

<div align="center">
  <br/>
  <code>[███░░░░░░░] 33% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
