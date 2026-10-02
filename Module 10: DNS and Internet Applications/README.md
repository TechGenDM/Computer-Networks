<div align="center">

# 🌐 Module 10: DNS and Internet Applications
### *Name Resolution Hierarchy, Caching, URL Anatomy, HTTP/1.1 Protocols & State*

<p align="center">
  <img src="https://img.shields.io/badge/Module-10-0052CC?style=for-the-badge&logo=target" alt="Module 10" />
  <img src="https://img.shields.io/badge/Topics-13_Chapters-blue?style=for-the-badge" alt="Topics" />
  <img src="https://img.shields.io/badge/Hands--On-Interactive_Quizzes-success?style=for-the-badge&logo=quizlet" alt="Quizzes" />
</p>

---

| ⬅️ [**Module 09: Routing Protocols**](../Module%2009%3A%20Routing%20Protocols/README.md) | 🏠 [**Repository Roadmap**](../README.md) | [**Module 11: TCP, UDP and Socket Programming**](../Module%2011%3A%20TCP%2C%20UDP%20and%20Socket%20Programming/README.md) ➡️ |
| :--- | :---: | ---: |

---

</div>

## 📖 Module Overview

Welcome to the Application Layer! When you type a URL into your browser, how does a human-readable name like 'google.com' translate into an IP address? And how does HTTP request and render the webpage? In this module, explore the distributed DNS hierarchy, URL anatomy, HTTP requests/responses, status codes, cookies, and sessions.

---

## 🎯 What You Will Master

- [x] **Explain why DNS is essential and how it maps human-readable domain names to machine-routable IP addresses.**
- [x] **Navigate the 4-tier DNS hierarchy: Root Name Servers (`.`), Top-Level Domain (TLD) Servers (`.com`), Authoritative Name Servers, and Recursive Resolvers.**
- [x] **Differentiate between recursive DNS queries (client to resolver) and iterative DNS queries (resolver to root/TLD/authoritative).**
- [x] **Analyze DNS caching at the browser, operating system, and ISP resolver levels, and understand the Time-To-Live (TTL) field.**
- [x] **Deconstruct a complete URL: scheme (`https://`), authority/host, port, path, query parameters, and fragment identifier.**
- [x] **Master the fundamentals of HTTP: stateless client-server request-response protocol over TCP.**
- [x] **Format and inspect raw HTTP requests: Request Line (Method, URI, HTTP Version), Headers, and Message Body.**
- [x] **Format and inspect raw HTTP responses: Status Line, Response Headers, and Entity Body.**
- [x] **Categorize HTTP status codes: 1xx (Informational), 2xx (Success), 3xx (Redirection), 4xx (Client Error), 5xx (Server Error).**
- [x] **Understand how HTTP Cookies preserve state across requests (`Set-Cookie`, `HttpOnly`, `Secure`, `SameSite`) and compare them with server-side Sessions.**

---

## 🧠 Architectural Mental Model

```
Iterative DNS Lookup Resolution:

[Your Browser] 
      │ 1. "What is example.com?" (Recursive)
      ▼
[Recursive Resolver (ISP / 8.8.8.8)] 
      │
      ├─► 2. "Where is example.com?" ────► [Root Name Server (.)]
      │◄── 3. "Ask the .com TLD server" ───┘
      │
      ├─► 4. "Where is example.com?" ────► [.com TLD Server]
      │◄── 5. "Ask ns1.example.com" ──────┘
      │
      ├─► 6. "What is example.com's IP?" ─► [Authoritative NS (ns1.example.com)]
      │◄── 7. "IP is 93.184.216.34" ──────┘
      │
      ▼ 8. "The IP is 93.184.216.34" (Cached!)
[Your Browser]
```

---

## 📑 Interactive Module Syllabus & Reading Order

> **How to learn:** Start with Topic `01` and follow the sequential links at the bottom of each file. Every chapter builds directly on the previous one!

| Chapter | Topic & Direct Link | Learning Status | Est. Read Time |
| :---: | :--- | :---: | :---: |
| **01** | [Why DNS? (Domain Name System)](./01_Why_DNS.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **02** | [🌳 DNS Hierarchy](./02_DNS_Hierarchy.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **03** | [Recursive Resolver 🔍](./03_Recursive_Resolver.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **04** | [Root Name Servers 🌳](./04_Root_Server.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **05** | [TLD (Top-Level Domain) Server 🌐](./05_TLD_Server.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **06** | [DNS Caching ⚡](./06_DNS_Caching.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **07** | [Anatomy of a URL 🔗](./07_URL_Anatomy.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **08** | [HTTP Basics 🌐](./08_HTTP_Basics.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **09** | [HTTP Request Anatomy 📤](./09_HTTP_Request.md) | `- [ ]` Ready | ⏱️ 10 mins |
| **10** | [📥 HTTP Response](./10_HTTP_Response.md) | `- [ ]` Ready | ⏱️ 12 mins |
| **11** | [🚦 HTTP Status Codes](./11_HTTP_Status_Codes.md) | `- [ ]` Ready | ⏱️ 14 mins |
| **12** | [🍪 Cookies](./12_Cookies.md) | `- [ ]` Ready | ⏱️ 16 mins |
| **13** | [🔐 Sessions](./13_Sessions.md) | `- [ ]` Ready | ⏱️ 10 mins |

---

## 💡 Practical Challenge & Self-Assessment

**Cookie Security Challenge:** What two security flags should always be set on session cookies to protect them against Cross-Site Scripting (XSS) and eavesdropping?

<details><summary>💡 <b>Click to Reveal Solution</b></summary>

- **`HttpOnly`**: Prevents client-side JavaScript (e.g., `document.cookie`) from reading the cookie, mitigating XSS attacks.
- **`Secure`**: Ensures the cookie is only transmitted over encrypted HTTPS connections, preventing plaintext snooping.
</details>

---

## 🚀 Get Started Now

Ready to dive in? Click below to begin the first chapter of this module:

<div align="center">

### 👉 [**Start Chapter 01: Why DNS? (Domain Name System)**](./01_Why_DNS.md) 👈

---

| ⬅️ [**Module 09: Routing Protocols**](../Module%2009%3A%20Routing%20Protocols/README.md) | 📑 [**Back to Top**](#-module-10-) | [**Module 11: TCP, UDP and Socket Programming**](../Module%2011%3A%20TCP%2C%20UDP%20and%20Socket%20Programming/README.md) ➡️ |
| :--- | :---: | ---: |

</div>
