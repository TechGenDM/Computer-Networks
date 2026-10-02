<div align="center">

# 🌐 Module 11: TCP, UDP and Socket Programming
### *Transport Layer Architecture, Ports, Connection Handshakes, Reliability & Flow Control*

<p align="center">
  <img src="https://img.shields.io/badge/Module-11-0052CC?style=for-the-badge&logo=target" alt="Module 11" />
  <img src="https://img.shields.io/badge/Topics-8_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 10: DNS and Internet Applications**](../Module%2010%3A%20DNS%20and%20Internet%20Applications/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 12: TCP, UDP and Socket Programming II**](../Module%2012%3A%20TCP%2C%20UDP%20and%20Socket%20Programming%20II/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

The Network Layer gets packets between machines, but the Transport Layer delivers data between specific running processes. This module explores ports and sockets, contrasts TCP's reliable byte-stream guarantee with UDP's lightweight datagram simplicity, dissects the Three-Way Handshake, and unpacks TCP flow control.

---

## 🎯 What You Will Master

- [x] **Explain the Transport Layer's core role: process-to-process communication, port multiplexing, and demultiplexing.**
- [x] **Differentiate between Well-Known ports (0-1023), Registered ports (1024-49151), and Ephemeral/Dynamic ports (49152-65535).**
- [x] **Contrast TCP (connection-oriented, reliable, ordered, flow-controlled) with UDP (connectionless, lightweight, low-latency, unordered).**
- [x] **Dissect the TCP Three-Way Handshake step-by-step: SYN (seq=x) -> SYN-ACK (seq=y, ack=x+1) -> ACK (ack=y+1).**
- [x] **Analyze TCP connection teardown: Four-Way Close (FIN, ACK, FIN, ACK), half-close state, and the critical `TIME_WAIT` state.**
- [x] **Understand TCP reliability mechanics: Sequence Numbers, Cumulative Acknowledgments, Retransmission Timers (RTO), and Fast Retransmit.**
- [x] **Master TCP Flow Control: how the receiver advertises its buffer capacity via the Receive Window (`rwnd`) to prevent buffer overflow.**

---

## 🧠 Architectural Mental Model

```
TCP Three-Way Handshake & Four-Way Teardown:

    CLIENT                                            SERVER
      |                                                 |
      | -------------- 1. SYN (seq=x) ----------------> |  (Listen -> SYN_RCVD)
      |                                                 |
      | <--------- 2. SYN-ACK (seq=y, ack=x+1) -------- |
      |                                                 |
      | -------------- 3. ACK (ack=y+1) --------------> |
      |                                                 |
[ESTABLISHED]                                     [ESTABLISHED]
      ~                                                 ~
      | <====== Full-Duplex Data Transfer ======>       |
      ~                                                 ~
      | -------------- 1. FIN (seq=u) ----------------> |  (Server ACK)
      | <------------- 2. ACK (ack=u+1) --------------- |
      |                                                 |
      | <------------- 3. FIN (seq=v) ----------------- |  (Server closes)
      | -------------- 4. ACK (ack=v+1) --------------> |
  [TIME_WAIT]                                        [CLOSED]
(Waits 2MSL then closes)
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [🧱 Topic 1 — Transport Layer](./01_Transport_Layer.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [🔌 Ports](./02_Ports.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [🛡️ TCP — Transmission Control Protocol](./03_TCP.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [⚡ UDP — User Datagram Protocol](./04_UDP.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [🤝 TCP Three-Way Handshake](./05_Three_Way_Handshake.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [🔚 TCP Four-Way Close](./06_Four_Way_Close.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **07** | [🛡️ TCP Reliability](./07_Reliability.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **08** | [🌊 TCP Flow Control](./08_Flow_Control.md) | `- [ ]` Ready | ⏱️ 16 mins |

---

## 💡 Practical Challenge & Self-Assessment

**TCP State Machine Challenge:** Why must the client enter the `TIME_WAIT` state and wait for $2 \times \text{MSL}$ (Maximum Segment Lifetime) before completely closing a TCP connection?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- To ensure the server receives the final **ACK** (if it was lost, the server can retransmit its FIN).
- To allow any old, delayed duplicate segments from the connection to "die out" in the network so they don't corrupt a future connection using the same socket tuple.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: 🧱 Topic 1 — Transport Layer**](./01_Transport_Layer.md) 👈

---

| ⬅️ [**Module 10: DNS and Internet Applications**](../Module%2010%3A%20DNS%20and%20Internet%20Applications/README.md) | 📑 [**Back to Top**](#-module-11-) | [**Module 12: TCP, UDP and Socket Programming II**](../Module%2012%3A%20TCP%2C%20UDP%20and%20Socket%20Programming%20II/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
