# 2. BFS — Breadth-First Search 🌳

Now let's learn **BFS**, one of the most important graph traversal algorithms.

## What is BFS?

**BFS = Breadth-First Search**

It explores a graph **level by level**.

Imagine you're standing at Router `A`:

```text id="3njp8x"
        A
      /   \
     B     C
    / \     \
   D   E     F
```

BFS explores:

```text id="5scj5s"
Level 0 → A
Level 1 → B, C
Level 2 → D, E, F
```

So the traversal is:

```text id="3kqv12"
A → B → C → D → E → F
```

---

# 🧠 How does BFS do this?

BFS uses a **Queue**.

Remember:

> **Queue = FIFO**
>
> First In → First Out

Think of a line at a ticket counter:

```text id="vq9kni"
A → B → C → D
↑
First out
```

---

# 🔍 Step-by-step

Using:

```text id="y5kq69"
        A
      /   \
     B     C
    / \     \
   D   E     F
```

Start at `A`.

### Step 1

Visit `A`.

Queue:

```text id="7t0f7w"
[A]
```

Remove `A`, then add its neighbors:

```text id="5n8p2d"
Queue: [B, C]
```

### Step 2

Remove `B`.

Add its unvisited neighbors:

```text id="g0m8qx"
Queue: [C, D, E]
```

### Step 3

Remove `C`.

Add `F`:

```text id="z7g6sh"
Queue: [D, E, F]
```

Then process:

```text
D → E → F
```

So:

```text id="6s7qz3"
BFS: A → B → C → D → E → F
```

---

# 🌐 Why is BFS useful in networking?

This is where it becomes interesting.

Suppose every network link has the **same cost**, and you want to find the path with the **fewest hops**.

Example:

```text id="p56z9k"
A ─ B ─ D
│       │
C ──────┘
```

From `A` to `D`:

```text
A → B → D = 2 hops
A → C → D = 2 hops
```

BFS can find a shortest path in terms of **number of edges/hops** in an unweighted graph.

---

# ⚠️ BFS vs Dijkstra

Don't confuse them.

### BFS

Finds shortest path when:

> **Every edge has equal weight** (or the graph is treated as unweighted).

### Dijkstra

Handles:

> **Different non-negative edge weights.**

For example:

```text id="7v8f8b"
A ──2── B
│       │
5       1
│       │
C ──1── D
```

Here, simply minimizing hops isn't enough.

That's where **Dijkstra** becomes useful.

---

# ⏱️ Complexity

With an **adjacency list**:

$$
O(V+E)
$$

where:

* `V` = number of vertices
* `E` = number of edges

So BFS is very efficient for large sparse graphs.

---

## 🎯 Remember this

> **BFS = level-by-level exploration**

> **BFS uses a Queue**

> **BFS finds shortest paths by number of edges in an unweighted graph**

Visual memory:

```text id="nq1rce"
        A          Level 0
      /   \
     B     C       Level 1
    / \     \
   D   E     F     Level 2
```

Next up: **DFS (Depth-First Search)** — same graph, but instead of spreading outward level-by-level, we'll go **as deep as possible before coming back**.
