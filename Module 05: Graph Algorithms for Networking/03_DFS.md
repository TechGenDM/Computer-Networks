<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 05: Graph Algorithms for Networking* • **Topic 03 of 04** (Global #036)

| [⬅️ Previous: BFS — Breadth-First Search 🌳](./02_BFS.md) | 📑 [**Module Overview**](./README.md) | [Next: Dijkstra's Algorithm 🚦 ➡️](./04_Dijkstra.md) |
| :--- | :---: | ---: |

---

</div>

# 3. DFS — Depth-First Search 🌊

Now let's learn **DFS**.

**DFS = Depth-First Search**

Instead of exploring level-by-level like BFS, DFS:

> **goes as deep as possible along one path, then comes back and tries another path.**

---

## 🌳 Example

Consider:

```text
        A
      /   \
     B     C
    / \
   D   E
```

Starting from `A`, DFS could visit:

```text
A → B → D → E → C
```

Why?

It goes:

```text
A
 ↓
B
 ↓
D
```

`D` has nothing new to explore, so it goes back:

```text
D
↑
B
 ↓
E
```

Then backtrack again and explore `C`.

---

# 🧠 BFS vs DFS

This is the easiest way to remember them:

### BFS

```text
        A
      /   \
     B     C
    / \     \
   D   E     F

A → B → C → D → E → F
```

**Spread outward.**

Uses:

> **Queue**

---

### DFS

```text
        A
        ↓
        B
        ↓
        D
        ↓
      backtrack
        ↓
        E
        ↓
      backtrack
        ↓
        C
```

**Go deep, then backtrack.**

Uses:

> **Stack** or **recursion**

---

# 🔧 How DFS Works

There are two common implementations.

### Recursive DFS

Conceptually:

```python
def dfs(node):
    mark_visited(node)

    for neighbor in graph[node]:
        if not visited[neighbor]:
            dfs(neighbor)
```

The function keeps calling itself until it can't go deeper.

---

### Iterative DFS

Use an explicit stack:

```text
Stack
┌───┐
│ A │
└───┘
```

The stack follows:

> **LIFO — Last In, First Out**

---

# 🌐 DFS in Networking

DFS isn't normally the main algorithm used by Internet routing protocols for choosing paths.

But graph traversal concepts are useful for:

* Exploring network topology
* Connectivity checking
* Detecting cycles
* Finding connected components
* Network analysis

For example:

> "Can every router in this topology be reached from router A?"

DFS can answer that.

---

# ⚠️ DFS does NOT guarantee shortest path

Suppose:

```text
A ─ B ─ C ─ D
 \       /
  ──────
```

DFS may take a long route before discovering another route.

So:

> **DFS = traversal**

not:

> **shortest-path algorithm**

For an unweighted graph, BFS is the usual choice for shortest path by hop count.

For weighted non-negative graphs, we'll use **Dijkstra**.

---

# ⏱️ Complexity

With an adjacency list:

$$
O(V+E)
$$

Same asymptotic complexity as BFS.

---

# 🎯 Remember

> **BFS → Queue → Wide/level-by-level**

> **DFS → Stack/Recursion → Deep/backtracking**

A tiny memory trick:

```text
BFS = Broad First
DFS = Deep First
```

Next is the most important algorithm in this module:

# **Dijkstra's Algorithm**

That's where we move from simply exploring a network to finding the **minimum-cost path between routers**.

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *How does Depth-First Search detect cycles in a network topology graph?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> During DFS traversal, if an edge leads to an ancestor node currently in the recursion call stack (a "back edge"), a cycle/loop is present in the graph.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: BFS — Breadth-First Search 🌳](./02_BFS.md) | [**Module 05: Graph Algorithms for Networking**](./README.md) | [Next: Dijkstra's Algorithm 🚦 ➡️](./04_Dijkstra.md) |

<div align="center">
  <br/>
  <code>[███░░░░░░░] 32% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
