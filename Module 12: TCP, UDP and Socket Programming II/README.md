<div align="center">

# 🌐 Module 12: TCP, UDP and Socket Programming II
### *Sliding Windows, Sockets, Local Network Integration (ARP, NAT/PAT, DHCP & Gateways)*

<p align="center">
  <img src="https://img.shields.io/badge/Module-12-0052CC?style=for-the-badge&logo=target" alt="Module 12" />
  <img src="https://img.shields.io/badge/Topics-11_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 11: TCP, UDP and Socket Programming**](../Module%2011%3A%20TCP%2C%20UDP%20and%20Socket%20Programming/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 13: Network Troubleshooting**](../Module%2013%3A%20Network%20Troubleshooting/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Connecting transport concepts with end-to-end local network engineering! In Module 12, learn the sliding window mechanism, write foundational socket code, and explore the crucial networking services that make home and office networks function: ARP, NAT, PAT, DHCP, Default Gateways, and Home Router internals.

---

## 🎯 What You Will Master

- [x] **Understand the TCP Sliding Window mechanism: pipelining in-flight bytes, Bandwidth-Delay Product (BDP), and window movement.**
- [x] **Master Socket Programming fundamentals: `socket()`, `bind()`, `listen()`, `accept()`, `connect()`, `send()`, and `recv()` system calls.**
- [x] **Understand the Client-Server architectural pattern: iterative single-threaded vs. multi-threaded/concurrent event-driven servers.**
- [x] **Demystify ARP (Address Resolution Protocol): broadcasting for target MAC addresses, caching in the OS ARP table, and ARP spoofing risks.**
- [x] **Contrast Network Address Translation (NAT) with Port Address Translation (PAT / NAT Overload) and trace translation tables.**
- [x] **Understand the Dynamic Host Configuration Protocol (DHCP) and the 4-step DORA lease process (Discover, Offer, Request, Acknowledge).**
- [x] **Dissect the modern Home Router: an integrated appliance combining a Switch, Wi-Fi AP, Router, Firewall, DHCP Server, and NAT gateway.**
- [x] **Design RFC 1918 Private Networks and understand security boundaries between internal LANs and the public Internet.**

---

## 🧠 Architectural Mental Model

```
The Multi-Function Home Router Appliance:

+-------------------------------------------------------------------------------+
|                             HOME ROUTER APPLIANCE                             |
|                                                                               |
|  [Wi-Fi Access Point]         [4-Port Layer-2 Switch]                         |
|         │                                │                                    |
|         └───────────────┬────────────────┘                                    |
|                         ▼                                                     |
|             [Internal LAN: 192.168.1.1]                                       |
|                         │                                                     |
|       +-----------------┴-----------------+                                   |
|       │  - DHCP Server (Assigns IPs)      │                                   |
|       │  - NAT / PAT Translation Engine   │                                   |
|       │  - Stateful Packet Firewall       │                                   |
|       +-----------------┬-----------------+                                   |
|                         ▼                                                     |
|            [Layer-3 Routing Engine]                                           |
|                         │                                                     |
|             [WAN Port: Public IP: 203.0.113.4]                                |
+-------------------------│-----------------------------------------------------+
                          ▼ (Connects to ISP Modem / Fiber ONT)
                   PUBLIC INTERNET
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [🪟 Topic 1 — Sliding Window (Conceptual)](./01_Sliding_Window_Conceptual.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [🔌 Socket Programming Concepts](./02_Socket_Programming_Concepts.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [🖥️↔️🖥️ Client-Server Model](./03_Client_Server_Model.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [🔎 ARP — Address Resolution Protocol](./04_ARP.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [🔄 NAT — Network Address Translation](./05_NAT.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [🔀 PAT — Port Address Translation](./06_PAT.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **07** | [📡 DHCP — Dynamic Host Configuration Protocol](./07_DHCP.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **08** | [⏳ DHCP Lease Process](./08_DHCP_Lease_Process.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **09** | [🚪 Default Gateway](./09_Default_Gateway.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **10** | [🏠 Home Router](./10_Home_Router.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **11** | [🔒 Private Networks](./11_Private_Networks.md) | `- [ ]` Ready | ⏱️ 14 mins |

---

## 💡 Practical Challenge & Self-Assessment

**DHCP DORA Challenge:** Why are the DHCP Discover and DHCP Request messages sent as Layer-2 and Layer-3 broadcasts (`255.255.255.255`)?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- The client does not yet have an IP address, does not know the IP address of the DHCP server, and does not know the subnet parameters!
- Therefore, it must broadcast to `255.255.255.255` (and MAC `FF:FF:FF:FF:FF:FF`) so any listening DHCP server on the local wire can receive and answer the request.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: 🪟 Topic 1 — Sliding Window (Conceptual)**](./01_Sliding_Window_Conceptual.md) 👈

---

| ⬅️ [**Module 11: TCP, UDP and Socket Programming**](../Module%2011%3A%20TCP%2C%20UDP%20and%20Socket%20Programming/README.md) | 📑 [**Back to Top**](#-module-12-) | [**Module 13: Network Troubleshooting**](../Module%2013%3A%20Network%20Troubleshooting/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
