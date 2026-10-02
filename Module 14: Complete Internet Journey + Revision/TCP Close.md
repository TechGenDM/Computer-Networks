# 🔚 TCP Close — Completing the Internet Journey

We’ve finally reached the **last step**. 🎯

After the server has sent the HTTP response and both sides are finished communicating, the TCP connection needs to be terminated gracefully.

The normal graceful TCP close is based on:

```text id="7r5d9q"
FIN
↓
ACK
↓
FIN
↓
ACK
```

---

# 1. Where TCP Close Fits

Our complete HTTPS journey now looks like:

```text id="8tmt1l"
Browser Cache
      ↓
DNS Lookup
      ↓
ARP (if needed)
      ↓
DHCP (if needed)
      ↓
TCP Handshake 🤝
      ↓
TLS Handshake 🔐
      ↓
HTTP Request 📤
      ↓
Router Forwarding
      ↓
NAT/PAT
      ↓
Internet
      ↓
Load Balancer
      ↓
Web Server
      ↓
HTTP Response 📥
      ↓
TCP Close 🔚
```

One nuance: **TLS has its own close/shutdown signaling**, so in a real HTTPS connection, TLS shutdown and TCP termination are separate layers. Your course is focusing here on the TCP part.

---

# 2. Why Does TCP Need to Close?

TCP is **full-duplex**.

That means communication exists in two independent directions:

```text
Client ─────────→ Server
Client ←───────── Server
```

So one side saying:

> "I'm finished sending"

doesn't necessarily mean the other side is finished too.

That's why each direction needs to be closed independently.

---

# 3. Step 1 — Client Sends FIN 📤

Suppose the browser is finished sending data.

It sends:

```text id="5m7s5r"
Client → Server

FIN
```

Meaning:

> **"I have no more data to send."**

The client enters a state such as:

```text id="w1v5h6"
FIN-WAIT-1
```

---

# 4. Step 2 — Server Sends ACK ✅

The server receives the FIN:

```text id="2ls7sk"
Server → Client

ACK
```

Meaning:

> **"I received your FIN."**

But the server may still have data to send.

Therefore:

```text id="fz8m44"
FIN received
      ≠
Entire connection immediately closed
```

---

# 5. Step 3 — Server Sends FIN

Once the server is also finished sending:

```text id="hxuz4b"
Server → Client

FIN
```

Meaning:

> **"I'm finished sending too."**

---

# 6. Step 4 — Client Sends Final ACK

The client responds:

```text id="5z9w2v"
Client → Server

ACK
```

Now both directions have been closed.

```text id="3dlquz"
FIN
 ↓
ACK
 ↓
FIN
 ↓
ACK

Connection gracefully terminated ✅
```

---

# 7. Why Four Segments?

This is the key idea:

```text id="8mkw9v"
Client → Server
"I'm done sending."     → FIN

Server → Client
"I heard you."          → ACK

Server → Client
"I'm done sending too." → FIN

Client → Server
"I heard you."          → ACK
```

Because TCP is full-duplex, **each direction shuts down separately**.

---

# 8. `TIME_WAIT` ⏳

After sending the final ACK, the side that actively closes the connection will commonly enter:

```text id="yjl7z3"
TIME_WAIT
```

Why?

It provides time for delayed packets from the old connection to expire and allows a retransmitted final FIN to be acknowledged if necessary.

So you may see:

```text id="e3tm9w"
FIN
 ↓
ACK
 ↓
FIN
 ↓
ACK
 ↓
TIME_WAIT
 ↓
CLOSED
```

The exact duration is implementation-dependent.

---

# 9. What About `RST`?

Not every connection ends gracefully.

TCP also has:

```text id="kwtne0"
RST
```

**RST = Reset**

It's used to abruptly terminate/reset a connection rather than performing the normal FIN-based shutdown.

For example, an application or TCP stack may reset a connection because of an invalid/unexpected condition.

So:

```text id="7rkkg0"
Normal close → FIN/ACK
Abrupt reset → RST
```

---

# 10. What About HTTPS?

Here's an important layering detail.

For HTTPS, there are two separate concepts:

```text id="1jvzqt"
TLS shutdown
      ↓
TCP shutdown
```

TLS can exchange a `close_notify` alert to indicate that the secure TLS session is being closed.

Then TCP can perform its own connection termination:

```text id="5g7qjo"
TLS close
   ↓
TCP FIN / ACK
```

You don't need to memorize the exact ordering for every implementation, but remember:

> **TLS and TCP have separate connection-lifecycle mechanisms.**

---

# 11. What Would This Look Like in Wireshark? 🦈

You might see:

```text id="2d9f5n"
TCP  FIN, ACK
TCP  ACK
TCP  FIN, ACK
TCP  ACK
```

Depending on the implementation, FIN and ACK may be combined in one segment.

So the conceptual four-way close can appear as fewer visible packets.

---

# 12. What Happens to the NAT/PAT Mapping?

Remember your home router created something like:

```text id="94x3hs"
192.168.1.25:53142
        ↓
203.x.x.x:62001
```

When the connection ends, the router eventually removes/expires the corresponding NAT/PAT state according to its connection tracking behavior.

So conceptually:

```text id="88h2b9"
TCP connection closes
       ↓
NAT/PAT state eventually removed
       ↓
Resources become available
```

---

# 13. The Complete Internet Journey 🌍🔥

Now you can tell the entire story from beginning to end.

Suppose you type:

```text id="a5s9xb"
https://example.com
```

### Step 1 — Browser Cache

```text id="2xqn3p"
"Do I already have a usable resource?"
```

### Step 2 — DNS

```text id="x0jh0e"
example.com
     ↓
IP address
```

### Step 3 — DHCP

Only if your device needs network configuration.

```text id="ub9x0h"
IP
Gateway
DNS
Lease
```

### Step 4 — ARP

If necessary:

```text id="dr7y7x"
Gateway IP
     ↓
Gateway MAC
```

### Step 5 — TCP Handshake

```text id="4e9gyo"
SYN
↓
SYN-ACK
↓
ACK
```

### Step 6 — TLS Handshake

```text id="ojqf13"
Authenticate
+
Key exchange
+
Session keys
```

### Step 7 — HTTP Request

```http id="g4i8ke"
GET / HTTP/1.1
Host: example.com
```

### Step 8 — Router Forwarding

```text id="1yq06u"
Destination IP
     ↓
Routing table
     ↓
Next hop
```

### Step 9 — NAT/PAT

```text id="a7r6co"
Private address
     ↓
Public-side mapping
```

### Step 10 — Internet

```text id="otqzkg"
Router → Router → Router → ...
```

### Step 11 — Load Balancer

```text id="o8nf8r"
Incoming traffic
      ↓
Choose backend
```

### Step 12 — Web Server

```text id="q5o0nw"
HTTP request
      ↓
Application
      ↓
Database/cache if needed
```

### Step 13 — HTTP Response

```http id="zqy8f3"
HTTP/1.1 200 OK
Content-Type: text/html
```

### Step 14 — Return Journey

```text id="6ezxqv"
Server
 ↓
Routers
 ↓
NAT/PAT
 ↓
Home Router
 ↓
Your MacBook
```

### Step 15 — TCP Close

```text id="am41gu"
FIN
↓
ACK
↓
FIN
↓
ACK
```

And the connection is gracefully terminated.

---

# 🧠 The Ultimate Mental Model

You should now be able to visualize this:

```text id="e7kfzg"
                    🌐 INTERNET

   YOUR MAC                                  WEB SERVICE
      │                                          │
      │ 1. Cache                                 │
      │ 2. DNS                                   │
      │ 3. ARP                                   │
      │ 4. TCP Handshake                         │
      │ 5. TLS Handshake                         │
      │ 6. HTTP Request                          │
      │                                          │
      │       🏠 Router                          │
      │          ↓                               │
      │       NAT/PAT                            │
      │          ↓                               │
      │    Internet Routers                      │
      │          ↓                               │
      │    Load Balancer                         │
      │          ↓                               │
      │      Web Server                          │
      │          ↓                               │
      │     HTTP Response                        │
      │          ↓                               │
      │    Internet Routers                      │
      │          ↓                               │
      │       NAT/PAT                            │
      │          ↓                               │
      │       Your Mac                           │
      │                                          │
      │ 7. TCP Close                             │
      │                                          │
```

---

# 🔥 Three things I want you to remember

### Connection establishment

```text
SYN → SYN-ACK → ACK
```

### Data communication

```text
HTTP
 ↓
TLS
 ↓
TCP
 ↓
IP
 ↓
Ethernet/Wi-Fi
```

### Connection termination

```text
FIN → ACK → FIN → ACK
```

And the most important overall idea:

> **A web request isn't one magical trip from your browser to a server. It's a chain of different protocols and devices, each responsible for a specific job.**

---

# 🏆 MODULE 14 — COMPLETE

✅ Browser Cache  
✅ DNS Lookup  
✅ ARP  
✅ DHCP (if needed)  
✅ TCP Handshake  
✅ TLS Handshake  
✅ HTTP Request  
✅ Router Forwarding  
✅ NAT  
✅ Load Balancer  
✅ Web Server  
✅ HTTP Response  
✅ **TCP Close**

You have now completed the **entire Internet Journey + Revision** module. 🎉

And more importantly, you can connect nearly everything you've studied:

```text
DNS → ARP → IP → Routing → TCP → TLS → HTTP
                    ↓
              NAT / Load Balancer
                    ↓
                 Server
```

That's the foundation you need before going deeper into **network security, distributed systems, backend infrastructure, and real-world system design**.