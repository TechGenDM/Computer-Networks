# 🤝 TCP Three-Way Handshake

This is how **TCP establishes a connection before normal data transfer**.

The entire handshake is:

```text
Client                         Server
  │                              │
  │ ─────── SYN ───────────────→ │
  │                              │
  │ ←──── SYN + ACK ──────────── │
  │                              │
  │ ─────── ACK ───────────────→ │
  │                              │
  │        Connection Ready      │
```

The three steps are:

```text
SYN
↓
SYN + ACK
↓
ACK
```

---

# 1. Why does TCP need a handshake?

TCP wants both sides to establish the connection state needed for reliable communication.

The handshake essentially lets the two endpoints:

- indicate that they want to establish a connection
- exchange **initial sequence numbers**
- acknowledge those sequence numbers
- negotiate certain TCP options

Think of it as:

```text
Client: "Can we communicate?"
Server: "Yes, I can communicate. Can you hear me?"
Client: "Yes."
```

Now both sides are ready for normal TCP data transfer.

---

# 2. Step 1 — SYN 📤

The client sends a TCP segment with the **SYN flag** set.

```text
Client → Server

SYN
Sequence Number = 1000
```

Meaning roughly:

> "I want to establish a TCP connection, and my initial sequence number is 1000."

The client enters a state such as:

```text
SYN-SENT
```

---

# 3. Step 2 — SYN + ACK 📥

The server receives the SYN and responds with **both SYN and ACK**.

```text
Server → Client

SYN + ACK
Sequence Number = 5000
Acknowledgment Number = 1001
```

Two things are happening:

### SYN

The server says:

> "I also want to establish my side of the connection."

So it chooses its own initial sequence number:

```text
5000
```

### ACK

The server acknowledges the client's SYN:

```text
1000 + 1 = 1001
```

So:

```text
ACK = 1001
```

The server is essentially saying:

> "I received your SYN with sequence 1000; I expect 1001 next."

---

# 4. Step 3 — ACK 📤

The client responds:

```text
Client → Server

ACK
Sequence Number = 1001
Acknowledgment Number = 5001
```

The client acknowledges the server's SYN:

```text
5000 + 1 = 5001
```

Now:

```text
Client ✅
Server ✅
```

The TCP connection is established.

---

# 5. Put the numbers together 🔢

Here's the classic example:

```text
Client                              Server
  │                                   │
  │ SYN                               │
  │ Seq = 1000                       │
  │─────────────────────────────────→│
  │                                   │
  │                SYN + ACK          │
  │                Seq = 5000         │
  │                Ack = 1001         │
  │←─────────────────────────────────│
  │                                   │
  │ ACK                               │
  │ Seq = 1001                       │
  │ Ack = 5001                       │
  │─────────────────────────────────→│
  │                                   │
  │        TCP CONNECTION READY       │
```

### Why `1001`?

Because the SYN itself consumes **one sequence number**.

Similarly:

```text
5000 → 5001
```

because the server's SYN also consumes one sequence number.

---

# 6. What is actually being established?

The handshake establishes state at both endpoints, including information needed for the TCP connection.

It also provides an opportunity to negotiate TCP options such as:

```text
MSS
Window Scaling
SACK
Timestamps
```

You don't need to memorize the details yet.

---

# 7. Why THREE steps?

You may wonder:

> Why not just SYN → SYN-ACK?

Because the server needs to know that the client **received the server's response**.

With three steps:

```text
1. Client → Server: SYN
   "I want to connect."

2. Server → Client: SYN-ACK
   "I received that, and here's my side."

3. Client → Server: ACK
   "I received your response too."
```

Now both sides have confirmation that communication is working in both directions.

---

# 8. What happens after the handshake?

Now TCP can carry application data.

For example, with HTTPS:

```text
TCP Three-Way Handshake
          ↓
TCP connection established
          ↓
TLS handshake
          ↓
Encrypted HTTP communication
```

So don't confuse these:

```text
TCP handshake → establishes TCP connection
TLS handshake → establishes cryptographic security
HTTP          → application-layer communication
```

With plain HTTP over TCP:

```text
TCP handshake
      ↓
HTTP request
      ↓
HTTP response
```

With HTTPS:

```text
TCP handshake
      ↓
TLS handshake
      ↓
Encrypted HTTP
```

---

# 9. TCP States

You may encounter these names in networking tools:

```text
CLOSED
   ↓
SYN-SENT
   ↓
ESTABLISHED
```

On the server side, a listening socket can be in:

```text
LISTEN
   ↓
SYN-RECEIVED
   ↓
ESTABLISHED
```

For now, the important one is:

> **ESTABLISHED = TCP connection is ready for normal data transfer.**

---

# 10. TCP vs UDP

This also reinforces what we learned earlier:

```text
TCP
Client ── SYN ──→ Server
Client ← SYN-ACK ─ Server
Client ── ACK ──→ Server
          ↓
       Data
```

UDP:

```text
Client ───── Datagram ─────→ Server
```

There is **no TCP-style three-way handshake in UDP**.

---

# 🧠 Best mental model

Remember:

```text
SYN
"I want to connect."

SYN-ACK
"I heard you, and I want to connect too."

ACK
"I heard you."

         ↓
Connection established ✅
```

And the sequence-number idea:

```text
Client ISN = 1000
Server ISN = 5000

SYN       → Seq 1000
SYN-ACK   → Seq 5000, Ack 1001
ACK       → Seq 1001, Ack 5001
```

---

# 🔥 Interview-ready definition

> **The TCP three-way handshake is the process in which a client and server exchange SYN, SYN-ACK, and ACK segments to establish TCP connection state, synchronize initial sequence numbers, and negotiate TCP parameters before normal data transfer.**

### Module 11 Progress

✅ Transport Layer  
✅ Ports 🔌  
✅ TCP 🛡️  
✅ UDP ⚡  
✅ **Three-way Handshake 🤝**  
⬜ Four-way Close  
⬜ Reliability  
⬜ Flow Control

**Next → Four-way Close 🔚** — we'll see why terminating a TCP connection normally takes **four segments instead of three**.