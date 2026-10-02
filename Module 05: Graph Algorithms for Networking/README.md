<div align="center">

# 🌐 Module 05: Graph Algorithms for Networking
### *Modeling Networks as Graphs, Breadth-First & Depth-First Search, Dijkstra's Algorithm*

<p align="center">
  <img src="https://img.shields.io/badge/Module-05-0052CC?style=for-the-badge&logo=target" alt="Module 05" />
  <img src="https://img.shields.io/badge/Topics-4_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 04: IP Addresses and Subnetting II**](../Module%2004%3A%20IP%20Addresses%20and%20Subnetting%20II/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 06: Graph Algorithms for Networking II**](../Module%2006%3A%20Graph%20Algorithms%20for%20Networking%20II/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Under the hood, computer networks are large, dynamic mathematical graphs. In this module, you connect classic computer science data structures with real-world networking: discovering network topologies via BFS, validating loop-free paths via DFS, and calculating shortest paths with Dijkstra's algorithm.

---

## 🎯 What You Will Master

- [x] **Represent network topologies as graphs: Routers/Switches as Vertices ($V$) and physical cables as Edges ($E$) with weights/costs.**
- [x] **Understand adjacency matrix vs. adjacency list representations and their computational trade-offs in network devices.**
- [x] **Apply Breadth-First Search (BFS) to compute hop counts and find the unweighted shortest path in local network meshes.**
- [x] **Apply Depth-First Search (DFS) for cycle detection, reachability verification, and graph component discovery.**
- [x] **Master Dijkstra's Shortest Path Algorithm: priority queue implementations, greedy relaxation, and building routing trees.**

---

## 🧠 Architectural Mental Model

```
Network Graph Topology:
       (2)
   [A] ---- [B]
    |        | \\ (3)
 (4)|     (2)|  \\
    |        |   [D]
   [C] ---- [D] //
       (1)

Link Costs:
A-B: 2, A-C: 4, B-D: 2, C-D: 1

Shortest Path from A to D:
- Path 1: A -> B -> D (Cost = 2 + 2 = 4)
- Path 2: A -> C -> D (Cost = 4 + 1 = 5)
-> Dijkstra selects Path 1: A -> B -> D with total cost 4!
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [Graph Representation in Networking 🌐🕸️](./01_Graph_Representation.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [BFS — Breadth-First Search 🌳](./02_BFS.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [DFS — Depth-First Search 🌊](./03_DFS.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [Dijkstra's Algorithm 🚦](./04_Dijkstra.md) | `- [ ]` Ready | ⏱️ 16 mins |

---

## 💡 Practical Challenge & Self-Assessment

**Graph Pathfinding Challenge:** In a network graph, why does standard BFS fail to find the optimal shortest path when link bandwidths differ?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- BFS assumes all edges have **equal weight (1 hop)**. It minimizes the *number of hops*, not the *cost* of the path.
- A 1-hop 10 Mbps satellite link would look "shorter" to BFS than a 2-hop 10 Gbps fiber path, even though the fiber path is 1,000x faster!
- That is why weighted algorithms like **Dijkstra** are required for real networks.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: Graph Representation in Networking 🌐🕸️**](./01_Graph_Representation.md) 👈

---

| ⬅️ [**Module 04: IP Addresses and Subnetting II**](../Module%2004%3A%20IP%20Addresses%20and%20Subnetting%20II/README.md) | 📑 [**Back to Top**](#-module-05-) | [**Module 06: Graph Algorithms for Networking II**](../Module%2006%3A%20Graph%20Algorithms%20for%20Networking%20II/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
