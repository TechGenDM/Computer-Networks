<div align="center">

# 🌐 Module 06: Graph Algorithms for Networking II
### *Distributed Bellman-Ford, Minimum Spanning Trees & Graph Foundations of Routing*

<p align="center">
  <img src="https://img.shields.io/badge/Module-06-0052CC?style=for-the-badge&logo=target" alt="Module 06" />
  <img src="https://img.shields.io/badge/Topics-3_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 05: Graph Algorithms for Networking**](../Module%2005%3A%20Graph%20Algorithms%20for%20Networking/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 07: Routing and Forwarding I**](../Module%2007%3A%20Routing%20and%20Forwarding%20I/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Building on graph traversal, Module 06 investigates distributed graph algorithms that routers run independently across networks. Learn how Bellman-Ford handles distributed distance vectors and why Minimum Spanning Trees (MST) are essential for eliminating Layer-2 broadcast storms.

---

## 🎯 What You Will Master

- [x] **Understand the Bellman-Ford algorithm: relaxation principle, edge relaxation iterations, and handling distributed state.**
- [x] **Contrast centralized graph computation (Dijkstra in Link-State) with distributed neighbor computation (Bellman-Ford in Distance-Vector).**
- [x] **Understand the Minimum Spanning Tree (MST) concept (Prim's & Kruskal's principles) and why switches need it (Spanning Tree Protocol / STP).**
- [x] **Analyze how bridge loops create catastrophic Layer-2 broadcast storms and how spanning tree algorithms neutralize redundant loops.**
- [x] **Connect theoretical graph mathematics directly with the design of production routing tables.**

---

## 🧠 Architectural Mental Model

```
Layer-2 Loop Prevention with Spanning Tree (MST):

Physical Topology (With Loop):            Logical Topology (MST Active):
       [Switch A]                                [Switch A]
        /      \\                                 /      \\
       /        \\                               /        \\
 [Switch B] ---- [Switch C]                [Switch B]      [Switch C]
       (Loop hazard!)                                  (Port Blocked! ❌)

-> STP blocks redundant port on Switch C, breaking the loop and transforming
   the physical cycle into an active tree structure!
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [Bellman-Ford](./01_Bellman_Ford.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [Minimum Spanning Tree (MST) 🌳](./02_Minimum_Spanning_Tree.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [Why Routing Uses Graphs? 🌐🕸️](./03_Why_Routing_Uses_Graphs.md) | `- [ ]` Ready | ⏱️ 14 mins |

---

## 💡 Practical Challenge & Self-Assessment

**Bellman-Ford Iteration Challenge:** In a network with $V$ routers, what is the maximum number of relaxation passes Bellman-Ford must execute before guaranteeing shortest path convergence in a static topology without negative cycles?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- At most **$|V| - 1$ iterations**.
- A simple shortest path can contain at most $|V| - 1$ edges, and each relaxation pass guarantees that paths with one additional edge are correctly determined.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: Bellman-Ford**](./01_Bellman_Ford.md) 👈

---

| ⬅️ [**Module 05: Graph Algorithms for Networking**](../Module%2005%3A%20Graph%20Algorithms%20for%20Networking/README.md) | 📑 [**Back to Top**](#-module-06-) | [**Module 07: Routing and Forwarding I**](../Module%2007%3A%20Routing%20and%20Forwarding%20I/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
