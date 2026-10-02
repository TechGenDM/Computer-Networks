<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 06: Graph Algorithms for Networking II* • **Topic 01 of 03** (Global #038)

| [⬅️ Prev Module: Dijkstra's Algorithm 🚦](../Module%2005%3A%20Graph%20Algorithms%20for%20Networking/04_Dijkstra.md) | 📑 [**Module Overview**](./README.md) | [Next: Minimum Spanning Tree (MST) 🌳 ➡️](./02_Minimum_Spanning_Tree.md) |
| :--- | :---: | ---: |

---

</div>

# 1. Bellman-Ford

Let's start with the key question:

> **What's the difference between Bellman-Ford and Dijkstra?**

Both can find shortest paths, but they have an important difference.

### Dijkstra

Works with:

> **Non-negative edge weights**

### Bellman-Ford

Can handle:

> **Negative edge weights**

For example:

```text
A ──4──► B
│        │
5       -3
│        │
▼        ▼
C ◄──────D
```

Dijkstra can fail when negative edges exist.

Bellman-Ford can still work.

---

## 🧠 How does Bellman-Ford work?

Its core idea is surprisingly simple:

> **Repeatedly try to improve the shortest-known distance to every node.**

This process is called **relaxation**.

Suppose:

```text
A ──5──► B
A ──2──► C
C ──1──► B
```

Initially:

```text
A = 0
B = ∞
C = ∞
```

From A:

```text
B = 5
C = 2
```

Then through C:

```text
A → C → B
2 + 1 = 3
```

So B improves:

```text
B: 5 → 3
```

Bellman-Ford keeps doing this systematically.

---

# 🔥 The famous `V - 1` rule

If there are `V` vertices, Bellman-Ford relaxes all edges up to:

\[
V-1
\]

times.

Why?

A shortest simple path can contain at most:

\[
V-1
\]

edges.

---

## 🚨 Negative Cycle Detection

This is another major advantage.

Suppose:

```text
A ──2──► B
▲       │
│       -5
└──1────C
```

If you can keep going around a cycle and continuously make the path cheaper, you've found a:

> **Negative-weight cycle**

Bellman-Ford can detect this.

That's something standard Dijkstra doesn't handle.

---

# 🌐 What about networking?

Here's an important historical connection.

The **Distance Vector** family of routing algorithms is closely related to the **Bellman-Ford shortest-path idea**.

Routers exchange information about distances to destinations and iteratively improve their routing knowledge.

A famous example is:

> **RIP (Routing Information Protocol)**

RIP uses a distance-vector approach based on hop count.

So:

```text
Bellman-Ford
      ↓
Distance Vector concept
      ↓
Routing protocols
```

That's why learning this algorithm is relevant to networking.

---

# 🆚 Dijkstra vs Bellman-Ford

| | Dijkstra | Bellman-Ford |
|---|---|---|
| Shortest path | ✅ | ✅ |
| Negative edges | ❌ | ✅ |
| Negative cycles | ❌ | ✅ Detects |
| Generally faster | ✅ | ❌ |
| Networking connection | Link-state | Distance-vector |

### Simple memory:

> **Dijkstra = fast, non-negative weights**

> **Bellman-Ford = more flexible, can handle negative weights**

---

Next we'll look at **Minimum Spanning Tree (MST)** at a high level, and I'll make sure you don't confuse an MST with a **shortest-path tree**—they solve completely different problems.

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *Why is the Bellman-Ford algorithm naturally suited for distributed routing protocols like RIP?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> Bellman-Ford operates locally: each router only needs distance estimates from its immediate neighbors to update its own table ($D_x(y) = \min_v \{c(x,v) + D_v(y)\}$) without knowing the global map.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Prev Module: Dijkstra's Algorithm 🚦](../Module%2005%3A%20Graph%20Algorithms%20for%20Networking/04_Dijkstra.md) | [**Module 06: Graph Algorithms for Networking II**](./README.md) | [Next: Minimum Spanning Tree (MST) 🌳 ➡️](./02_Minimum_Spanning_Tree.md) |

<div align="center">
  <br/>
  <code>[███░░░░░░░] 33% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
