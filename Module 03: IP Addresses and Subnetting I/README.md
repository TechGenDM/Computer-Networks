<div align="center">

# 🌐 Module 03: IP Addresses and Subnetting I
### *Logical Layer-3 Addressing, Binary Foundations, Network Boundaries & CIDR*

<p align="center">
  <img src="https://img.shields.io/badge/Module-03-0052CC?style=for-the-badge&logo=target" alt="Module 03" />
  <img src="https://img.shields.io/badge/Topics-8_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 02: Network Packets and Layered Communication**](../Module%2002%3A%20Network%20Packets%20and%20Layered%20Communication/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 04: IP Addresses and Subnetting II**](../Module%2004%3A%20IP%20Addresses%20and%20Subnetting%20II/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

How does a router know where in the world to deliver your packet? Welcome to IP addressing! In this module, you will master the fundamental building blocks of IPv4: 32-bit binary representation, network vs. host boundaries, subnet masks, CIDR prefix notation, and private vs. public address spaces.

---

## 🎯 What You Will Master

- [x] **Explain why logical Layer-3 IP addresses are indispensable for internetwork routing alongside Layer-2 MAC addresses.**
- [x] **Dissect 32-bit IPv4 addresses in dotted-decimal notation and calculate total address space ($2^{32} \approx 4.29$ billion).**
- [x] **Perform fast decimal-to-binary and binary-to-decimal conversions for byte octets (powers of 2: 128, 64, 32, 16, 8, 4, 2, 1).**
- [x] **Separate an IP address into its two fundamental components: Network ID and Host ID.**
- [x] **Master the subnet mask and use bitwise AND operations to determine whether two devices are on the same local subnet.**
- [x] **Understand modern Classless Inter-Domain Routing (CIDR) notation (`/24`, `/16`, `/28`) and why it replaced legacy classful addressing.**
- [x] **Differentiate between globally routable Public IPs and RFC 1918 Private IP address spaces (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`).**
- [x] **Explain the special role of loopback addresses (`127.0.0.1` / `127.0.0.0/8`) and how `localhost` works inside the OS kernel.**

---

## 🧠 Architectural Mental Model

```
IPv4 Address:  192.168.1.75 / 24
Binary Form:   11000000 . 10101000 . 00000001 . 01001011

Subnet Mask:   255.255.255.0  (/24 = 24 ones, 8 zeros)
Binary Mask:   11111111 . 11111111 . 11111111 . 00000000
               | <-------- NETWORK ID (24 bits) -------> | <-- HOST (8b) -> |

Bitwise AND:
               11000000 . 10101000 . 00000001 . 01001011  (192.168.1.75)
          AND  11111111 . 11111111 . 11111111 . 00000000  (255.255.255.0)
          ---------------------------------------------------------------
Result:        11000000 . 10101000 . 00000001 . 00000000  (192.168.1.0 = Network ID)
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [Why IP Addresses?](./01_Why_IP_Addresses.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [IPv4 🌐](./02_IPv4.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [Binary Refresher 🔢](./03_Binary_Refresher.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [Network ID vs Host ID](./04_Network_ID_vs_Host_ID.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [Subnet Mask](./05_Subnet_Mask.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [CIDR Notation 🌐](./06_CIDR_Notation.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **07** | [Public vs Private IP Addresses 🌐](./07_Public_vs_Private_IP.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **08** | [Loopback Address 🔄](./08_Loopback_Address.md) | `- [ ]` Ready | ⏱️ 16 mins |

---

## 💡 Practical Challenge & Self-Assessment

**Bitwise AND Challenge:** Given IP address `10.20.30.45` and Subnet Mask `255.255.240.0` (`/20`), what is the Network ID?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- Look at the 3rd octet: 30 vs 240.
- 30 in binary: `00011110`
- 240 in binary: `11110000`
- Bitwise AND: `00010000` = 16 in decimal.
- Therefore, the Network ID is **`10.20.16.0/20`**.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: Why IP Addresses?**](./01_Why_IP_Addresses.md) 👈

---

| ⬅️ [**Module 02: Network Packets and Layered Communication**](../Module%2002%3A%20Network%20Packets%20and%20Layered%20Communication/README.md) | 📑 [**Back to Top**](#-module-03-) | [**Module 04: IP Addresses and Subnetting II**](../Module%2004%3A%20IP%20Addresses%20and%20Subnetting%20II/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
