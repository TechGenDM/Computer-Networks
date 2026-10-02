# 🚦 Router Forwarding — In the Complete Internet Journey

Now our HTTP request has been created, encrypted by TLS, carried by TCP, and placed into an IP packet.

It's time for the packet to **travel across the network**.

The key idea:

> **A router receives a packet, examines its destination IP, looks up the best matching route, and forwards the packet to the next hop.**

---

# 1. Where We Are

Our journey so far:

```text
Browser Cache
     ↓
DNS Lookup
     ↓
ARP (if needed)
     ↓
TCP Handshake 🤝
     ↓
TLS Handshake 🔐
     ↓
HTTP Request 📤
     ↓
Router Forwarding 🚦 ← HERE
     ↓
NAT
     ↓
Load Balancer
     ↓
Web Server
```

---

# 2. The Packet Reaches Your Home Router

Suppose your MacBook has:

```text
IP: 192.168.1.25
```

and wants:

```text
Server: 93.184.216.34
```

Your MacBook sends a local frame to the router.

```text
MacBook
192.168.1.25
     │
     │ Ethernet/Wi-Fi frame
     │ Destination MAC = router
     ↓
Router
192.168.1.1
```

The router receives the frame.

---

# 3. Router Removes the Local Layer-2 Frame

The router doesn't simply forward the exact same Ethernet frame.

It processes the incoming frame and extracts the IP packet:

```text
Incoming:

Ethernet
   ↓
IP
   ↓
TCP
   ↓
TLS
   ↓
HTTP
```

The router primarily needs the IP-layer information for routing.

---

# 4. Router Looks at Destination IP

The router examines:

```text id="gjx1l8"
Destination IP = 93.184.216.34
```

It asks:

> **"Which route should I use to reach this destination?"**

---

# 5. Routing Table Lookup 🗺️

Imagine the router has:

```text id="qflhuz"
Destination       Next Hop
192.168.1.0/24    directly connected
93.184.216.0/24   ISP-A
93.184.0.0/16     ISP-B
0.0.0.0/0         ISP-C
```

For:

```text
93.184.216.34
```

multiple routes might match.

The router uses:

> **Longest Prefix Match**

So a more specific route wins over a broader route.

For example:

```text
93.184.0.0/16
93.184.216.0/24  ← more specific
```

The `/24` route wins.

---

# 6. What Does the Router Learn?

The routing table can come from:

```text
Directly connected routes
Static routes
OSPF
RIP
BGP
Other routing mechanisms
```

This connects directly to the previous routing modules.

The router generally isn't running Dijkstra from scratch for every packet.

Instead:

```text
Routing protocols
      ↓
Build routing information
      ↓
Forwarding information
      ↓
Packet lookup
```

Then packets can be forwarded quickly.

---

# 7. Router Chooses the Next Hop

Suppose the best route says:

```text
93.184.216.0/24
        ↓
Next Hop = ISP Router
```

The router now knows:

```text
"Send this packet to that next hop."
```

It may need to determine the next-hop Layer-2 address on the outgoing link using the appropriate mechanism, such as ARP for IPv4 Ethernet networks.

---

# 8. Router Builds a New Layer-2 Frame

This is extremely important.

Suppose:

```text
Incoming frame:
Source MAC = MacBook
Destination MAC = Home Router
```

After routing:

```text
Outgoing frame:
Source MAC = Home Router
Destination MAC = ISP Router
```

So:

```text
      MAC addresses change
              ↓
MacBook → Router → ISP Router
```

But the IP packet is still being routed toward:

```text
93.184.216.34
```

subject to normal changes such as NAT.

---

# 9. Hop-by-Hop Forwarding

Now imagine:

```text
MacBook
   ↓
Home Router
   ↓
ISP Router 1
   ↓
ISP Router 2
   ↓
Transit Router
   ↓
Destination Network
   ↓
Server
```

Each router repeats roughly:

```text
Receive frame
    ↓
Extract IP packet
    ↓
Check destination IP
    ↓
Forwarding table lookup
    ↓
Choose outgoing interface/next hop
    ↓
Create new Layer-2 frame
    ↓
Transmit
```

That's **hop-by-hop forwarding**.

---

# 10. TTL Gets Decreased

As an IPv4 packet passes through a router:

```text
TTL -= 1
```

For example:

```text
TTL = 64
   ↓ Router 1
TTL = 63
   ↓ Router 2
TTL = 62
   ↓ Router 3
TTL = 61
```

This prevents a packet caught in a routing loop from circulating forever.

If TTL reaches zero, the router discards the packet and may send an:

```text
ICMP Time Exceeded
```

This is exactly the mechanism that `traceroute` exploits.

🔥 So now you can connect:

```text
Router forwarding
      ↓
TTL
      ↓
Traceroute
```

---

# 11. Does the Router Understand HTTP?

Usually, **not for ordinary Layer-3 forwarding**.

The packet conceptually contains:

```text
IP
 ↓
TCP
 ↓
TLS
 ↓
HTTP
```

A normal router can forward it based primarily on:

```text
Destination IP
```

It doesn't need to read:

```http
GET /products
```

to make an ordinary IP routing decision.

That's one of the strengths of layered networking.

---

# 12. Router vs Switch

Another useful distinction:

### Switch

Usually forwards based on:

```text
MAC address
```

within a Layer-2 network.

### Router

Forwards between IP networks based on:

```text
Destination IP
```

So:

```text
Switch → MAC
Router → IP
```

---

# 13. Full Example 🔥

Your MacBook sends:

```text
Source IP:
192.168.1.25

Destination IP:
93.184.216.34
```

### Hop 1

```text
MacBook
    ↓
Home Router
```

Router looks up:

```text
93.184.216.34
```

and forwards toward the ISP.

### Hop 2

```text
Home Router
    ↓
ISP Router
```

ISP router performs another lookup.

### Hop 3

```text
ISP Router
    ↓
Transit Router
```

Another lookup.

And so on.

Every router makes its own **local forwarding decision**.

---

# 14. Why "Local Decision"?

This is important.

Router R1 might choose:

```text
R1 → R2
```

while R2 chooses:

```text
R2 → R5
```

and R5 chooses:

```text
R5 → R7
```

No individual router necessarily needs to know every physical detail of the end-to-end path to perform its forwarding job.

```text
Each router:
"Given this destination, what is my best next step?"
```

That's the essence of hop-by-hop routing.

---

# 🧠 Best Mental Model

Imagine passing a parcel through a series of logistics hubs:

```text
You
 ↓
Local Hub
 ↓
Regional Hub
 ↓
National Hub
 ↓
Destination Hub
 ↓
Recipient
```

At every hub:

> "Which truck should this parcel go on next?"

That's essentially router forwarding.

The router doesn't need to personally carry the parcel all the way to the destination.

---

# 🔥 Must Remember

### Router forwarding process:

```text
Receive Frame
      ↓
Read Destination IP
      ↓
Routing/Forwarding Table Lookup
      ↓
Longest Prefix Match
      ↓
Choose Next Hop / Interface
      ↓
Decrement TTL
      ↓
Build New Layer-2 Frame
      ↓
Forward
```

### Key distinction:

```text
MAC → current local link
IP  → destination used for routing
```

### And:

```text
Switch → forwards frames
Router → forwards packets
```

---

# 🎯 Module 14 Progress

✅ Browser Cache  
✅ DNS Lookup  
✅ ARP  
✅ DHCP (if needed)  
✅ TCP Handshake 🤝  
✅ TLS Handshake 🔐  
✅ HTTP Request 📤  
✅ **Router Forwarding 🚦**  
⬜ NAT  
⬜ Load Balancer  
⬜ Web Server  
⬜ HTTP Response  
⬜ TCP Close  

**Next → NAT 🔄** — we'll follow the packet through your home router and see exactly how `192.168.1.25` is translated into the router's external address before it enters the ISP network.