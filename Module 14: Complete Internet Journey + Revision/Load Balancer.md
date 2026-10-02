# ⚖️ Load Balancer

Now our request has passed through your **home router, routing, and NAT/PAT** and reached the service's infrastructure.

But there's a new problem:

> **A large website usually isn't running on just one server.**

It may have many servers handling the same application.

That's where a **load balancer** comes in.

---

# 1. What is a Load Balancer?

A **load balancer** distributes incoming traffic across multiple backend servers.

Instead of:

```text
Internet
   ↓
One Server
```

we can have:

```text
                 Internet
                    ↓
              Load Balancer ⚖️
              /      |      \
             ↓       ↓       ↓
          Server 1 Server 2 Server 3
```

The load balancer decides **which backend should handle the traffic**.

---

# 2. Why Do We Need One?

Imagine your website suddenly gets:

```text
10 users
```

One server may be enough.

But suppose it gets:

```text
1,000,000 users
```

A single server could become overloaded.

So instead:

```text
                 Traffic
                    ↓
            Load Balancer
              /    |    \
             ↓     ↓     ↓
           333k  333k  334k
          Server Server Server
```

The workload can be distributed.

---

# 3. What Happens in Our Internet Journey?

So far:

```text
Browser
   ↓
DNS
   ↓
ARP
   ↓
TCP Handshake
   ↓
TLS Handshake
   ↓
HTTP Request
   ↓
Router Forwarding
   ↓
NAT/PAT
   ↓
Internet
   ↓
🌐 Load Balancer ← HERE
```

The request reaches the service's frontend/edge infrastructure.

The load balancer then determines where to send it.

---

# 4. Example

Suppose the request is:

```http
GET /products
Host: example.com
```

The load balancer might have:

```text
Backend Pool

Server A ✅
Server B ✅
Server C ✅
```

It could choose:

```text
Request #1 → Server A
Request #2 → Server B
Request #3 → Server C
```

So:

```text
                    Load Balancer
                    ⚖️
                 /    |    \
                ↓     ↓     ↓
              A       B      C
```

---

# 5. How Does It Decide?

There are different load-balancing strategies.

### Round Robin

```text
Request 1 → A
Request 2 → B
Request 3 → C
Request 4 → A
```

Simple rotation.

---

### Least Connections

Send the new connection to the backend with fewer active connections.

```text
A → 100 connections
B → 40 connections
C → 70 connections

New request → B
```

---

### Weighted

Some servers can receive more traffic than others.

```text
A → weight 5
B → weight 3
C → weight 2
```

So A receives a larger share.

---

# 6. Health Checks ❤️‍🩹

A load balancer shouldn't keep sending requests to a broken server.

It can periodically perform **health checks**:

```text
Load Balancer
   │
   ├── Server A → ✅ Healthy
   ├── Server B → ✅ Healthy
   └── Server C → ❌ Unhealthy
```

Then:

```text
New traffic
   ↓
A / B
```

rather than:

```text
A / B / C
```

until C becomes healthy again.

This improves availability.

---

# 7. Layer 4 vs Layer 7 Load Balancing

This is an important networking distinction.

## Layer 4

Operates using transport-level information such as:

```text
IP
TCP/UDP
Ports
```

It can make decisions without understanding the HTTP request itself.

Example:

```text
TCP :443
      ↓
Load Balancer
      ↓
Backend
```

---

## Layer 7

Operates at the application layer.

For HTTP, it can inspect things like:

```text
Host
Path
Headers
HTTP method
```

For example:

```text
/api/*       → API servers
/images/*    → Image servers
/admin/*     → Admin service
```

So:

```text
L4 → TCP/UDP-level information
L7 → HTTP/application-level information
```

---

# 8. Where Does TLS Terminate?

This is a useful real-world detail.

A load balancer can sometimes terminate TLS:

```text
Client
   ↓
HTTPS
   ↓
Load Balancer
   ↓
TLS decrypted here
   ↓
HTTP or HTTPS
   ↓
Backend
```

Alternatively, TLS can remain encrypted through the load balancer and terminate further downstream.

So don't assume:

> "The load balancer always decrypts HTTPS."

It depends on the architecture.

---

# 9. Does the Load Balancer Replace the Server?

No.

Think:

```text
Load Balancer
     ↓
Traffic distributor
```

while:

```text
Backend Server
     ↓
Actually processes the application request
```

For example:

```text
Browser
   ↓
Load Balancer
   ↓
Web Server
   ↓
Application
   ↓
Database
```

---

# 10. Load Balancer vs Router

Don't confuse these.

### Router 🚦

Makes network-layer forwarding decisions:

```text
Destination IP
    ↓
Next hop
```

### Load Balancer ⚖️

Chooses among backend servers:

```text
Incoming service traffic
        ↓
Which backend?
```

So:

```text
Router
→ "Which network/next hop?"

Load Balancer
→ "Which backend server?"
```

They solve different problems.

---

# 11. Load Balancer vs DNS

Also different.

### DNS

```text
example.com
     ↓
IP address / DNS response
```

### Load Balancer

```text
Incoming connection/request
        ↓
Choose backend
```

DNS can sometimes be used for traffic distribution as well, but it isn't a replacement for a load balancer's per-connection/request decision-making.

---

# 12. What Happens After the Load Balancer?

Suppose it selects Server B:

```text
Browser
   ↓
Internet
   ↓
Load Balancer
   ↓
Server B
```

Server B now processes:

```http
GET /products
```

It may then:

```text
Authenticate
     ↓
Run application logic
     ↓
Query database/cache
     ↓
Generate response
```

This brings us to our next step:

> **Web Server 🖥️**

---

# 🧠 Best Mental Model

Imagine a restaurant with one reception desk and many chefs.

```text
Customers
   ↓
Reception ⚖️
   ↓
Chef A
Chef B
Chef C
```

The receptionist decides:

> "Which available chef should handle this order?"

That's roughly what a load balancer does.

---

# 🔥 Must Remember

> **A load balancer distributes incoming connections or requests across a pool of backend servers.**

Key concepts:

```text
Load Balancer
├── Distributes traffic
├── Health checks backends
├── Can use different balancing algorithms
├── L4 → TCP/UDP level
└── L7 → HTTP/application level
```

And:

```text
Router
→ chooses next network hop

Load Balancer
→ chooses backend server
```

---

## 🎯 Module 14 Progress

✅ Browser Cache  
✅ DNS Lookup  
✅ ARP  
✅ DHCP (if needed)  
✅ TCP Handshake 🤝  
✅ TLS Handshake 🔐  
✅ HTTP Request 📤  
✅ Router Forwarding 🚦  
✅ NAT 🔄  
✅ **Load Balancer ⚖️**  
⬜ Web Server  
⬜ HTTP Response  
⬜ TCP Close

**Next → Web Server 🖥️** — we'll follow the request from the load balancer into the actual server/application, including **web server → backend application → database → response generation**.