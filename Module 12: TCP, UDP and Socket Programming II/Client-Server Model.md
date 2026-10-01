# 🖥️↔️🖥️ Client-Server Model

This is where **everything we've learned about IPs, ports, TCP/UDP, and sockets** comes together.

The basic idea is:

> **A client requests a service; a server provides the service.**

---

# 1. What is a Client?

A **client** is a program/device that **requests a service or resource**.

Examples:

```text
🌐 Browser
📱 Mobile app
💻 React frontend
🖥️ Desktop application
```

For example, when your browser requests:

```http
GET /products
```

the browser is acting as the **client**.

---

# 2. What is a Server?

A **server** is a program that **listens for requests and provides a service or resource**.

Examples:

```text
Web server
API server
Database server
DNS server
Mail server
```

Important:

> A server is usually **software running on a machine**, not necessarily a special physical computer.

Your Mac can act as a server when you run:

```text
localhost:3000
```

with a development server.

---

# 3. Basic Communication

```text
Client                           Server
  │                                │
  │ ─────── Request ─────────────→ │
  │                                │
  │ ←────── Response ───────────── │
  │                                │
```

For an HTTP application:

```text
Browser                         Web Server
   │                                │
   │ GET /products                  │
   │ ─────────────────────────────→ │
   │                                │
   │ 200 OK + product data          │
   │ ←───────────────────────────── │
```

---

# 4. Where Do Sockets Come In?

Remember our previous topic:

```text
Socket = application's network communication endpoint
```

A typical TCP server does:

```text
socket()
   ↓
bind()
   ↓
listen()
   ↓
accept()
```

The client does:

```text
socket()
   ↓
connect()
```

Then they communicate:

```text
send() / receive()
```

So the complete picture is:

```text
Client
  │
  │ IP + Port
  ↓
Network
  │
  ↓
Server Socket
```

---

# 5. A Real Example

Suppose you build a backend in Node.js:

```text
Server:
192.168.1.20:8000
```

Your frontend is running on:

```text
localhost:3000
```

The frontend makes:

```http
GET http://192.168.1.20:8000/api/users
```

Conceptually:

```text
React Frontend
      │
      │ TCP
      ↓
192.168.1.20:8000
      │
      ↓
Node.js Backend
      │
      ↓
Database
```

The backend processes the request and sends the response back.

---

# 6. Client and Server Roles

### Client

Usually:

```text
Initiates communication
Sends requests
Consumes responses/services
```

### Server

Usually:

```text
Waits for incoming connections/requests
Processes requests
Provides responses/services
```

The important word is **usually**.

A machine can be both:

```text
Client for one service
        +
Server for another service
```

For example, your backend can be:

```text
Client → PostgreSQL database
Server → React frontend
```

So "client" and "server" describe **roles in a particular communication**, not permanent identities of machines.

---

# 7. One Server, Many Clients

A server isn't normally limited to one client.

```text
             Server
               │
       ┌───────┼────────┐
       ↓       ↓        ↓
    Client A Client B Client C
```

For example:

```text
                  Node.js Server
                       :8000
                    /    |    \
                   /     |     \
             Browser   Mobile  Another API
```

Each client can have its own connection.

For TCP, the connections are distinguishable by their endpoint information.

---

# 8. Request-Response Cycle

Suppose you open an e-commerce website.

```text
1. Client requests page
        ↓
2. Server receives request
        ↓
3. Server processes request
        ↓
4. Server may query database
        ↓
5. Server creates response
        ↓
6. Client receives response
        ↓
7. Browser displays result
```

For example:

```text
Browser
   │
   │ GET /products
   ↓
Backend
   │
   │ SQL query
   ↓
Database
   │
   │ Product data
   ↓
Backend
   │
   │ JSON / HTML
   ↓
Browser
```

---

# 9. Client-Server vs Peer-to-Peer

It's useful to distinguish these architectures.

### Client-Server

```text
Client → Server
Client → Server
Client → Server
```

The server provides a centralized service.

### Peer-to-Peer

```text
Peer ↔ Peer
Peer ↔ Peer
Peer ↔ Peer
```

Participants can communicate more directly with each other.

Modern systems can also use **hybrid architectures**, combining both models.

---

# 10. Connection to HTTP

HTTP uses the client-server model.

For example:

```text
Client
  ↓
HTTP Request
  ↓
Server
  ↓
HTTP Response
  ↓
Client
```

And underneath:

```text
HTTP
 ↓
TCP
 ↓
IP
 ↓
Wi-Fi / Ethernet
```

So we've now connected:

```text
Client-Server
      ↓
Socket
      ↓
TCP / UDP
      ↓
Ports
      ↓
IP
```

---

# 🧠 The complete mental model

Imagine a restaurant 🍽️:

```text
Client = Customer
Server = Restaurant
Request = Order
Response = Food / Result
Port = Service counter/entry point
Socket = Communication endpoint
```

The customer doesn't need to know how the kitchen operates internally.

They simply:

```text
Request → Service → Response
```

That's the essence of the client-server model.

---

# 🔥 Must Remember

> **Client-server architecture is a model where a client initiates requests for services/resources and a server listens for and processes those requests.**

Remember this flow:

```text
CLIENT
  ↓
Socket
  ↓
IP + Port
  ↓
Network
  ↓
Server Socket
  ↓
Server Application
  ↓
Response
  ↓
CLIENT
```

And one subtle point:

> **Client and server are roles, not necessarily different physical machines.**

Your own Mac can simultaneously be a **client** and a **server**.

---

## Module 12 Progress

✅ Sliding Window  
✅ Socket Programming Concepts 🔌  
✅ **Client-Server Model 🖥️↔️🖥️**  
⬜ ARP  
⬜ NAT  
⬜ PAT  
⬜ DHCP  
⬜ Lease Process  
⬜ Default Gateway  
⬜ Home Router  
⬜ Private Networks

**Next → ARP 🔎** — we'll understand how a machine discovers the **MAC address corresponding to a local IPv4 address**, and exactly where ARP fits between IP and Ethernet.