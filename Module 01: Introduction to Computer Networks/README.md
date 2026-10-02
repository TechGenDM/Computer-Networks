<div align="center">

# 🌐 Module 01: Introduction to Computer Networks
### *Foundations of Connectivity, Physical Infrastructure & Layered Mental Models*

<p align="center">
  <img src="https://img.shields.io/badge/Module-01-0052CC?style=for-the-badge&logo=target" alt="Module 01" />
  <img src="https://img.shields.io/badge/Topics-9_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| 🏁 *First Module* | 🏠 [**Repository Roadmap**](../README.md) | [**Module 02: Network Packets and Layered Communication**](../Module%2002%3A%20Network%20Packets%20and%20Layered%20Communication/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Welcome to the starting line of your Computer Networks journey! In this module, you will build an intuitive, foundational mental model of how networks are structured—from physical links and network devices to packet switching, delay math, and layered architectural models (OSI and TCP/IP).

---

## 🎯 What You Will Master

- [x] **Understand why computer networks exist and master the WHO-HOW-WHERE-WHAT communication mental model.**
- [x] **Distinguish between LAN, MAN, WAN, and the global interconnected network: the Internet.**
- [x] **Trace internet topology through Tier 1, 2, and 3 ISPs, IXPs, and Points of Presence (POPs).**
- [x] **Differentiate the Network Edge (hosts/servers) from the Network Core (mesh of switches/routers).**
- [x] **Compare Packet Switching vs. Circuit Switching and understand why the modern Internet uses packet switching.**
- [x] **Calculate the four components of network delay: nodal processing, queuing, transmission, and propagation delay.**
- [x] **Grasp the physics and mathematics of bandwidth, throughput, and link bottlenecks.**
- [x] **Identify hardware roles: Hubs, Layer-2 Switches, Layer-3 Routers, and Modems.**
- [x] **Compare the 7-layer OSI conceptual reference model with the pragmatic 4/5-layer TCP/IP protocol suite.**

---

## 🧠 Architectural Mental Model

```
+-----------------------------------------------------------------------------------+
|                                  THE NETWORK EDGE                                 |
|   [Laptop / Client]                     [Smartphone]                  [IoT]       |
+------------------------------------------+----------------------------------------+
                                           | Access Network (Wi-Fi / Ethernet / 5G)
                                           v
+-----------------------------------------------------------------------------------+
|                                  THE NETWORK CORE                                 |
|                 (Mesh of Routers + High-Capacity Fiber Trunk Links)               |
|                                                                                   |
|           [Router A] <======================> [Router B]                          |
|                \\                               //                                |
|                 \\                             //                                 |
|                  v                             v                                  |
|              [Router C] <================> [Router D]                             |
+------------------------------------------+----------------------------------------+
                                           | Tier-1 ISP Backbone / Data Center Link
                                           v
+-----------------------------------------------------------------------------------+
|                         DESTINATION (Servers / Cloud Services)                    |
|                        [Web Server]    [Database]    [API Gateway]                |
+-----------------------------------------------------------------------------------+
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [Why Computer Networks?](./01_Why_Computer_Networks.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [Types of Computer Networks](./02_Types_of_Networks_LAN_WAN_MAN.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [Internet Architecture](./03_Internet_Architecture.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [Network Edge vs Network Core](./04_Network_Edge_vs_Network_Core.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [Packet Switching vs Circuit Switching](./05_Packet_Switching_vs_Circuit_Switching.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [Network Delay](./06_Delay_Latency_Throughput.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **07** | [Bandwidth, Throughput & Bottleneck](./07_Bandwidth_and_Bottlenecks.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **08** | [Network Devices](./08_Network_Devices.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **09** | [OSI Model vs TCP/IP Model](./09_OSI_vs_TCP_IP_Birds_Eye_View.md) | `- [ ]` Ready | ⏱️ 10 mins |

---

## 💡 Practical Challenge & Self-Assessment

**Delay Calculation Challenge:** Suppose a packet is 1,500 bytes (12,000 bits) long. The link transmission rate (bandwidth) is 10 Mbps (10,000,000 bits/sec), the link distance is 2,000 km, and signal propagation speed is 200,000 km/s. What are the transmission delay ($d_{trans}$) and propagation delay ($d_{prop}$)?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- $d_{trans} = \frac{L}{R} = \frac{12,000 \text{ bits}}{10,000,000 \text{ bps}} = 0.0012 \text{ s} = 1.2 \text{ ms}$
- $d_{prop} = \frac{d}{s} = \frac{2,000 \text{ km}}{200,000 \text{ km/s}} = 0.010 \text{ s} = 10.0 \text{ ms}$
- Total Link Delay = $d_{trans} + d_{prop} = 1.2 \text{ ms} + 10.0 \text{ ms} = 11.2 \text{ ms}$ (ignoring queuing and processing).
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: Why Computer Networks?**](./01_Why_Computer_Networks.md) 👈

---

| 🏁 *First Module* | 📑 [**Back to Top**](#-module-01-) | [**Module 02: Network Packets and Layered Communication**](../Module%2002%3A%20Network%20Packets%20and%20Layered%20Communication/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
