# 🔍 Module 9 — Topic 3: Recursive Resolver

This is one of the **most important pieces of DNS**.

We know:

```text
You type:
www.example.com
```

Your laptop needs an IP address.

But does your laptop itself contact the Root Server, then TLD Server, then Authoritative Server?

**Usually, no.**

Instead, it asks a **Recursive DNS Resolver**.

---

# 1. What is a Recursive Resolver?

A **recursive resolver** is a DNS server that performs the DNS lookup **on behalf of your device**.

Think of it as a middleman:

```text
Your Laptop
     │
     │ "What's the IP of example.com?"
     ↓
Recursive Resolver
     │
     ├── Root
     ↓
    TLD
     ↓
Authoritative Server
     ↓
   IP address
     │
     ↓
Your Laptop
```

So your laptop basically says:

> **"You figure it out and give me the answer."**

That's why it's called **recursive** resolution.

---

# 2. Who operates the resolver?

The resolver can be provided by:

- your ISP
- your organization
- a public DNS provider
- your router/local network
- another DNS service

Your device might be configured to use a particular DNS resolver.

For example, conceptually:

```text
Laptop
   ↓
DNS Resolver
   ↓
Internet DNS hierarchy
```

---

# 3. Complete example 🔥

Suppose you type:

```text
www.example.com
```

Your browser needs the IP.

### Step 1 — Laptop asks resolver

```text
Laptop
   │
   │ "IP for www.example.com?"
   ↓
Recursive Resolver
```

---

### Step 2 — Resolver checks its cache

The resolver first asks:

> "Do I already know this answer?"

If yes:

```text
Cache
 ↓
IP address
 ↓
Laptop
```

Done! ⚡

We'll study **DNS caching** separately.

If not, the resolver continues.

---

# 4. Resolver asks the Root

Suppose the answer isn't cached.

The resolver asks a Root DNS server:

> "Who handles `.com`?"

The Root responds approximately:

> "Here are the servers responsible for `.com`."

Notice:

**Root doesn't necessarily give the final IP.**

```text
Resolver
   ↓
Root
   ↓
".com TLD servers"
```

---

# 5. Resolver asks the TLD server

Now the resolver asks a `.com` TLD server:

> "Who is authoritative for `example.com`?"

The TLD server responds with information directing the resolver to the **authoritative name servers** for `example.com`.

```text
Resolver
   ↓
.com TLD
   ↓
"Ask these authoritative servers."
```

---

# 6. Resolver asks the authoritative server

Now the resolver asks the authoritative DNS server:

> "What's the DNS record for `www.example.com`?"

The authoritative server responds with the relevant DNS record.

Conceptually:

```text
www.example.com
       ↓
93.184.216.34
```

---

# 7. Resolver gives the answer to your laptop

Now:

```text
Authoritative Server
       ↓
Recursive Resolver
       ↓
Your Laptop
       ↓
Browser
```

Your browser now knows where to connect.

Then the next stage is:

```text
DNS
 ↓
IP address
 ↓
TCP/TLS connection
 ↓
HTTP/HTTPS
 ↓
Web server
```

---

# 8. Why is it called "recursive"?

Because the resolver is responsible for **following the chain of DNS queries until it can return an answer**.

Your laptop doesn't have to individually perform:

```text
Laptop → Root
Laptop → TLD
Laptop → Authoritative
```

Instead:

```text
Laptop → Resolver
             │
             ├→ Root
             ├→ TLD
             └→ Authoritative
                    ↓
             Final answer
             ↓
          Laptop
```

The resolver does the work.

---

# 9. Recursive vs Iterative — important distinction

This is a common interview/exam question.

### Recursive query

Client tells resolver:

> **"Give me the final answer."**

```text
Laptop → Resolver
         "Find www.example.com for me."
```

The resolver takes responsibility for resolving it.

### Iterative query

A DNS server essentially responds:

> **"I don't have the final answer, but here's who you should ask next."**

For example:

```text
Resolver → Root
Root → ".com servers"

Resolver → .com
.com → "Authoritative servers"

Resolver → Authoritative
Authoritative → "Here's the answer."
```

So the resolver is typically making **iterative queries** to DNS servers while providing a **recursive service** to the client.

That's an important distinction.

---

# 10. Full DNS resolution picture 🧠

Put everything together:

```text
                 YOU
                  │
                  │
           "www.example.com"
                  │
                  ↓
        ┌──────────────────┐
        │ Recursive Resolver│
        └────────┬─────────┘
                 │
                 ↓
              Root
                 │
                 ↓
              .com
             TLD Server
                 │
                 ↓
       Authoritative Server
                 │
                 ↓
            IP Address
                 │
                 ↓
        Recursive Resolver
                 │
                 ↓
               YOU
```

---

# 11. One subtle point: the resolver doesn't always contact all three

This is important.

If the resolver already has the answer cached:

```text
Laptop
  ↓
Resolver
  ↓
Cache HIT
  ↓
IP
```

It doesn't need to contact:

```text
Root
TLD
Authoritative
```

That's why DNS can often be extremely fast.

We'll dive into this with **DNS Caching** later.

---

# 12. Resolver vs Authoritative Server

Don't mix these up.

### Recursive Resolver

> **Finds answers on behalf of clients.**

```text
Laptop → Resolver
```

### Authoritative DNS Server

> **Provides authoritative DNS records for a domain/zone.**

```text
Resolver → Authoritative Server
```

Easy memory trick:

> **Resolver = researcher 🔎**

> **Authoritative server = source of truth 📚**

---

# 🔥 Exam Cheat Sheet

```text
Recursive Resolver
│
├── Receives DNS queries from clients
│
├── Checks cache
│
├── If needed, queries DNS hierarchy
│
├── Root → TLD → Authoritative
│
└── Returns result to client
```

### One-line definition:

> **A recursive DNS resolver is a DNS server that accepts a client's request and obtains the requested DNS information on the client's behalf, often using cached data or querying other DNS servers.**

---

## Module 9 progress

✅ Why DNS?  
✅ DNS Hierarchy  
✅ **Recursive Resolver**  
⬜ Root Server  
⬜ TLD Server  
⬜ Caching  
⬜ URL Anatomy  
⬜ HTTP Basics  
⬜ HTTP Request  
⬜ HTTP Response  
⬜ Status Codes  
⬜ Cookies  
⬜ Sessions

**Next → Root Server 🌳**

We'll zoom into the first level of the DNS hierarchy and understand **what Root Servers actually know, what they don't know, and why there are many root-server instances rather than one physical machine.**