<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 10: DNS and Internet Applications* • **Topic 01 of 13** (Global #058)

| [⬅️ Prev Module: 🌍 BGP — Border Gateway Protocol](../Module%2009%3A%20Routing%20Protocols/03_BGP.md) | 📑 [**Module Overview**](./README.md) | [Next: 🌳 DNS Hierarchy ➡️](./02_DNS_Hierarchy.md) |
| :--- | :---: | ---: |

---

</div>

# 1. Why DNS? (Domain Name System)

Great, we're moving from **routing** into the **application side of the Internet**.

Until now, we learned how packets find their way through networks. Now we'll answer:

> **How does typing `google.com` actually turn into a connection to a server?**

This module has **12 topics**:

1. **Why DNS?**
2. DNS Hierarchy
3. Recursive Resolver
4. Root Server
5. TLD Server
6. Caching
7. URL Anatomy
8. HTTP Basics
9. HTTP Request
10. HTTP Response
11. Status Codes
12. Cookies
13. Sessions

Small correction: the screenshot actually lists **13 topics**, because Cookies and Sessions are separate.

---

# 1. Why DNS?

Let's start with the simplest question.

Computers communicate using **IP addresses**.

For example, a server might have:

```text
142.250.195.14
```

You could technically type:

```text
https://142.250.195.14
```

But imagine having to remember IP addresses for every website:

```text
Google      → 142.250.x.x
YouTube     → 142.250.x.x
Amazon      → 18.x.x.x
Netflix     → 52.x.x.x
GitHub      → 140.82.x.x
```

😵‍💫 Impossible.

Humans prefer names:

```text
google.com
youtube.com
amazon.com
netflix.com
github.com
```

So we need a system that translates:

```text
Domain Name
     ↓
IP Address
```

That system is:

# **DNS — Domain Name System**

---

# 2. DNS is like the Internet's phonebook 📞

Imagine your phone:

```text
"Mom"
  ↓
+91 XXXXX XXXXX
```

You don't remember the phone number every time.

Similarly:

```text
google.com
     ↓
142.250.x.x
```

DNS performs the translation.

So:

> **DNS maps domain names to IP addresses.**

---

# 3. A very important distinction

DNS does **not** carry your webpage.

DNS answers something like:

> "Which IP address corresponds to `example.com`?"

Then another protocol—usually **HTTP/HTTPS**—is used to actually communicate with the web server.

So:

```text
DNS
 ↓
Find server IP
 ↓
HTTP/HTTPS
 ↓
Request webpage
 ↓
Response
```

This distinction is extremely important.

---

# 4. What happens when you type `google.com`?

Conceptually:

```text
You type:

google.com
    ↓
Your device asks DNS:
"What IP belongs to google.com?"
    ↓
DNS resolution
    ↓
IP address
    ↓
Connect to server
    ↓
HTTP/HTTPS request
    ↓
Web page
```

But DNS resolution itself has a hierarchy.

And that's our next major concept.

---

# 🧠 Mental model

Think of DNS as:

```text
Human-friendly name
        ↓
       DNS
        ↓
Machine-friendly IP address
```

### One-line definition:

> **DNS is a distributed naming system that translates domain names into IP addresses and also stores other types of DNS records.**

---

## Module 9 progress

✅ **Why DNS?**  
⬜ DNS Hierarchy  
⬜ Recursive Resolver  
⬜ Root Server  
⬜ TLD Server  
⬜ Caching  
⬜ URL Anatomy  
⬜ HTTP Basics  
⬜ HTTP Request  
⬜ HTTP Response  
⬜ Status Codes

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *Why does DNS use UDP port 53 for standard lookups rather than TCP?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> UDP avoids the 3-way handshake overhead, completing lookups in a single round-trip. DNS falls back to TCP only when responses exceed 512 bytes (or for zone transfers).
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Prev Module: 🌍 BGP — Border Gateway Protocol](../Module%2009%3A%20Routing%20Protocols/03_BGP.md) | [**Module 10: DNS and Internet Applications**](./README.md) | [Next: 🌳 DNS Hierarchy ➡️](./02_DNS_Hierarchy.md) |

<div align="center">
  <br/>
  <code>[█████░░░░░] 51% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
