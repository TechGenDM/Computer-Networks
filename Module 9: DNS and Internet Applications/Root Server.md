# 🌳 Module 9 — Topic 4: Root Server

Now we're going to zoom into the **very top of the DNS hierarchy**.

Remember our flow:

```text
Your Laptop
    ↓
Recursive Resolver
    ↓
Root Server
    ↓
TLD Server
    ↓
Authoritative DNS Server
    ↓
IP Address
```

The **Root Server is the starting point of DNS delegation** when the resolver doesn't already have the information cached.

---

# 1. What is a Root Server?

A DNS **root server** is a server in the DNS root-server system that helps resolvers find the appropriate **Top-Level Domain (TLD) servers**.

For example, suppose you're looking for:

```text
www.example.com
```

The resolver asks the root:

> "Where can I find information about `.com`?"

The root responds with information about the **`.com` TLD name servers**.

It does **not normally answer**:

> "`www.example.com` = 93.184.216.34"

That's the authoritative server's job.

---

# 2. Think of the Root as the top directory 📁

Imagine your computer has:

```text
/
├── Documents
├── Photos
├── Videos
└── Downloads
```

The root directory doesn't necessarily contain the file you're looking for.

It tells you where to look next.

DNS works similarly:

```text
                    Root
                      .
          ┌───────────┼───────────┐
          ↓           ↓           ↓
        .com         .org        .in
          ↓
       example
          ↓
         www
```

The root is essentially saying:

> **"For `.com`, go to these TLD servers."**

---

# 3. What does the root actually know?

The root zone contains information about **TLDs**.

For example:

```text
.com
.org
.net
.in
.edu
...
```

It contains delegation information that allows resolvers to find the servers responsible for those TLDs.

So conceptually:

```text id="4px9c4"
Root
 │
 ├── .com → TLD servers
 ├── .org → TLD servers
 ├── .net → TLD servers
 └── .in  → TLD servers
```

But:

```text
Root
  ✗
  └── doesn't store every website's final IP address
```

---

# 4. Complete lookup example

Let's say you enter:

```text
www.example.com
```

Your recursive resolver doesn't have the answer cached.

### Step 1

Resolver → Root:

> "I need `www.example.com`."

### Step 2

Root looks at:

```text
.com
```

and says:

> "I don't have the final answer. Here are the name servers responsible for `.com`."

```text
Root
 ↓
.com TLD servers
```

### Step 3

Resolver asks a `.com` TLD server:

> "Who is responsible for `example.com`?"

TLD responds with the authoritative name-server information.

```text
.com
 ↓
Authoritative servers for example.com
```

### Step 4

Resolver asks the authoritative server:

> "What's `www.example.com`?"

Then it gets the actual DNS record.

```text
www.example.com
       ↓
    IP address
```

---

# 5. There isn't just ONE root server

This is an important real-world detail.

You might hear:

> "There are 13 DNS root servers."

That statement is a little misleading.

There are **13 root server identities**, traditionally named:

```text
A-root
B-root
C-root
...
M-root
```

But these identities are operated using **many globally distributed server instances**, largely through **anycast**.

So the Internet doesn't depend on 13 individual physical machines.

Conceptually:

```text
              Root system
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
    Instance     Instance    Instance
    India        Europe      USA
       ↓           ↓           ↓
     users       users       users
```

This provides geographic distribution and resilience.

---

# 6. Why are root servers distributed?

Imagine there were only one physical root server:

```text
Everyone
   ↓
ONE SERVER
   ↓
💥
```

That would create an enormous:

- availability problem
- latency problem
- traffic problem
- failure risk

Instead, root-server operators deploy many instances around the world.

So when your resolver in India needs root information, DNS infrastructure can direct the request to an appropriate nearby/available instance.

---

# 7. What is Anycast?

You don't need to go extremely deep into Anycast yet, but understand the basic idea.

Multiple physical servers can advertise the **same IP address** from different locations.

```text
             Same IP
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
     India    Europe    USA
    Server    Server    Server
```

Routing infrastructure determines which instance is reached based on network routing.

So:

> **One logical service → many physical locations.**

This is one reason root DNS infrastructure can serve enormous amounts of global traffic.

---

# 8. Root server doesn't perform the whole lookup

This is another common misconception.

Don't imagine:

```text
Laptop
  ↓
Root
  ↓
Root searches everything
  ↓
IP
```

Instead:

```text
Laptop
  ↓
Recursive Resolver
  ↓
Root
  ↓
TLD
  ↓
Authoritative
  ↓
IP
```

The **recursive resolver is the one coordinating the lookup**.

The root simply provides the next delegation information.

---

# 9. Root Server vs Recursive Resolver

Very important distinction:

| Root Server | Recursive Resolver |
|---|---|
| Top of DNS hierarchy | Acts on behalf of clients |
| Knows about TLD delegations | Finds answers |
| Doesn't normally provide final website IP | Can return final answer |
| Part of authoritative DNS hierarchy | Performs recursive resolution |
| Receives queries from resolvers | Receives queries from clients |

Think:

```text
Recursive Resolver = 🔎 Detective
Root Server        = 🗂️ Directory
TLD Server         = 🗂️ More specific directory
Authoritative      = 📖 Source of domain records
```

---

# 🧠 The complete mental model

For:

```text
www.example.com
```

think:

```text
                 "www.example.com"
                         ↓
                Recursive Resolver
                         ↓
                      Root "."
                         ↓
                    TLD ".com"
                         ↓
               example.com servers
                         ↓
                  "www" record
                         ↓
                    IP address
```

The hierarchy is basically:

> **Root → TLD → Domain → Host**

---

# 🔥 Exam Cheat Sheet

### Root Server

> **The root DNS system provides delegation information for Top-Level Domains.**

### Remember:

```text
Root
 ↓
TLD
 ↓
Authoritative
 ↓
DNS record
```

### Important facts:

- Root is represented by **`.`**
- Root directs queries toward **TLD servers**
- It doesn't normally provide the final website IP
- There are **13 root-server identities**
- Those identities are served by **many globally distributed instances**
- **Anycast** helps distribute traffic to those instances

---

## Module 9 progress

✅ Why DNS?  
✅ DNS Hierarchy  
✅ Recursive Resolver  
✅ **Root Server**  
⬜ TLD Server  
⬜ Caching  
⬜ URL Anatomy  
⬜ HTTP Basics  
⬜ HTTP Request  
⬜ HTTP Response  
⬜ Status Codes  
⬜ Cookies  
⬜ Sessions

**Next → TLD Server** 🌐

We'll follow the same `www.example.com` request one level deeper and see exactly how the **`.com` server hands the resolver off to the authoritative DNS server for `example.com`.**