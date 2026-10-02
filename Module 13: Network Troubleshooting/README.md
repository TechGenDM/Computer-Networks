<div align="center">

# 🌐 Module 13: Network Troubleshooting
### *The Diagnostic Toolkit: Command-Line Utilities, Packet Sniffing & Debugging Playbook*

<p align="center">
  <img src="https://img.shields.io/badge/Module-13-0052CC?style=for-the-badge&logo=target" alt="Module 13" />
  <img src="https://img.shields.io/badge/Topics-10_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 12: TCP, UDP and Socket Programming II**](../Module%2012%3A%20TCP%2C%20UDP%20and%20Socket%20Programming%20II/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 14: Complete Internet Journey + Revision**](../Module%2014%3A%20Complete%20Internet%20Journey%20%2B%20Revision/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Theory meets reality in the trenches of network debugging. When an API call fails, latency spikes, or DNS stops resolving, how do you diagnose the root cause? In this hands-on module, master the essential CLI toolkit (`ping`, `traceroute`, `nslookup`, `dig`, `netstat`, `ss`, `tcpdump`, `wireshark`) and systematic troubleshooting workflows.

---

## 🎯 What You Will Master

- [x] **Use `ping` to test end-to-end reachability, evaluate round-trip times (RTT), detect packet loss, and understand ICMP Echo Request/Reply.**
- [x] **Use `traceroute` (or `tracert`) to map network hops via TTL incrementing and identify which specific router or ISP is failing.**
- [x] **Query and troubleshoot DNS records using `nslookup` and the powerful `dig` utility (evaluating A, AAAA, CNAME, MX, TXT records, and TTLs).**
- [x] **Inspect open sockets, listening ports, and active TCP connections on your local operating system using `netstat` and modern Linux `ss`.**
- [x] **Capture live network packets from the command line using `tcpdump` with Berkeley Packet Filters (BPF).**
- [x] **Analyze raw packet captures in Wireshark: inspect protocol headers down the stack, follow TCP streams, and spot retransmissions.**
- [x] **Follow the professional Troubleshooting Playbook to methodically isolate Layer 1 through Layer 7 networking failures.**

---

## 🧠 Architectural Mental Model

```
Layered Troubleshooting Triage Workflow:

   "Can't connect to api.example.com"
                 │
                 ▼
       [ 1. Physical / Link ]  ──► Is interface UP? (`ip a`, `ifconfig`, Wi-Fi connected?)
                 │ YES
                 ▼
       [ 2. Local Gateway   ]  ──► Can you ping your Default Gateway? (`ping 192.168.1.1`)
                 │ YES
                 ▼
       [ 3. External IP     ]  ──► Can you ping an external public IP? (`ping 8.8.8.8`)
                 │ YES
                 ▼
       [ 4. DNS Resolution  ]  ──► Can you resolve the hostname? (`dig api.example.com`)
                 │ YES
                 ▼
       [ 5. TCP Port Reach  ]  ──► Can you establish a TCP handshake? (`curl -v`, `nc -zv`)
                 │ YES
                 ▼
       [ 6. TLS / HTTP App  ]  ──► Inspect HTTP response code & TLS cert (`curl -ILs`)
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [🛠️ Topic 1 — `ping`](./01_ping.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [🗺️ `traceroute`](./02_traceroute.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [🔎 `nslookup`](./03_nslookup.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [🧪 `dig` — DNS Investigation Tool](./04_dig.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [📊 `netstat`](./05_netstat.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [🔌 `ss` — Socket Statistics](./06_ss.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **07** | [🐙 `tcpdump` — Packet Capture](./07_tcpdump.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **08** | [🦈 Wireshark — Packet Analysis](./08_wireshark.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **09** | [🔬 Reading Packet Captures](./09_Reading_Packet_Captures.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **10** | [🛠️ Common Networking Problems](./10_Common_Networking_Problems.md) | `- [ ]` Ready | ⏱️ 12 mins |

---

## 💡 Practical Challenge & Self-Assessment

**Troubleshooting Triage Challenge:** A user reports: *"I can ping 8.8.8.8 successfully, but typing `google.com` into my browser says 'Server Not Found'."* At what layer and service is the failure occurring?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- The failure is in **DNS resolution (Application Layer)**!
- Pinging `8.8.8.8` proves that Layers 1, 2, 3 (Physical, Data Link, Network/IP) and Default Gateway routing are working perfectly. The client machine simply cannot resolve the domain name to an IP address.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: 🛠️ Topic 1 — `ping`**](./01_ping.md) 👈

---

| ⬅️ [**Module 12: TCP, UDP and Socket Programming II**](../Module%2012%3A%20TCP%2C%20UDP%20and%20Socket%20Programming%20II/README.md) | 📑 [**Back to Top**](#-module-13-) | [**Module 14: Complete Internet Journey + Revision**](../Module%2014%3A%20Complete%20Internet%20Journey%20%2B%20Revision/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
