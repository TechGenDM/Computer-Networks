# ⚡ Module 9 — Topic 6: DNS Caching

This is the piece that makes DNS **fast and scalable**.

We've learned the full lookup:

```text
Laptop
  ↓
Recursive Resolver
  ↓
Root
  ↓
TLD
  ↓
Authoritative Server
  ↓
IP address
```

But imagine doing all of that **every single time** you visit a website.

That would be wasteful.

So DNS uses **caching**.

---

# 1. What is DNS caching?

**DNS caching = temporarily storing DNS answers so they can be reused later.**

Suppose the resolver looks up:

```text
example.com
       ↓
93.184.216.34
```

It can store:

```text
example.com → 93.184.216.34
```

for some period of time.

Then another user asks:

```text
"What is the IP of example.com?"
```

The resolver can simply answer from its cache.

```text
User
 ↓
Resolver
 ↓
💾 Cache
 ↓
IP
```

No Root/TLD/Authoritative lookup required.

---

# 2. Why is caching necessary?

Without caching:

```text
User 1 → Root → TLD → Authoritative
User 2 → Root → TLD → Authoritative
User 3 → Root → TLD → Authoritative
User 4 → Root → TLD → Authoritative
...
```

Millions of repeated queries would create unnecessary traffic.

With caching:

```text
User 1 → Resolver → Root/TLD/Authoritative
                         ↓
                      Cache it
                         ↓
User 2 → Resolver → 💾 Cache
User 3 → Resolver → 💾 Cache
User 4 → Resolver → 💾 Cache
```

Much more efficient.

---

# 3. TTL — Time To Live ⏱️

Here's the most important term in DNS caching:

> **TTL = Time To Live**

A DNS record can specify how long a resolver may cache the answer.

For example:

```text
example.com
IP = 93.184.216.34
TTL = 3600 seconds
```

That means the record can generally be cached for:

```text
3600 seconds = 1 hour
```

During that period, the resolver can reuse the cached information.

---

# 4. What happens when TTL expires?

Suppose:

```text
example.com → 93.184.216.34
TTL = 60 seconds
```

After 60 seconds, the cached record becomes **expired/stale for normal use**.

The next query may require the resolver to obtain fresh information.

Conceptually:

```text
Cache
  ↓
TTL expired ❌
  ↓
Query DNS hierarchy
  ↓
Fresh answer
  ↓
Store in cache again
```

---

# 5. Worked example 🔥

Imagine you visit:

```text
www.example.com
```

### First request

Your resolver doesn't have the answer.

```text
Laptop
 ↓
Resolver
 ↓
Root
 ↓
.com TLD
 ↓
Authoritative Server
 ↓
93.184.216.34
```

The resolver receives:

```text
93.184.216.34
TTL = 300 seconds
```

It stores:

```text
💾 Cache

www.example.com
→ 93.184.216.34
→ 300 sec
```

---

### Second request

Another request arrives 20 seconds later:

```text
Laptop
 ↓
Resolver
 ↓
💾 Cache HIT
 ↓
93.184.216.34
```

⚡ Much faster.

---

### After 300 seconds

The TTL expires:

```text
💾 Cache
   ↓
Expired
```

The resolver needs to obtain fresh information.

---

# 6. Cache HIT vs Cache MISS

You'll hear these terms frequently.

### Cache HIT

Answer exists in cache and is still usable.

```text
Query
 ↓
Cache
 ↓
✅ HIT
 ↓
Answer
```

### Cache MISS

Answer isn't available in cache.

```text
Query
 ↓
Cache
 ↓
❌ MISS
 ↓
DNS lookup
 ↓
Answer
```

Simple:

> **HIT = found it.**

> **MISS = have to look it up.**

---

# 7. Where can DNS caching happen?

This is an important detail.

Caching isn't necessarily happening in only one place.

### ① Browser

Your browser may maintain DNS-related cached information.

```text
Browser
   ↓
Cache
```

### ② Operating System

Your OS can also cache DNS results.

```text
OS
 ↓
DNS cache
```

### ③ Local network/router

A home router or local DNS service may cache results.

### ④ Recursive resolver

This is a **major caching layer**.

```text
Your device
    ↓
Recursive Resolver
    ↓
💾 DNS Cache
```

So there can be multiple caching layers.

---

# 8. DNS caching doesn't mean the IP is permanent

This is VERY important.

Suppose:

```text
example.com
→ 1.2.3.4
```

You might think:

> "DNS says example.com is 1.2.3.4 forever."

❌ No.

DNS records can change.

For example:

```text
Old:
example.com → 1.2.3.4

Later:
example.com → 5.6.7.8
```

Caching means resolvers may temporarily continue using the old answer **until its TTL expires**, subject to DNS caching behavior.

---

# 9. Why would a website use a low TTL?

Suppose a service expects to change servers frequently.

It might use:

```text
TTL = 60 seconds
```

Then cached information doesn't remain around as long.

For a relatively stable record, it might use a longer TTL:

```text
TTL = 3600 seconds
```

or longer.

So:

> **Short TTL → fresher information, potentially more DNS queries**

> **Long TTL → fewer DNS queries, potentially longer persistence of cached data**

There's a trade-off.

---

# 10. DNS caching vs IP packet TTL

This is a **very common confusion**.

We've already learned about IPv4 TTL:

```text
IP Packet
 ↓
TTL decreases at each router
```

DNS also has something called **TTL**:

```text
DNS Record
 ↓
Cache lifetime
```

They are **completely different concepts**.

### IP TTL

Protects packets from looping forever.

```text
Router → Router → Router
TTL decreases
```

### DNS TTL

Controls how long a DNS record can be cached.

```text
DNS answer
 ↓
Cache
 ↓
TTL expires
```

🔥 Same name, completely different purpose.

---

# 11. Negative caching

DNS can also cache certain **negative responses**.

For example:

```text
Does:
doesnotexist.example.com
exist?
```

Authoritative DNS might indicate that the name doesn't exist.

A resolver can cache that negative result for an appropriate period.

Then:

```text
User 1 → "doesnotexist..."
         ↓
       DNS lookup
         ↓
       doesn't exist

User 2 → "doesnotexist..."
         ↓
       cached negative answer
```

This avoids repeatedly asking the DNS hierarchy about something that doesn't exist.

You don't need the detailed record mechanics yet—just understand the concept.

---

# 12. Full DNS process with caching 🧠

Now you understand the whole thing:

```text
                 User
                   │
                   ↓
            Local/OS Cache
                   │
              Cache miss?
                   ↓
          Recursive Resolver
                   │
              Cache hit?
              ↙         ↘
            YES          NO
             ↓            ↓
            IP          Root
                          ↓
                         TLD
                          ↓
                   Authoritative
                          ↓
                       DNS answer
                          ↓
                    Store in cache
                          ↓
                         User
```

This is the real power of DNS caching.

---

# 🔥 The most important things to remember

### DNS caching

> **Stores DNS answers temporarily to avoid repeatedly performing full DNS resolution.**

### TTL

> **Controls how long a DNS record can normally remain cached.**

### Cache Hit

> Answer found in cache.

### Cache Miss

> Answer not available → resolver needs to look it up.

### Multiple caching layers

```text
Browser
   ↓
OS
   ↓
Router/local DNS
   ↓
Recursive Resolver
```

### And don't confuse:

```text
DNS TTL ≠ IP TTL
```

---

## Module 9 progress

✅ Why DNS?  
✅ DNS Hierarchy  
✅ Recursive Resolver  
✅ Root Server  
✅ TLD Server  
✅ **Caching** ⚡  
⬜ URL Anatomy  
⬜ HTTP Basics  
⬜ HTTP Request  
⬜ HTTP Response  
⬜ Status Codes  
⬜ Cookies  
⬜ Sessions

**Next → URL Anatomy 🔗**

We'll break down something you use every day:

```text
https://www.example.com:443/products?id=42#reviews
```

and understand exactly what **`https`**, **`www`**, **`example.com`**, **`:443`**, **`/products`**, **`?id=42`**, and **`#reviews`** each mean.