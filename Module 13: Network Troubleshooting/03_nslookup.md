<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 13: Network Troubleshooting* • **Topic 03 of 10** (Global #092)

| [⬅️ Previous: 🗺️ `traceroute`](./02_traceroute.md) | 📑 [**Module Overview**](./README.md) | [Next: 🧪 `dig` — DNS Investigation Tool ➡️](./04_dig.md) |
| :--- | :---: | ---: |

---

</div>

# 🔎 `nslookup`

Now we're focusing specifically on **DNS troubleshooting**.

You've already learned how DNS works:

```text
Domain name
    ↓
DNS
    ↓
IP address
```

`nslookup` lets you **query DNS and inspect the answer**.

> **`nslookup` = a command-line tool for querying DNS records.**

---

# 1. Basic Usage

Run:

```bash
nslookup google.com
```

You may see something like:

```text
Server:         192.168.1.1
Address:        192.168.1.1#53

Non-authoritative answer:
Name:   google.com
Address: 142.250.x.x
```

This tells you that DNS resolved:

```text
google.com
     ↓
142.250.x.x
```

---

# 2. What is `Server`?

Example:

```text
Server: 192.168.1.1
Address: 192.168.1.1#53
```

This is the **DNS resolver that answered your query**.

In a home network, it may be your router:

```text
Laptop
   ↓
192.168.1.1
   ↓
DNS resolver
```

That resolver might itself forward queries to an upstream DNS service.

---

# 3. What does `#53` mean?

Remember:

```text
DNS → Port 53
```

So:

```text
192.168.1.1#53
```

means:

```text
IP   = 192.168.1.1
Port = 53
```

DNS commonly uses UDP port 53 for ordinary queries, though TCP 53 is also used in certain situations.

---

# 4. "Non-authoritative answer"

You may see:

```text
Non-authoritative answer:
```

This generally means the response came from a **recursive resolver/cache**, rather than directly from the authoritative DNS server for that domain.

Remember the distinction:

```text
Recursive Resolver
        ↓
Finds/caches answers
```

versus:

```text
Authoritative Server
        ↓
Source of DNS records for the zone
```

So:

> **Non-authoritative doesn't mean the answer is wrong.**

It simply indicates that the responding server isn't authoritative for that domain.

---

# 5. Query Different Record Types

`nslookup` can also inspect different DNS record types.

### A record — IPv4

```bash
nslookup -type=A example.com
```

Example:

```text
example.com → 93.184.216.34
```

---

### AAAA record — IPv6

```bash
nslookup -type=AAAA example.com
```

This asks for IPv6 addresses.

---

### MX record — Mail servers

```bash
nslookup -type=MX example.com
```

This asks:

> "Which mail servers handle email for this domain?"

---

### NS record — Name servers

```bash
nslookup -type=NS example.com
```

This helps identify the domain's authoritative name servers.

---

# 6. Why is `nslookup` Useful for Troubleshooting?

Imagine:

```bash
ping google.com
```

fails.

Before blaming the network, test DNS:

```bash
nslookup google.com
```

### Case 1 — DNS works

```text
google.com → IP address
```

Then DNS resolution is working, so the problem may be elsewhere.

### Case 2 — DNS fails

You might get something like:

```text
connection timed out
```

or:

```text
server can't find ...
```

Then DNS itself may be the problem.

---

# 7. Test DNS Without Using Your Normal Resolver

You can specify a DNS server.

For example:

```bash
nslookup google.com 8.8.8.8
```

Now you're asking:

> "Ask the DNS server at `8.8.8.8` for this domain."

This is extremely useful for troubleshooting.

Suppose:

```text
nslookup google.com
```

fails, but:

```bash
nslookup google.com 8.8.8.8
```

works.

That gives you evidence that your **configured/local DNS resolver path may be the issue**, rather than DNS resolution in general.

---

# 8. `nslookup` vs `ping`

This distinction is important.

### `ping`

```bash
ping google.com
```

Tests:

```text
DNS resolution
+
IP connectivity
+
RTT
```

assuming the hostname is successfully resolved and ICMP is permitted.

### `nslookup`

```bash
nslookup google.com
```

Focuses on:

```text
DNS resolution
```

So:

```text
ping       → "Can I reach it?"
nslookup   → "What does DNS say?"
```

---

# 9. `nslookup` vs `dig`

Both can query DNS:

```text
nslookup
dig
```

But `dig` generally provides **more detailed and structured DNS information**, making it especially popular for advanced DNS troubleshooting.

For now:

```text
nslookup → simple/easy DNS lookup
dig      → detailed DNS investigation
```

We'll do `dig` next.

---

# 10. A Practical Troubleshooting Sequence 🛠️

Suppose a website isn't opening.

You can think:

```text
1. ping 127.0.0.1
        ↓
2. ping gateway
        ↓
3. ping remote IP
        ↓
4. nslookup example.com
        ↓
5. Check application/HTTP
```

For DNS specifically:

```bash
nslookup example.com
```

Then perhaps:

```bash
nslookup example.com 1.1.1.1
```

Comparing the results can help isolate a resolver problem.

---

# 🧠 Best Mental Model

Think of `nslookup` as calling the **DNS directory service**:

```text
You:
"What's the address for example.com?"

DNS resolver:
"Here's the IP."
```

The command simply lets you perform that conversation directly from the terminal.

---

# 🔥 Must Remember

```text
nslookup
   ↓
DNS query tool
   ↓
Domain → DNS record
```

Useful examples:

```bash
nslookup google.com
nslookup -type=A google.com
nslookup -type=AAAA google.com
nslookup -type=MX google.com
nslookup -type=NS google.com
nslookup google.com 8.8.8.8
```

And:

```text
ping     → connectivity
nslookup → DNS
```

### One-line definition:

> **`nslookup` is a command-line utility used to query DNS and inspect how domain names resolve to DNS records.**

---

## Module 13 Progress

✅ `ping` 🏓  
✅ `traceroute` 🗺️  
✅ **`nslookup` 🔎**  
⬜ `dig`  
⬜ `netstat`  
⬜ `ss`  
⬜ `tcpdump`  
⬜ Wireshark

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *How do you query a specific nameserver (e.g. Cloudflare's `1.1.1.1`) using `nslookup`?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> Run `nslookup example.com 1.1.1.1`.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🗺️ `traceroute`](./02_traceroute.md) | [**Module 13: Network Troubleshooting**](./README.md) | [Next: 🧪 `dig` — DNS Investigation Tool ➡️](./04_dig.md) |

<div align="center">
  <br/>
  <code>[████████░░] 82% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
