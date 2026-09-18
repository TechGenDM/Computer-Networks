# 🌳 DNS Hierarchy

Now let's understand **how DNS is organized**.

The key idea:

> **DNS is not one giant server/database. It is a hierarchical, distributed system.**

Imagine the domain:

```text
www.google.com
```

DNS organizes this name from **right → left**.

---

## 1. The DNS hierarchy

```text
                    Root
                     .
                     │
                  ┌──┴──┐
                  ↓     ↓
                 .com  .org
                  │
                  ↓
                google
                  │
                  ↓
                 www
```

So:

```text
www.google.com
│    │       │
│    │       └── TLD
│    └────────── Domain
└─────────────── Subdomain/host
```

But there's an important detail:

### The full hierarchy is:

```text
.
└── com
    └── google
        └── www
```

The `.` at the end is the **root**.

Technically:

```text
www.google.com.
```

That final dot is normally hidden from you.

---

# 2. Root (`.`)

At the very top is:

```text
.
```

This is the **DNS root**.

The root doesn't normally know the IP address of `www.google.com`.

Instead, it knows:

> **"Who handles `.com`?"**

So the root points the resolver toward the appropriate **TLD servers**.

---

# 3. TLD

**TLD = Top-Level Domain**

Examples:

```text
.com
.org
.net
.in
.edu
.gov
```

For:

```text
www.google.com
```

the TLD is:

```text
.com
```

For:

```text
example.in
```

the TLD is:

```text
.in
```

The `.com` TLD infrastructure knows which **authoritative name servers** are responsible for domains such as `google.com`.

It doesn't necessarily contain Google's final IP address itself.

---

# 4. Domain

Now take:

```text
www.google.com
```

The domain here is:

```text
google.com
```

The `google` portion is the domain label immediately below `.com`.

The authoritative DNS servers for `google.com` contain DNS records for that domain.

For example, conceptually:

```text
google.com
   │
   ├── www
   ├── mail
   └── other hosts
```

---

# 5. Subdomain / Host

Now:

```text
www.google.com
```

`www` is a label below `google.com`.

It can be used as a **subdomain/hostname**, depending on how the DNS zone is configured.

Other examples:

```text
mail.google.com
api.example.com
blog.example.com
```

So you might have:

```text
example.com
├── www.example.com
├── api.example.com
├── blog.example.com
└── mail.example.com
```

Each can have different DNS records.

---

# 6. Why use a hierarchy?

Imagine if DNS were one giant database:

```text
EVERY DOMAIN ON EARTH
        ↓
ONE DATABASE
        ↓
ONE SERVER SYSTEM
```

That would be difficult to scale and manage.

Instead:

```text
                    Root
                      │
       ┌──────────────┼──────────────┐
      .com           .org            .in
       │              │               │
    google          wikipedia       example
       │
      www
```

Responsibility is **distributed**.

This makes DNS:

- scalable
- distributed
- manageable
- resilient

---

# 7. A real DNS lookup

Suppose you type:

```text
www.example.com
```

A resolver can conceptually work through:

```text
        www.example.com
               ↓
             Root
               ↓
          ".com server"
               ↓
       "Who handles example.com?"
               ↓
   Authoritative server for example.com
               ↓
       "What's www.example.com?"
               ↓
          IP address
```

For example:

```text
www.example.com
       ↓
93.184.216.34
```

The exact IP can change; the important thing is the **hierarchical lookup process**.

---

# 8. Root → TLD → Authoritative

This is the sequence you should remember:

```text
Root
  ↓
TLD
  ↓
Authoritative DNS server
  ↓
DNS record
  ↓
IP address
```

For:

```text
www.example.com
```

that means:

```text
. 
↓
.com
↓
example.com
↓
www.example.com
```

---

# 9. One important correction to a common misconception

Don't think:

> ❌ Root server gives you the IP address.

Instead:

> ✅ Root server directs you toward the appropriate **TLD servers**.

Then:

> ✅ TLD servers direct you toward the **authoritative DNS servers** for the domain.

Finally:

> ✅ The authoritative server provides the actual DNS record.

This distinction becomes very important in the next topics.

---

# 🧠 Think of DNS like an office directory

You ask:

> "Where is Google's office?"

The receptionist doesn't personally know every employee.

They say:

```text
Reception
   ↓
"Go to the Google department."
   ↓
Google department
   ↓
"Ask the Web team."
   ↓
Web team
   ↓
"www is here."
```

That's the basic idea behind DNS delegation and hierarchy.

---

# 🔥 Exam Cheat Sheet

```text
DNS Hierarchy

        Root (.)
           ↓
       TLD (.com)
           ↓
    Domain (google.com)
           ↓
   Host/subdomain (www)
```

### Remember:

**Root** → knows where TLD infrastructure is.

**TLD** → knows where authoritative servers for domains are.

**Authoritative DNS server** → has the actual DNS records.

---

## Module 9 progress

✅ Why DNS?  
✅ **DNS Hierarchy**  
⬜ Recursive Resolver  
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

**Next → Recursive Resolver** 🔍

This is the piece that actually **does the DNS lookup on your behalf**, and we'll trace a complete `google.com` lookup from your laptop all the way to the authoritative DNS server.