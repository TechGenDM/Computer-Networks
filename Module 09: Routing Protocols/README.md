<div align="center">

# 🌐 Module 09: Routing Protocols
### *In-Depth Protocol Mechanics: RIP, OSPF & Internet-Scale BGP*

<p align="center">
  <img src="https://img.shields.io/badge/Module-09-0052CC?style=for-the-badge&logo=target" alt="Module 09" />
  <img src="https://img.shields.io/badge/Topics-3_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 08: Routing and Forwarding II**](../Module%2008%3A%20Routing%20and%20Forwarding%20II/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 10: DNS and Internet Applications**](../Module%2010%3A%20DNS%20and%20Internet%20Applications/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Now that you understand routing theory, Module 09 dives deep into the implementation specifics of the three definitive protocols that run the world: RIP (classic Distance Vector), OSPF (enterprise Link-State), and BGP (the inter-domain policy routing protocol of the global Internet).

---

## 🎯 What You Will Master

- [x] **Deep dive into RIP mechanics: RIP message format, UDP port 520, update timers (30s), invalid timers (180s), and flush timers (240s).**
- [x] **Examine OSPF in depth: Hello packets, Designated Router (DR) / Backup Designated Router (BDR) election, LSDB synchronization, and OSPF Area 0 backbone hierarchy.**
- [x] **Demystify Border Gateway Protocol (BGP): Autonomous System Numbers (ASN), eBGP (between ASes) vs. iBGP (inside an AS).**
- [x] **Master BGP Path Attributes: AS-PATH (loop prevention), NEXT-HOP, Multi-Exit Discriminator (MED), and Local Preference.**
- [x] **Understand how Internet Service Providers use BGP routing policies (peering vs. transit agreements) to control global traffic flows.**

---

## 🧠 Architectural Mental Model

```
Autonomous Systems and BGP vs IGP Hierarchy:

+-----------------------------+               +-----------------------------+
|    Autonomous System 100    |               |    Autonomous System 200    |
|                             |               |                             |
|    [Internal Router A]      |               |    [Internal Router C]      |
|            |                |               |            |                |
|       (OSPF / IGP)          |     eBGP      |       (OSPF / IGP)          |
|            |                |   Peering     |            |                |
|    [Border Router R1] <===========================> [Border Router R2]    |
+-----------------------------+               +-----------------------------+
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [RIP — Routing Information Protocol 🚦](./01_RIP.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [🌐 OSPF — Open Shortest Path First](./02_OSPF.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [🌍 BGP — Border Gateway Protocol](./03_BGP.md) | `- [ ]` Ready | ⏱️ 14 mins |

---

## 💡 Practical Challenge & Self-Assessment

**BGP Loop Prevention Challenge:** How does BGP detect and prevent routing loops when exchanging route updates across the Internet?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- Through the **`AS-PATH`** attribute.
- When a border router receives an update, it inspects the `AS-PATH` list. If its own Autonomous System Number (ASN) is already listed in the path, it immediately discards the update to prevent a loop!
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: RIP — Routing Information Protocol 🚦**](./01_RIP.md) 👈

---

| ⬅️ [**Module 08: Routing and Forwarding II**](../Module%2008%3A%20Routing%20and%20Forwarding%20II/README.md) | 📑 [**Back to Top**](#-module-09-) | [**Module 10: DNS and Internet Applications**](../Module%2010%3A%20DNS%20and%20Internet%20Applications/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
