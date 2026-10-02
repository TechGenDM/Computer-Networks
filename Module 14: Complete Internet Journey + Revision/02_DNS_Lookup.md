<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 14: Complete Internet Journey + Revision* • **Topic 02 of 13** (Global #101)

| [⬅️ Previous: 🧭 Topic 1 — Browser Cache](./01_Browser_Cache.md) | 📑 [**Module Overview**](./README.md) | [Next: 🔎 ARP — In the Complete Internet Journey ➡️](./03_ARP.md) |
| :--- | :---: | ---: |

---

</div>

# 🔎 DNS Lookup

Now we move to the **second step of the Complete Internet Journey**.

You type:

```text id="n5w72x"
https://example.com
```

The browser needs to know:

> **"Which IP address belongs to `example.com`?"**

That's what **DNS lookup** solves.

---

# 1. The Basic Flow

At a high level:

```text id="86f15c"
example.com
     ↓
DNS Lookup
     ↓
IP Address
     ↓
Connect to that IP
```

But the browser doesn't necessarily contact the root DNS server every time.

There are **caches** along the way.

---

# 2. First: Check Local Information

The browser/operating system may already know the answer.

A simplified flow is:

```text id="k7xgq1"
Browser / OS
     ↓
DNS cache?
     │
   ┌─┴─┐
  YES  NO
   ↓    ↓
Use    Ask configured
cache  DNS resolver
```

So a repeated visit can be much faster.

---

# 3. Ask the Recursive Resolver

If the local cache doesn't have a usable answer, the device sends a DNS query to its **configured recursive resolver**.

For example:

```text id="5k3w8f"
Laptop
   │
   │ "What is example.com's IP?"
   ↓
Recursive Resolver
```

That resolver might be:

```text id="8lsg44"
Your ISP's DNS
Your router's forwarding resolver
A public DNS service
An organization's DNS resolver
```

---

# 4. Resolver Checks Its Own Cache

The recursive resolver first checks whether it already knows the answer.

```text id="qz7qxa"
Recursive Resolver
       ↓
Cache?
   ┌───┴───┐
  YES      NO
   ↓        ↓
Return    Start DNS lookup
answer
```

If the answer is cached and its TTL hasn't expired:

```text id="2q8o4j"
example.com
     ↓
93.184.216.34
```

The resolver can return it immediately.

---

# 5. If Cache Miss → Root Server 🌍

If the resolver doesn't have the answer, it starts the DNS hierarchy lookup.

For:

```text id="9a2w6z"
example.com
```

the resolver can ask a root server:

> "Where should I look for `.com`?"

The root doesn't normally provide the final IP.

Instead, it directs the resolver toward the **`.com` TLD servers**.

```text id="0xqf4r"
Recursive Resolver
       ↓
      Root
       ↓
    .com TLD
```

---

# 6. TLD Server

The resolver then asks a `.com` TLD server:

> "Which authoritative name servers handle `example.com`?"

The TLD infrastructure provides the delegation information.

```text id="32z4pb"
Resolver
   ↓
.com TLD
   ↓
Authoritative NS for example.com
```

---

# 7. Authoritative DNS Server

Now the resolver queries the authoritative server for the actual record.

For example:

```text id="ui4b3a"
Resolver
   ↓
Authoritative Server
   ↓
A record
   ↓
93.184.216.34
```

This is where the actual DNS record for the zone comes from.

---

# 8. Resolver Returns the Answer

The recursive resolver now sends the result back to your computer:

```text id="y6v4lu"
Authoritative Server
       ↓
Recursive Resolver
       ↓
Laptop
       ↓
93.184.216.34
```

The resolver may also cache the result according to its TTL.

---

# 9. Complete DNS Lookup

Here's the full journey:

```text id="m4h0j7"
Browser wants:
example.com
      ↓
Browser/OS cache?
      ↓
   No usable answer
      ↓
Recursive Resolver
      ↓
Resolver cache?
      ↓
   No usable answer
      ↓
Root DNS
      ↓
.com TLD
      ↓
Authoritative DNS
      ↓
A/AAAA record
      ↓
Recursive Resolver
      ↓
Your computer
      ↓
IP address
```

🔥 This is the DNS hierarchy you learned earlier, now embedded into the actual Internet journey.

---

# 10. A and AAAA Records

The browser may need an IPv4 or IPv6 address.

### A

```text id="k9zq2g"
A → IPv4
```

Example:

```text
example.com → 93.184.216.34
```

### AAAA

```text id="5j9zvp"
AAAA → IPv6
```

Example:

```text
example.com → 2001:db8:...
```

Modern systems may obtain both and then use the available connectivity according to their networking behavior.

---

# 11. Does DNS Always Go Root → TLD → Authoritative?

**No.**

This is an important distinction.

That is the **hierarchical lookup path when the resolver needs to discover the answer**.

In practice, caching means the resolver may already know:

```text id="8c1l4m"
example.com → IP
```

and simply return it.

So:

```text id="0y2jrl"
DNS lookup
≠
Always contact root server
```

Caching is one of the main reasons DNS can scale efficiently.

---

# 12. What Protocol Carries the DNS Query?

Ordinary DNS commonly uses:

```text id="zsq2xs"
UDP port 53
```

TCP port 53 is also used in certain circumstances.

There are also newer encrypted DNS mechanisms such as:

```text id="w4m4yo"
DoT → DNS over TLS
DoH → DNS over HTTPS
```

These change how DNS queries are transported, but the DNS naming system itself still provides the resolution service.

For your current module, focus on the normal DNS flow first.

---

# 13. What Happens After DNS?

This is where your Module 14 journey continues.

Once the browser knows:

```text id="eqxv7s"
example.com
      ↓
93.184.216.34
```

it can proceed toward establishing communication with that IP.

Conceptually:

```text id="a4v3hc"
DNS Lookup ✅
     ↓
ARP (if needed) 🔎
     ↓
TCP Handshake 🤝
     ↓
TLS Handshake 🔐
     ↓
HTTP Request 📤
```

Exactly the sequence shown in your course.

---

# 🧠 Best Mental Model

Think of DNS as a **global distributed phone directory** 📖.

You know:

```text
"example.com"
```

but the network needs:

```text
"93.184.216.34"
```

So:

```text id="7vav0g"
Domain Name
    ↓
DNS
    ↓
IP Address
```

And the lookup hierarchy is:

```text id="l3mjjk"
Root
 ↓
TLD
 ↓
Authoritative
 ↓
Actual DNS record
```

---

# 🔥 Must Remember

> **DNS lookup converts a domain name into the IP information needed to communicate with the destination, usually through a recursive resolver and DNS caching.**

### Full flow:

```text id="e4s7xk"
Browser/OS Cache
      ↓
Recursive Resolver
      ↓
Resolver Cache
      ↓
Root
      ↓
TLD
      ↓
Authoritative Server
      ↓
A / AAAA record
      ↓
IP address
```

### Most important distinction:

```text id="6kcyhg"
Recursive Resolver
→ performs lookup on behalf of client

Authoritative Server
→ provides authoritative DNS records
```

---

## 🎯 Module 14 Progress

✅ Browser Cache  
✅ **DNS Lookup 🔎**  
⬜ ARP  
⬜ DHCP (if needed)  
⬜ TCP Handshake  
⬜ TLS Handshake  
⬜ HTTP Request  
⬜ Router Forwarding  
⬜ NAT  
⬜ Load Balancer  
⬜ Web Server

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *In the complete DNS resolution chain for `www.example.com`, which server provides the final authoritative IP?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> The Authoritative Name Server for `example.com` (delegated by the `.com` TLD servers).
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🧭 Topic 1 — Browser Cache](./01_Browser_Cache.md) | [**Module 14: Complete Internet Journey + Revision**](./README.md) | [Next: 🔎 ARP — In the Complete Internet Journey ➡️](./03_ARP.md) |

<div align="center">
  <br/>
  <code>[█████████░] 90% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
