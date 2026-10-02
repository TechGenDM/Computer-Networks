<div align="center">

# 🌐 Module 04: IP Addresses and Subnetting II
### *Practical Subnet Engineering, VLSM, IPv6 Expansion & Gateway Routing*

<p align="center">
  <img src="https://img.shields.io/badge/Module-04-0052CC?style=for-the-badge&logo=target" alt="Module 04" />
  <img src="https://img.shields.io/badge/Topics-6_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 03: IP Addresses and Subnetting I**](../Module%2003%3A%20IP%20Addresses%20and%20Subnetting%20I/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 05: Graph Algorithms for Networking**](../Module%2005%3A%20Graph%20Algorithms%20for%20Networking/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Ready to level up your subnetting skills to engineer-grade proficiency? Module 04 dives into practical subnet calculations, Variable Length Subnet Masking (VLSM) to prevent IP waste, the 128-bit architecture of IPv6, default gateway exit paths, and an introduction to NAT.

---

## 🎯 What You Will Master

- [x] **Solve practical subnetting problems: calculate usable host ranges, first usable host, last usable host, and broadcast addresses.**
- [x] **Implement Variable Length Subnet Masking (VLSM) to allocate appropriately sized subnets for LANs and point-to-point router links.**
- [x] **Explore IPv6: 128-bit hexadecimal addressing, zero compression shorthand (`::`), and why IPv6 eliminates the need for NAT.**
- [x] **Understand the Default Gateway: how hosts determine whether a destination IP is on-link (local) or off-link (requires gateway).**
- [x] **Differentiate clearly between Network Address (all host bits 0) and Directed Broadcast Address (all host bits 1).**
- [x] **Grasp the conceptual need for Network Address Translation (NAT) and how private home devices share a single public IP.**

---

## 🧠 Architectural Mental Model

```
Subnetting a /24 network (192.168.1.0/24) into two /25 subnets:

Original:  [ 192.168.1.0 ------------------------------------- 192.168.1.255 ] (/24)
           Total: 256 addresses (254 usable hosts)

Subnet A:  [ 192.168.1.0 ------- 192.168.1.127 ] (/25, Mask: 255.255.255.128)
           - Network ID: 192.168.1.0
           - Usable Range: 192.168.1.1 - 192.168.1.126 (126 hosts)
           - Broadcast: 192.168.1.127

Subnet B:  [ 192.168.1.128 ----- 192.168.1.255 ] (/25, Mask: 255.255.255.128)
           - Network ID: 192.168.1.128
           - Usable Range: 192.168.1.129 - 192.168.1.254 (126 hosts)
           - Broadcast: 192.168.1.255
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [Subnetting Problems](./01_Subnetting_Problems.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [Variable Length Subnetting (VLSM) 🔀](./02_Variable_Length_Subnetting_VLSM.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [IPv6 🌐](./03_IPv6.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [Default Gateway 🚪](./04_Default_Gateway.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [Network Address vs Broadcast Address 📡](./05_Network_Address_vs_Broadcast_Address.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [NAT — Network Address Translation 🔄](./06_Introduction_to_NAT.md) | `- [ ]` Ready | ⏱️ 12 mins |

---

## 💡 Practical Challenge & Self-Assessment

**VLSM Sizing Challenge:** You are given the block `172.16.0.0/24`. You must allocate IPs for: (1) Office LAN with 50 hosts, (2) Branch LAN with 20 hosts, and (3) a Point-to-Point WAN link with 2 hosts. What prefix lengths (`/x`) should you use for each?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- Office LAN (50 hosts): Needs $2^6 - 2 = 62$ hosts $\rightarrow$ **/26** (e.g., `172.16.0.0/26`).
- Branch LAN (20 hosts): Needs $2^5 - 2 = 30$ hosts $\rightarrow$ **/27** (e.g., `172.16.0.64/27`).
- WAN Link (2 hosts): Needs $2^2 - 2 = 2$ hosts $\rightarrow$ **/30** (e.g., `172.16.0.96/30`).
- This avoids wasting valuable IP addresses!
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: Subnetting Problems**](./01_Subnetting_Problems.md) 👈

---

| ⬅️ [**Module 03: IP Addresses and Subnetting I**](../Module%2003%3A%20IP%20Addresses%20and%20Subnetting%20I/README.md) | 📑 [**Back to Top**](#-module-04-) | [**Module 05: Graph Algorithms for Networking**](../Module%2005%3A%20Graph%20Algorithms%20for%20Networking/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
