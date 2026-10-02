# 🧪 `dig` — DNS Investigation Tool

Now we go one step deeper than `nslookup`.

You can think of it like this:

```text
nslookup → simple DNS lookup
dig      → detailed DNS investigation
```

`dig` stands for **Domain Information Groper**.

> **`dig` is a command-line tool for querying DNS servers and inspecting detailed DNS responses.**

---

# 1. Basic command

On macOS/Linux:

```bash
dig google.com
```

You may see something like:

```text
; <<>> DiG 9.x <<>> google.com
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12345
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; QUESTION SECTION:
;google.com.          IN      A

;; ANSWER SECTION:
google.com.   300     IN      A       142.250.x.x

;; Query time: 15 msec
;; SERVER: 192.168.1.1#53
;; WHEN: ...
;; MSG SIZE  rcvd: ...
```

It looks intimidating, but the structure is actually logical.

---

# 2. The Header

This line is important:

```text
;; ->>HEADER<<- opcode: QUERY, status: NOERROR
```

### `opcode: QUERY`

This is a normal DNS query.

### `status: NOERROR`

The DNS server successfully processed the query.

Some other status codes you may encounter:

```text
NOERROR   → successful
NXDOMAIN  → domain name does not exist
SERVFAIL  → server failed to complete the query
REFUSED   → server refused the query
```

---

# 3. DNS Flags

Example:

```text
flags: qr rd ra
```

The important ones here:

```text
qr → this is a response
rd → recursion desired
ra → recursion available
```

For your level, remember:

> `rd` means the client asked for recursive resolution, and `ra` means the server supports recursion.

---

# 4. QUESTION SECTION

Example:

```text
;; QUESTION SECTION:
;google.com.    IN    A
```

This means:

```text
Domain → google.com
Class  → IN
Type   → A
```

`IN` means **Internet** DNS class.

`A` means:

> **IPv4 address record**

---

# 5. ANSWER SECTION 🎯

Example:

```text
;; ANSWER SECTION:
google.com.   300   IN   A   142.250.x.x
```

This is the actual DNS answer.

Break it apart:

```text
google.com.
    ↓
Domain

300
    ↓
TTL

IN
    ↓
Internet class

A
    ↓
IPv4 record

142.250.x.x
    ↓
IP address
```

So:

```text
google.com → 142.250.x.x
```

---

# 6. TTL — Again!

Remember DNS caching?

Here you can see the TTL directly.

Example:

```text
google.com.   300   IN   A   142.250.x.x
              ↑
             TTL
```

`300` means the record can generally be cached for:

```text
300 seconds = 5 minutes
```

This connects directly to what you learned earlier:

```text
DNS Record
    ↓
TTL
    ↓
Resolver caches it
    ↓
TTL expires
    ↓
Fresh lookup may happen
```

🔥 `dig` makes DNS caching much easier to visualize.

---

# 7. AUTHORITY SECTION

Suppose you're querying a domain and the response includes:

```text
;; AUTHORITY SECTION:
example.com.    86400   IN   NS   ns1.example.com.
```

This section provides **authoritative information**, such as the name servers responsible for the zone.

This connects directly to your DNS hierarchy:

```text
Root
  ↓
TLD
  ↓
Authoritative Name Server
  ↓
DNS Records
```

---

# 8. ADDITIONAL SECTION

The additional section may contain extra useful records related to the response.

For example:

```text
;; ADDITIONAL SECTION:
ns1.example.com.   300   IN   A   203.0.113.10
```

This can provide information that helps the resolver use the answer efficiently.

You don't need to memorize all possible cases yet.

---

# 9. Query Different Record Types

This is where `dig` becomes very useful.

### A record

```bash
dig example.com A
```

Find IPv4 address.

### AAAA

```bash
dig example.com AAAA
```

Find IPv6 address.

### MX

```bash
dig example.com MX
```

Find mail servers.

### NS

```bash
dig example.com NS
```

Find name servers.

### CNAME

```bash
dig www.example.com CNAME
```

Check whether the name is an alias.

---

# 10. `+short`

Normal `dig` output contains lots of information.

For just the answer:

```bash
dig example.com +short
```

You might get:

```text
93.184.216.34
```

This is extremely convenient in scripts and quick troubleshooting.

Think:

```text
dig           → detailed investigation
dig +short    → just give me the answer
```

---

# 11. Query a Specific DNS Server

Just like with `nslookup`, you can choose the resolver:

```bash
dig @8.8.8.8 example.com
```

The format is:

```text
dig @DNS_SERVER DOMAIN
```

So you're saying:

> "Ask this DNS server."

For example:

```bash
dig @1.1.1.1 example.com
```

This is great for comparing DNS resolvers.

---

# 12. `dig` + DNS Hierarchy

This is one of the coolest parts.

You learned earlier:

```text
Root
 ↓
TLD
 ↓
Authoritative
```

`dig` can help you investigate each level.

For example:

```bash
dig example.com NS
```

asks for the domain's name servers.

You can also inspect TLD information:

```bash
dig com NS
```

And root name servers:

```bash
dig . NS
```

This lets you see the hierarchy rather than treating DNS as a black box.

---

# 13. `dig` and Troubleshooting

Suppose:

```bash
ping example.com
```

is failing.

Run:

```bash
dig example.com
```

### Case 1 — `NOERROR`

You get an answer:

```text
status: NOERROR
```

DNS resolution is functioning for that query.

The problem may be elsewhere.

### Case 2 — `NXDOMAIN`

```text
status: NXDOMAIN
```

The DNS system says the queried domain name does not exist.

### Case 3 — `SERVFAIL`

```text
status: SERVFAIL
```

The resolver couldn't successfully complete the query.

This can point toward a DNS resolution/problem path rather than simply "the domain doesn't exist."

---

# 14. `dig` vs `nslookup`

| | `nslookup` | `dig` |
|---|---|---|
| DNS lookup | ✅ | ✅ |
| Simple output | ✅ | |
| Detailed response | Limited | ✅ |
| DNS sections | Limited | ✅ |
| TTL visibility | Less convenient | ✅ |
| DNS troubleshooting | ✅ | ✅✅ |
| Scripting | Possible | Very useful |

Mental model:

```text
nslookup
   ↓
"What's the IP?"

dig
   ↓
"Show me exactly what the DNS server
returned and how."
```

---

# 15. A Very Useful Command Set

These are worth keeping:

```bash
dig example.com
```

Basic detailed lookup.

```bash
dig example.com +short
```

Just the answer.

```bash
dig example.com A
```

IPv4.

```bash
dig example.com AAAA
```

IPv6.

```bash
dig example.com MX
```

Mail servers.

```bash
dig example.com NS
```

Name servers.

```bash
dig @8.8.8.8 example.com
```

Use Google's public resolver.

---

# 🧠 Best Mental Model

Think of `dig` as a **DNS laboratory tool** 🔬.

Instead of simply asking:

> "What's the IP of this website?"

you can inspect:

```text
Who answered?
What was the status?
What record was requested?
What was the answer?
What's the TTL?
Which name servers are involved?
How long did the query take?
```

---

# 🔥 Must Remember

> **`dig` is a detailed command-line DNS query and troubleshooting tool.**

The most important parts of its output:

```text
HEADER
   ↓
Status + flags

QUESTION
   ↓
What was asked?

ANSWER
   ↓
What was returned?

AUTHORITY
   ↓
Authoritative information

ADDITIONAL
   ↓
Extra useful records
```

And these commands:

```bash
dig example.com
dig example.com +short
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com NS
dig @8.8.8.8 example.com
```

### One-line memory:

> **`nslookup` helps you query DNS; `dig` helps you inspect DNS.**

---

## Module 13 Progress

✅ `ping` 🏓  
✅ `traceroute` 🗺️  
✅ `nslookup` 🔎  
✅ **`dig` 🧪**  
⬜ `netstat`  
⬜ `ss`  
⬜ `tcpdump`  
⬜ Wireshark  
⬜ Reading Packet Captures  
⬜ Common Networking Problems

**Next → `netstat` 📊** — we'll see how to inspect **active connections, listening ports, and network statistics** directly from your machine.