# 🌐 Module 9 — Topic 5: TLD Server

Great. We are now one level deeper in the DNS hierarchy.

Our lookup is:

```text
www.example.com
```

So far:

```text
Your Laptop
     ↓
Recursive Resolver
     ↓
Root Server
     ↓
      ??? 
```

That `???` is the **TLD Server**.

---

# 1. What is a TLD?

**TLD = Top-Level Domain**

It's the part at the **right side of a domain name**, just before the optional final `.`.

Examples:

```text
google.com
       ↑
      TLD

example.org
       ↑
      TLD

example.in
       ↑
      TLD
```

Common TLDs include:

```text
.com
.org
.net
.in
.edu
.gov
```

There are also many newer TLDs such as:

```text
.dev
.app
.ai
.shop
```

---

# 2. What does a TLD server do?

The TLD server **doesn't normally know the final IP address** of every website.

Instead, it knows:

> **"Which authoritative DNS servers are responsible for this particular domain?"**

For example:

```text
example.com
```

The `.com` TLD infrastructure knows which authoritative name servers are responsible for:

```text
example.com
```

So the process is:

```text
Root
 ↓
".com is handled here"
 ↓
.com TLD
 ↓
"example.com is handled by these authoritative servers"
```

---

# 3. Complete example

Let's resolve:

```text
www.example.com
```

### Step 1 — Your laptop

```text
Laptop
   ↓
"What's the IP of www.example.com?"
```

It asks the **recursive resolver**.

---

### Step 2 — Resolver → Root

The resolver asks:

> "Who handles `.com`?"

Root responds with information about the `.com` TLD servers.

```text
Resolver
   ↓
Root
   ↓
.com TLD servers
```

---

### Step 3 — Resolver → TLD

Now the resolver asks a `.com` TLD server:

> **"Who is authoritative for `example.com`?"**

The TLD server responds with the relevant authoritative name-server information.

Conceptually:

```text
.com TLD
    ↓
"Ask ns.example-authority..."
```

The exact server names vary by domain.

---

### Step 4 — Resolver → Authoritative DNS server

Now the resolver knows where to ask.

It asks:

> "What is `www.example.com`?"

The authoritative server returns the relevant DNS record.

For example, conceptually:

```text
www.example.com
       ↓
93.184.216.34
```

Then:

```text
Authoritative
      ↓
Recursive Resolver
      ↓
Your Laptop
```

---

# 4. The hierarchy now makes sense

We can visualize the entire process:

```text
                    DNS ROOT
                       .
                       ↓
                    .com
                 TLD Server
                       ↓
                  example.com
             Authoritative Server
                       ↓
                www.example.com
                       ↓
                   IP address
```

And remember:

**The recursive resolver is the one doing this journey on your behalf.**

```text
                 Recursive Resolver
                  /       |       \
                 ↓        ↓        ↓
              Root       TLD   Authoritative
```

---

# 5. TLD Server vs Root Server

This distinction is important.

### Root Server

Answers essentially:

> **"Who handles `.com`?"**

### TLD Server

Answers essentially:

> **"Who handles `example.com`?"**

### Authoritative Server

Answers:

> **"What is the DNS record for `www.example.com`?"**

So:

```text
Root
 ↓
TLD
 ↓
Authoritative
 ↓
Actual DNS record
```

---

# 6. Think of it like a company directory 🏢

Suppose you're looking for:

```text
Employee: Rahul
Company: Google
Department: Engineering
```

You could imagine:

```text
Reception
   ↓
"Which company?"
   ↓
Google
   ↓
"Which department?"
   ↓
Engineering
   ↓
"Where is Rahul?"
   ↓
Rahul's location
```

DNS is similar:

```text
Root
 ↓
.com
 ↓
example.com
 ↓
www.example.com
 ↓
IP address
```

Each level gets **more specific**.

---

# 7. Important: TLD ≠ authoritative server

This is a common confusion.

For:

```text
www.example.com
```

The `.com` TLD server is **not necessarily authoritative for `example.com`**.

Instead, it knows the delegation:

```text
.com
  ↓
"These name servers are authoritative for example.com."
```

Then those authoritative servers contain the actual records.

---

# 8. Another example: `.in`

Suppose we have:

```text
www.example.in
```

The process becomes:

```text
Resolver
   ↓
Root
   ↓
.in TLD
   ↓
Authoritative servers for example.in
   ↓
www.example.in record
   ↓
IP
```

Notice how only the TLD changes:

```text
.com → .com TLD
.in  → .in TLD
.org → .org TLD
```

---

# 🧠 The golden mental model

Think of DNS lookup as asking increasingly specific questions:

```text
ROOT:
"Who manages .com?"

       ↓

TLD:
"Who manages example.com?"

       ↓

AUTHORITATIVE:
"What is www.example.com's record?"

       ↓

ANSWER:
"Here is the IP."
```

That's the DNS hierarchy in action.

---

# 🔥 Exam Cheat Sheet

### TLD

**Top-Level Domain**

Examples:

```text
.com
.org
.net
.in
```

### TLD Server

> **Provides delegation information for domains within that TLD, directing resolvers to the appropriate authoritative name servers.**

### Remember:

```text
Root
 ↓
TLD
 ↓
Authoritative
 ↓
DNS Record
```

And:

> **Root tells you where the TLD is.**  
> **TLD tells you where the domain's authoritative servers are.**  
> **Authoritative server gives you the domain's actual DNS record.**

---

## Module 9 progress

✅ Why DNS?  
✅ DNS Hierarchy  
✅ Recursive Resolver  
✅ Root Server  
✅ **TLD Server**  
⬜ Caching  
⬜ URL Anatomy  
⬜ HTTP Basics  
⬜ HTTP Request  
⬜ HTTP Response  
⬜ Status Codes  
⬜ Cookies  
⬜ Sessions

**Next → DNS Caching ⚡**

This is a really important one because you'll finally understand **why DNS doesn't have to perform Root → TLD → Authoritative lookup every single time you visit a website.**