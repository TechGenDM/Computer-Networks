# 🔄 NAT — In the Complete Internet Journey

We already learned NAT and PAT separately. Now let's place **NAT inside the actual journey of your HTTPS request**.

At this point:

```text
Browser Cache
   ↓
DNS Lookup
   ↓
ARP
   ↓
TCP Handshake
   ↓
TLS Handshake
   ↓
HTTP Request 📤
   ↓
Router Forwarding 🚦
   ↓
NAT 🔄  ← HERE
   ↓
Load Balancer
   ↓
Web Server
```

The key idea:

> **NAT changes addressing as traffic crosses a network boundary, commonly translating a private IPv4 source address into an address used on the upstream/public side.**

In a typical home network, this is commonly done together with **PAT**.

---

# 1. Before NAT

Suppose your MacBook has:

```text
Private IP:
192.168.1.25

Source port:
53142
```

and the web server is:

```text
93.184.216.34:443
```

Your outgoing connection looks conceptually like:

```text
192.168.1.25:53142
        ↓
93.184.216.34:443
```

But `192.168.1.25` is a **private IPv4 address**.

It isn't globally routable across the public Internet.

So your home router may translate it.

---

# 2. NAT/PAT Translation

Suppose your router's external address is:

```text
203.0.113.50
```

The router might create a mapping such as:

```text
192.168.1.25:53142
        ↓
203.0.113.50:62001
```

So the outgoing Internet-facing traffic becomes conceptually:

```text
203.0.113.50:62001
        ↓
93.184.216.34:443
```

The server therefore sees the connection arriving from the router's external address/port rather than the laptop's private address.

---

# 3. Why the Router Needs a Translation Table

The router needs to remember:

```text
Private                    Public
────────────────────────────────────────
192.168.1.25:53142   →   203.0.113.50:62001
```

This lets it understand the response.

Suppose the web server replies:

```text
93.184.216.34:443
        ↓
203.0.113.50:62001
```

The router looks at its NAT/PAT state:

```text
203.0.113.50:62001
        ↓
192.168.1.25:53142
```

Then forwards the response to your MacBook.

---

# 4. Multiple Devices

This is where PAT becomes important.

Suppose your home has:

```text
MacBook → 192.168.1.25:53142
Phone   → 192.168.1.26:53142
TV      → 192.168.1.27:53142
```

The router can translate them to:

```text
192.168.1.25:53142 → 203.0.113.50:62001
192.168.1.26:53142 → 203.0.113.50:62002
192.168.1.27:53142 → 203.0.113.50:62003
```

So:

```text
                 🏠 Home Router
                      │
                    PAT
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
     :62001       :62002       :62003
          └───────────┴───────────┘
                      ↓
                 Internet
```

One external IPv4 address can therefore support many simultaneous connections, distinguished by translated ports.

---

# 5. What Happens to the HTTP Request?

This is a subtle but important point.

Your request is:

```http
GET /products HTTP/1.1
Host: example.com
```

For HTTPS, it's inside TLS:

```text
HTTP
 ↓
TLS encryption
 ↓
TCP
 ↓
IP
```

When NAT happens, the router generally changes **IP/transport addressing information**, not the encrypted HTTP contents.

Conceptually:

```text
Before NAT:

IP:
192.168.1.25:53142
        ↓
93.184.216.34:443

Encrypted application data
        ↓
TLS
```

After NAT:

```text
IP:
203.0.113.50:62001
        ↓
93.184.216.34:443

Same encrypted application data
        ↓
TLS
```

So:

> **NAT does not need to understand your HTTP request to perform ordinary translation.**

---

# 6. NAT Happens at the Network Boundary

Think:

```text
              PRIVATE NETWORK
                    │
             192.168.1.25
                    │
                    ↓
                🏠 Router
                 NAT/PAT
                    │
          203.0.113.50
                    ↓
                ISP Network
                    ↓
                Internet
```

This is the boundary between your private network and the upstream network.

---

# 7. NAT vs Routing

These are different operations.

### Routing

Answers:

> **"Where should this packet go next?"**

### NAT

Answers:

> **"Does the address/port need to be translated at this boundary?"**

A router can perform both:

```text
Packet arrives
    ↓
Routing decision
    ↓
NAT/PAT translation
    ↓
Forward
```

The exact implementation order can vary by platform and direction, so don't memorize a universal internal processing sequence. The conceptual distinction is what matters.

---

# 8. NAT vs PAT

Keep the terms straight:

```text
NAT
→ address translation

PAT
→ address + port translation
```

In most home IPv4 networks, what people casually call **"NAT"** is often actually **PAT/NAT overload**.

That's why a single home public IPv4 address can support dozens or hundreds of simultaneous connections.

---

# 9. What Happens to the Return Traffic?

This is the full cycle:

```text
        OUTBOUND
MacBook
192.168.1.25:53142
       ↓
    NAT/PAT
       ↓
203.0.113.50:62001
       ↓
Web Server
93.184.216.34:443
```

Response:

```text
        INBOUND
Web Server
93.184.216.34:443
       ↓
203.0.113.50:62001
       ↓
    NAT/PAT
       ↓
192.168.1.25:53142
       ↓
MacBook
```

That's the complete translation loop.

---

# 10. Why This Is Called Stateful NAT/PAT

The router needs to remember active mappings.

For example:

```text
203.0.113.50:62001
        ↓
192.168.1.25:53142
```

This state usually exists only for some period and is associated with the connection/flow.

When the flow becomes inactive, the mapping can eventually expire.

---

# 11. Important: NAT Is Not Encryption 🔐

NAT does **not** encrypt your traffic.

For HTTPS:

```text
TLS → encryption
NAT → address/port translation
```

These are different jobs.

```text
TLS
→ protects application data

NAT/PAT
→ translates addressing
```

---

# 12. Important: NAT Is Not a Firewall 🛡️

Again:

```text
NAT/PAT → translation
Firewall → traffic filtering
```

A home router commonly implements both, but they're not the same function.

---

# 13. The Complete Journey So Far 🔥

Now our request's story is:

```text
You enter:
https://example.com

        ↓

Browser Cache
        ↓

DNS
        ↓
93.184.216.34
        ↓

ARP for gateway MAC
        ↓

TCP Handshake
        ↓

TLS Handshake
        ↓

HTTP Request
        ↓

Home Router
        ↓
Routing Decision
        ↓
NAT/PAT
        ↓

203.0.113.50:62001
        ↓

ISP
        ↓
Internet
        ↓
Destination Network
```

The next major component is the **load balancer**.

---

# 🧠 Best Mental Model

Imagine your house has an internal address system:

```text
Room A → 192.168.1.25
Room B → 192.168.1.26
Room C → 192.168.1.27
```

The outside world sees:

```text
House's external identity
203.0.113.50
```

The router keeps track of which internal conversation corresponds to which external port.

```text
Private connection
       ↓
PAT translation
       ↓
Public connection
```

---

# 🔥 Must Remember

> **NAT translates IP addressing at a network boundary; in typical home IPv4 networks, PAT additionally translates ports so many private devices can share one external IPv4 address.**

The key example:

```text
192.168.1.25:53142
        ↓
203.0.113.50:62001
```

And the three things to keep separate:

```text
Routing → where should the packet go?
NAT/PAT → how should its address/port be translated?
TLS → how is the application data protected?
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
✅ **NAT 🔄**  
⬜ Load Balancer  
⬜ Web Server  
⬜ HTTP Response  
⬜ TCP Close

**Next → Load Balancer ⚖️** — we'll see why a major website may have **hundreds or thousands of servers behind one public service address**, and how the incoming connection gets distributed among them.