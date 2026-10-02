<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 14: Complete Internet Journey + Revision* • **Topic 07 of 13** (Global #106)

| [⬅️ Previous: 🔐 TLS Handshake](./06_TLS_Handshake.md) | 📑 [**Module Overview**](./README.md) | [Next: 🚦 Router Forwarding — In the Complete Internet Journey ➡️](./08_Router_Forwarding.md) |
| :--- | :---: | ---: |

---

</div>

# 📤 HTTP Request — In the Complete Internet Journey

We've now reached the point where the browser finally says:

> **"Okay, I've found the server, established TCP, secured the connection with TLS — now here's what I actually want."**

That "what I want" is the **HTTP request**.

---

# 1. Where HTTP Request Fits

For a typical HTTPS connection using TCP:

```text
Browser
   ↓
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
HTTP Request 📤  ← YOU ARE HERE
   ↓
Router Forwarding
   ↓
NAT
   ↓
Load Balancer
   ↓
Web Server
   ↓
HTTP Response 📥
```

One important correction to keep straight:

> **The HTTP request is created by the browser, then TLS encrypts it, and the resulting encrypted data is carried by TCP/IP packets through routers.**

The routers and NAT devices are **not processing the HTTP request as HTTP** in the normal case.

---

# 2. What Does the Browser Actually Create?

Suppose you visit:

```text
https://example.com/products?id=42
```

Conceptually, the browser creates:

```http
GET /products?id=42 HTTP/1.1
Host: example.com
Accept: text/html
User-Agent: ...
Cookie: session_id=abc123
```

Let's break this apart.

---

# 3. Request Line

```http
GET /products?id=42 HTTP/1.1
```

### `GET`

HTTP method:

> "I want to retrieve something."

### `/products?id=42`

The requested resource and query parameters.

```text
/products
     ↓
Path

id=42
     ↓
Query
```

### `HTTP/1.1`

The HTTP version in this example.

---

# 4. Headers

The browser adds metadata:

```http
Host: example.com
Accept: text/html
User-Agent: ...
Cookie: session_id=abc123
```

For example:

### Host

```http
Host: example.com
```

Tells the HTTP server which hostname the request targets.

### Accept

```http
Accept: text/html
```

Indicates the content types the client is willing to receive/prefer.

### Cookie

```http
Cookie: session_id=abc123
```

Can carry previously stored cookie data back to the server.

---

# 5. Request Body

A GET request often has no body.

But a POST request can contain data:

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "name": "Devasish"
}
```

So:

```text
GET
→ commonly retrieves

POST
→ commonly sends data
```

---

# 6. Now HTTPS Changes the Picture 🔐

Here's the crucial part.

The browser has created:

```http
GET /products?id=42 HTTP/1.1
Host: example.com
Cookie: ...
```

But because you're using:

```text
https://
```

the HTTP data is passed into the TLS-protected connection.

Conceptually:

```text
HTTP Request
     ↓
TLS encryption
     ↓
Encrypted TLS Application Data
     ↓
TCP
     ↓
IP
     ↓
Wi-Fi / Ethernet
```

So an observer on the network doesn't normally see:

```http
GET /products?id=42
Cookie: session_id=abc123
```

as plaintext.

They see encrypted traffic and various network-layer metadata.

---

# 7. Then TCP Carries It

Remember what TCP does:

```text
HTTP data
   ↓
TCP
   ↓
Segments
```

TCP handles things like:

```text
Sequence numbers
Acknowledgements
Retransmission
Ordering
Flow control
Congestion control
```

So:

```text
HTTP
 ↓
TLS
 ↓
TCP
```

---

# 8. Then IP Handles Routing 🌍

TCP data goes inside an IP packet.

For example:

```text
Source IP:
192.168.1.25

Destination IP:
93.184.216.34
```

Now the packet can be routed toward the server.

---

# 9. What About the Ethernet/Wi-Fi Frame?

Your machine still has to send the packet over the **local link**.

For a remote destination:

```text
Destination IP
= 93.184.216.34

Destination MAC
= Router's MAC
```

So the first frame is roughly:

```text
Ethernet / Wi-Fi
 ├── Destination MAC → Router
 ├── Source MAC      → MacBook
 │
 └── IP
      ├── Source IP      → MacBook
      └── Destination IP → Server
```

This connects directly to your ARP lesson.

---

# 10. Router Forwarding 🚦

The router receives the frame.

It looks at:

```text
Destination IP = 93.184.216.34
```

Then:

```text
Routing table
      ↓
Longest Prefix Match
      ↓
Choose next hop/interface
      ↓
Create new Layer-2 frame
      ↓
Forward
```

The HTTP request is still inside the packet, but the router doesn't normally need to understand the HTTP contents to route it.

---

# 11. NAT/PAT 🔄

At your home router, IPv4 NAT/PAT may translate the connection.

For example:

```text
Before:

192.168.1.25:53142
        ↓
93.184.216.34:443
```

After translation:

```text
203.x.x.x:62001
        ↓
93.184.216.34:443
```

The router tracks the mapping so the response can return to your MacBook.

Again:

> **NAT/PAT operates on network/transport information, not on the HTTP request itself.**

---

# 12. Internet Routers

The packet may travel through several routers:

```text
Your MacBook
     ↓
Home Router
     ↓
ISP Router
     ↓
Transit Router
     ↓
Destination Network
     ↓
Server Infrastructure
```

At every hop:

```text
Layer-2 frame → replaced
IP packet      → forwarded
```

The HTTP request remains protected inside TLS.

---

# 13. Load Balancer ⚖️

When the traffic reaches a large web service, there may not be just one web server.

Instead:

```text
                 Internet
                    ↓
              Load Balancer
                /    |    \
               ↓     ↓     ↓
             Web 1 Web 2 Web 3
```

The load balancer can decide which backend should handle the request.

Depending on the architecture, a load balancer may operate at different layers:

```text
Layer 4 → TCP/UDP-level balancing
Layer 7 → HTTP/application-level balancing
```

For an HTTPS connection, where TLS terminates matters: TLS may terminate at the load balancer, at a reverse proxy, or further downstream.

For this course's simplified journey, think:

> **Load balancer distributes incoming traffic among available backend servers.**

---

# 14. Finally — Web Server 🖥️

The request reaches the server/application infrastructure.

For example:

```http
GET /products?id=42
```

The web application might:

```text
Receive request
      ↓
Authenticate user
      ↓
Read query parameter
      ↓
Query database
      ↓
Build response
```

For example:

```text
Database
   ↓
Products
   ↓
Product #42
```

Then the server creates the HTTP response.

---

# 15. The Whole Request Journey 🔥

Now put everything together:

```text
You enter:

https://example.com/products?id=42
                ↓
          Browser Cache
                ↓
            DNS Lookup
                ↓
          Destination IP
                ↓
       ARP for gateway MAC
          (if needed)
                ↓
         TCP Handshake
                ↓
          TLS Handshake
                ↓
        Create HTTP Request
                ↓
          TLS encrypts it
                ↓
             TCP
                ↓
              IP
                ↓
       Home Router / NAT
                ↓
        ISP + Internet
                ↓
          Load Balancer
                ↓
           Web Server
                ↓
          Process Request
                ↓
          HTTP Response
```

---

# 16. One Very Important Layering Picture

This is probably the most useful diagram from today's topic:

```text
HTTP Request
     ↓
TLS encryption
     ↓
TCP segment
     ↓
IP packet
     ↓
Ethernet/Wi-Fi frame
```

At the destination, the reverse happens conceptually:

```text
Ethernet/Wi-Fi
     ↓
IP
     ↓
TCP
     ↓
TLS decryption
     ↓
HTTP Request
```

The server's application finally gets the original HTTP request.

---

# 🧠 Best Mental Model

Think of sending a letter inside several envelopes:

```text
HTTP Request
   ↓
Put into TLS-protected envelope 🔐
   ↓
Put into TCP transport
   ↓
Put into IP packet
   ↓
Put into Ethernet/Wi-Fi frame
```

Routers mainly care about the outer networking information needed to forward it.

The server eventually unwraps the layers and reaches:

```text
HTTP Request
```

---

# 🔥 Must Remember

### What the browser creates:

```http
GET /products?id=42 HTTP/1.1
Host: example.com
Cookie: ...
```

### Then:

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

### During the journey:

```text
MAC → changes hop-by-hop
IP  → used for routing
TCP → carries the byte stream
TLS → protects application data
HTTP → actual web communication
```

And the most important conceptual point:

> **The HTTP request is created at the application layer, encrypted by TLS for HTTPS, transported by TCP, routed by IP, and carried across each local link inside a Layer-2 frame.**

---

## 🎯 Module 14 Progress

✅ Browser Cache  
✅ DNS Lookup  
✅ ARP  
✅ DHCP (if needed)  
✅ TCP Handshake 🤝  
✅ TLS Handshake 🔐  
✅ **HTTP Request 📤**  
⬜ Router Forwarding  
⬜ NAT  
⬜ Load Balancer  
⬜ Web Server

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *How is an HTTPS request protected from packet sniffing on public Wi-Fi?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> The entire HTTP request (method, URL path, headers, cookies, body) is encrypted by the TLS record layer before transmission, appearing as random ciphertext to eavesdroppers.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🔐 TLS Handshake](./06_TLS_Handshake.md) | [**Module 14: Complete Internet Journey + Revision**](./README.md) | [Next: 🚦 Router Forwarding — In the Complete Internet Journey ➡️](./08_Router_Forwarding.md) |

<div align="center">
  <br/>
  <code>[█████████░] 94% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
