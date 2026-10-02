<div align="center">

# 🌐 Module 08: Routing and Forwarding II
### *Dynamic Routing Protocols, Distance Vector vs Link State, RIP, OSPF, BGP & Loops*

<p align="center">
  <img src="https://img.shields.io/badge/Module-08-0052CC?style=for-the-badge&logo=target" alt="Module 08" />
  <img src="https://img.shields.io/badge/Topics-7_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 07: Routing and Forwarding I**](../Module%2007%3A%20Routing%20and%20Forwarding%20I/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 09: Routing Protocols**](../Module%2009%3A%20Routing%20Protocols/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Networks are constantly changing: links fail, cables get cut, and traffic spikes. Routers cannot rely solely on static routes. Module 08 introduces dynamic routing protocols, contrasting the two dominant philosophies—Distance Vector (RIP) and Link State (OSPF)—while exploring convergence, inter-domain routing (BGP), and routing loop mitigation.

---

## 🎯 What You Will Master

- [x] **Contrast Distance Vector ("tell neighbors your view of the world") with Link State ("tell the world your view of your neighbors").**
- [x] **Understand the fundamentals of RIP (Routing Information Protocol): hop-count metric, 15-hop limit, and periodic updates.**
- [x] **Understand the fundamentals of OSPF (Open Shortest Path First): link-state database, Dijkstra execution, and hierarchical Areas.**
- [x] **Understand the high-level role of BGP (Border Gateway Protocol) as the glue connecting Autonomous Systems (AS) across the global Internet.**
- [x] **Define network convergence and analyze the trade-off between convergence speed and router CPU/bandwidth overhead.**
- [x] **Analyze the causes of Routing Loops (such as the Count-to-Infinity problem) and how Split Horizon, Poison Reverse, and Hold-down Timers prevent them.**

---

## 🧠 Architectural Mental Model

```
Routing Philosophies Comparison:

DISTANCE VECTOR (e.g., RIP)              LINK STATE (e.g., OSPF)
"Routing by Rumor"                       "Complete Network Map"

[Router A] <--> [Router B] <--> [Router C]   Each router floods Link-State Advertisements
     |               |               |       to all other routers in the Area.
(A only knows what B tells it about C)
                                             Every router builds an IDENTICAL map (LSDB),
- Simpler, lightweight                       then runs Dijkstra independently to find
- Slower convergence                         the true shortest path tree.
- Risk of count-to-infinity loops
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [🌐 Distance Vector Routing — Deep Dive](./01_Distance_Vector_Routing.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [🌐 Module 8 — Topic 2: Link State Routing](./02_Link_State_Routing.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [🌐 Module 8 — Topic 3: RIP](./03_RIP.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [🌐 Module 8 — Topic 4: OSPF](./04_OSPF.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [🌍 Module 8 — Topic 5: BGP (High Level)](./05_BGP_High_Level.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [🔄 Module 8 — Topic 6: Convergence](./06_Convergence.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **07** | [🔁 Module 8 — Final Topic: Routing Loops](./07_Routing_Loops.md) | `- [ ]` Ready | ⏱️ 14 mins |

---

## 💡 Practical Challenge & Self-Assessment

**Count-to-Infinity Challenge:** What mechanism in Distance Vector routing prevents a router from advertising a route back out the same interface it learned that route from?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- **Split Horizon**.
- With Poison Reverse, it advertises the route back out that interface with a metric of **infinity (16 in RIP)** to definitively declare it unusable.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: 🌐 Distance Vector Routing — Deep Dive**](./01_Distance_Vector_Routing.md) 👈

---

| ⬅️ [**Module 07: Routing and Forwarding I**](../Module%2007%3A%20Routing%20and%20Forwarding%20I/README.md) | 📑 [**Back to Top**](#-module-08-) | [**Module 09: Routing Protocols**](../Module%2009%3A%20Routing%20Protocols/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
