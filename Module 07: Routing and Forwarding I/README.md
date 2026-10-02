<div align="center">

# 🌐 Module 07: Routing and Forwarding I
### *Router Internals, Routing Tables, Longest Prefix Match & Forwarding Mechanics*

<p align="center">
  <img src="https://img.shields.io/badge/Module-07-0052CC?style=for-the-badge&logo=target" alt="Module 07" />
  <img src="https://img.shields.io/badge/Topics-7_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 06: Graph Algorithms for Networking II**](../Module%2006%3A%20Graph%20Algorithms%20for%20Networking%20II/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 08: Routing and Forwarding II**](../Module%2008%3A%20Routing%20and%20Forwarding%20II/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Now we transition from graph algorithms to actual router behavior. What happens inside a router when a packet arrives at an interface? In this module, you will explore the separation between the Control Plane and Data Plane, examine routing tables, master the Longest Prefix Match algorithm, and trace packet forwarding step-by-step.

---

## 🎯 What You Will Master

- [x] **Distinguish between Routing (Control Plane: deciding the path) and Forwarding (Data Plane: moving the packet from input to output port).**
- [x] **Inspect the anatomy of an IP Routing Table: Destination Prefix, Subnet Mask, Next-Hop IP, Outgoing Interface, and Metric.**
- [x] **Master the Longest Prefix Match (LPM) algorithm and understand why more specific CIDR prefixes always win over broader prefixes.**
- [x] **Configure and analyze Static Routes, understanding administrative distance and use cases for stub networks.**
- [x] **Understand the Default Route (`0.0.0.0/0`): why it is the "gateway of last resort" and how it keeps routing tables compact.**
- [x] **Trace the step-by-step packet forwarding pipeline: Layer-2 frame decapsulation, IP header validation, TTL decrement, checksum update, Layer-2 rewrite, and transmission.**
- [x] **Understand the Hop-by-Hop routing paradigm and how independent routers collaborate to provide end-to-end delivery.**

---

## 🧠 Architectural Mental Model

```
Packet Forwarding Pipeline inside a Router:

[Incoming Frame] ──► 1. Verify FCS & Decapsulate Frame
                           │
                           ▼
                     2. Validate IPv4 Header Checksum
                           │
                           ▼
                     3. Decrement TTL by 1 (Drop & send ICMP if TTL=0)
                           │
                           ▼
                     4. Longest Prefix Match (LPM) in Routing/FIB Table
                           │
                           ▼
                     5. Determine Outgoing Interface & Next-Hop IP
                           │
                           ▼
                     6. Recompute IPv4 Header Checksum
                           │
                           ▼
                     7. Query ARP for Next-Hop MAC Address
                           │
                           ▼
[Outgoing Frame] ◄── 8. Encapsulate into New L2 Frame & Transmit!
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [Router](./01_Router.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [Routing Table 🗺️](./02_Routing_Table.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [Longest Prefix Match 🎯](./03_Longest_Prefix_Match.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [Static Routing 🛣️](./04_Static_Routing.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [Default Route `0.0.0.0/0` 🌍](./05_Default_Route.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [Packet Forwarding 📦➡️](./06_Packet_Forwarding.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **07** | [Hop by Hop Routing](./07_Hop_by_Hop_Routing.md) | `- [ ]` Ready | ⏱️ 14 mins |

---

## 💡 Practical Challenge & Self-Assessment

**LPM Forwarding Challenge:** A router has the following routing table:
1. `10.0.0.0/8` -> Interface eth0
2. `10.1.0.0/16` -> Interface eth1
3. `10.1.2.0/24` -> Interface eth2
4. `0.0.0.0/0` -> Interface eth3

Which interface will a packet with destination IP `10.1.2.55` be forwarded to?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- All four routes match! However, `10.1.2.0/24` has a prefix length of **24 bits**, which is the longest (most specific) match.
- Therefore, the packet is forwarded out **Interface eth2**.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: Router**](./01_Router.md) 👈

---

| ⬅️ [**Module 06: Graph Algorithms for Networking II**](../Module%2006%3A%20Graph%20Algorithms%20for%20Networking%20II/README.md) | 📑 [**Back to Top**](#-module-07-) | [**Module 08: Routing and Forwarding II**](../Module%2008%3A%20Routing%20and%20Forwarding%20II/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
