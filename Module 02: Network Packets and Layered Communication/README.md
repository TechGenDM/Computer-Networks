<div align="center">

# 🌐 Module 02: Network Packets and Layered Communication
### *Protocol Rules, Encapsulation, Framing, MTU & MAC Addressing*

<p align="center">
  <img src="https://img.shields.io/badge/Module-02-0052CC?style=for-the-badge&logo=target" alt="Module 02" />
  <img src="https://img.shields.io/badge/Topics-10_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 01: Introduction to Computer Networks**](../Module%2001%3A%20Introduction%20to%20Computer%20Networks/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 03: IP Addresses and Subnetting I**](../Module%2003%3A%20IP%20Addresses%20and%20Subnetting%20I/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Now that you understand the big picture of networks, let's zoom in on what actually travels across the wire. This module unpacks the heartbeat of data communication: how protocols enforce rules, how data is encapsulated into packets and frames down the stack, and how hardware MAC addresses deliver bits on local links.

---

## 🎯 What You Will Master

- [x] **Define what a network protocol is and understand the syntax, semantics, and timing of communication rules.**
- [x] **Trace the step-by-step encapsulation of user application data down the stack (adding L4, L3, and L2 headers).**
- [x] **Trace the decapsulation process on the receiving host as headers are stripped and payloads are passed upward.**
- [x] **Demystify packet anatomy: distinguish between protocol headers and payload data.**
- [x] **Master PDU (Protocol Data Unit) terminology: Segments (Transport), Packets/Datagrams (Network), Frames (Data Link), Bits (Physical).**
- [x] **Examine Ethernet frame layout: Preamble, SFD, Destination/Source MAC, EtherType, Payload, and CRC/FCS.**
- [x] **Understand MTU (Maximum Transmission Unit), the standard 1500-byte limit, and link-layer constraints.**
- [x] **Analyze IP packet fragmentation: how packets exceeding link MTU are split, flagged (DF, MF), and reassembled.**
- [x] **Deep dive into MAC addresses: 48-bit physical addressing, OUI vendor bytes, and why MACs cannot replace IPs for global routing.**

---

## 🧠 Architectural Mental Model

```
[Application Layer]       | Data: "GET /index.html" |
                                   |
                                   v  (Add TCP Header)
[Transport Layer]         | TCP Header | Data |                         --> [SEGMENT]
                                   |
                                   v  (Add IP Header)
[Network Layer]           | IP Header  | TCP Header | Data |            --> [PACKET]
                                   |
                                   v  (Add Ethernet Header & FCS Trailer)
[Data Link Layer]         | Eth Header | IP Header | TCP | Data | FCS | --> [FRAME]
                                   |
                                   v  (Transmitted as physical signals)
[Physical Layer]          0 1 0 1 1 0 0 1 0 1 0 1 1 1 0 0 1 ...          --> [BITS]
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [Protocols — The Rules of Network Communication 🤝](./01_Protocols.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [Encapsulation 📦➕](./02_Encapsulation.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [Decapsulation 📦➡️](./03_Decapsulation.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [Headers 📋](./04_Headers.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [Payload 📦](./05_Payload.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [Frames vs Packets vs Segments 📦🔄](./06_Frames_vs_Packets_vs_Segments.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **07** | [Ethernet Frame 🔗](./07_Ethernet_Frame.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **08** | [MTU — Maximum Transmission Unit 📏](./08_MTU.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **09** | [IP Fragmentation 📦✂️](./09_Fragmentation.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **10** | [MAC Addresses 🔗](./10_MAC_Addresses.md) | `- [ ]` Ready | ⏱️ 12 mins |

---

## 💡 Practical Challenge & Self-Assessment

**PDU Identification Challenge:** A network engineer captures traffic and sees a chunk of data with an EtherType of `0x0800` containing source MAC `00:1A:2B:3C:4D:5E`. What layer PDU is this engineer inspecting, and what protocol does the payload contain?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- The PDU is a **Data Link Layer Frame** (specifically an Ethernet II Frame), identified by the MAC address and frame structure.
- EtherType `0x0800` indicates that the payload inside this Ethernet frame is an **IPv4 Packet**.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: Protocols — The Rules of Network Communication 🤝**](./01_Protocols.md) 👈

---

| ⬅️ [**Module 01: Introduction to Computer Networks**](../Module%2001%3A%20Introduction%20to%20Computer%20Networks/README.md) | 📑 [**Back to Top**](#-module-02-) | [**Module 03: IP Addresses and Subnetting I**](../Module%2003%3A%20IP%20Addresses%20and%20Subnetting%20I/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
