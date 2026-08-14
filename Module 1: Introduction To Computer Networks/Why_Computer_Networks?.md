# 1. Why Computer Networks?

## What is a Computer Network?

A **computer network** is a collection of devices connected together to communicate and share data/resources.
### Example: Accessing Google

```
Your Laptop
    │
    │ (Wi-Fi)
    ▼
  Router
    │
    ▼
 Internet
    │
    ▼
Google Server
```

When you visit `google.com`, your laptop communicates with Google's servers through a network.
## Why Do We Need Networks?

### 1. **Communication**
Devices can communicate with each other.

```
Phone ──► WhatsApp Server ──► Friend's Phone
```

### 2. **Resource Sharing**
Multiple computers can share resources.

```
PC 1 ──┐
PC 2 ──┼──► Shared Printer
PC 3 ──┘
```

### 3. **Data Sharing**
Files, databases, websites, videos, etc. can be transferred between machines.

### 4. **Remote Access**
You can access something that is physically somewhere else.

```
Your Laptop ──► GitHub
Your Laptop ──► AWS Server  
Your Laptop ──► College Server
```

### 5. **Distributed Systems**
Modern applications rarely run on one computer.

```
         ┌──► Web Server
         │
You ─────┼──► API Server
         │
         ├──► Database
         │
         └──► Cache
```

These machines communicate over networks.
## 🧠 Key Developer Insight

Don't think of networking as:
> *"Computers connected with cables."*

Think of it as:
> **Networking = Moving information from one device/process to another reliably and efficiently.**

This becomes critical when learning:
- Backend development
- APIs & REST
- Distributed Systems
- Cloud computing (AWS, Azure, GCP)
- Containerization (Docker, Kubernetes)
- WebSockets & real-time communication
- Security & encryption

### Example: What Happens When Frontend Calls Backend

When your React frontend calls:
```javascript
GET https://api.example.com/users/123
```

There's a huge amount of networking happening underneath:

```
Browser
   ↓
DNS (Resolve domain → IP)
   ↓
TCP (Establish connection)
   ↓
TLS/HTTPS (Secure the connection)
   ↓
Internet (Route through network)
   ↓
Routers (Forward packets)
   ↓
Server (Receive request)
   ↓
Backend (Process request)
   ↓
Database (Fetch data)
```

**Computer Networks explains every step above.**
## 🎯 Mental Model: WHO → HOW → WHERE → WHAT

Whenever two computers communicate, ask yourself:
### **WHO?**
Sender and Receiver

### **HOW?**
Network protocols (TCP, UDP, HTTP, etc.)

### **WHERE?**
IP addresses & ports

### **WHAT?**
Data / packets

---

**We'll build on this mental model throughout the entire networking course.**