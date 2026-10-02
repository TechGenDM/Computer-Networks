<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 14: Complete Internet Journey + Revision* • **Topic 05 of 13** (Global #104)

| [⬅️ Previous: 📡 DHCP — “if needed”](./04_DHCP_if_needed.md) | 📑 [**Module Overview**](./README.md) | [Next: 🔐 TLS Handshake ➡️](./06_TLS_Handshake.md) |
| :--- | :---: | ---: |

---

</div>

# 🤝 TCP Handshake — In the Complete Internet Journey

We already learned the **TCP three-way handshake** separately. Now let's place it into the real process of opening a website.

Suppose you enter:

```text
https://example.com
```

and DNS has already given you:

```text
example.com → 93.184.216.34
```

Your browser now needs a **TCP connection** to the server's HTTPS service.

---

# 1. Where TCP Handshake Fits

For a typical HTTPS connection using TCP:

```text
Browser
   ↓
DNS Lookup ✅
   ↓
ARP (if needed) ✅
   ↓
Router / NAT ✅
   ↓
TCP Handshake 🤝  ← YOU ARE HERE
   ↓
TLS Handshake 🔐
   ↓
HTTP Request 📤
   ↓
HTTP Response 📥
```

So TCP comes **before TLS and HTTP data transfer**.

---

# 2. Client Chooses a Source Port

Suppose your MacBook has:

```text
IP = 192.168.1.25
```

and the server is:

```text
IP = 93.184.216.34
Port = 443
```

Your operating system may choose an ephemeral source port:

```text
192.168.1.25:53142
```

So the connection is conceptually:

```text
192.168.1.25:53142
        ↓
93.184.216.34:443
```

Remember:

```text
Source port      → client's temporary port
Destination 443  → HTTPS service
```

---

# 3. Step 1 — SYN 📤

The client sends:

```text
Client → Server

SYN
Seq = 1000
```

Meaning:

> "I want to establish a TCP connection."

The client enters:

```text
SYN-SENT
```

---

# 4. Step 2 — SYN-ACK 📥

The server responds:

```text
Server → Client

SYN + ACK
Seq = 5000
Ack = 1001
```

This does two things:

```text
SYN
→ "I also want to establish communication."

ACK
→ "I received your SYN."
```

The server's SYN has its own initial sequence number:

```text
5000
```

---

# 5. Step 3 — ACK 📤

The client sends:

```text
Client → Server

ACK
Seq = 1001
Ack = 5001
```

Now both sides know the other's connection-establishment message was received.

```text
TCP Connection = ESTABLISHED ✅
```

---

# 6. The Complete Handshake

```text
Client                              Server
  │                                   │
  │ SYN                               │
  │ Seq = 1000                        │
  │─────────────────────────────────→ │
  │                                   │
  │             SYN + ACK             │
  │             Seq = 5000            │
  │             Ack = 1001            │
  │ ←──────────────────────────────── │
  │                                   │
  │ ACK                               │
  │ Seq = 1001                        │
  │ Ack = 5001                        │
  │─────────────────────────────────→ │
  │                                   │
  │       CONNECTION ESTABLISHED      │
```

Remember:

```text
SYN → SYN-ACK → ACK
```

---

# 7. Why Does the Browser Need This?

Because TCP wants to establish the state required for its transport service, including:

```text
Sequence numbers
Acknowledgement state
TCP options
Receive-window information
```

This prepares the endpoints for the reliable ordered byte stream that follows.

---

# 8. What Happens Immediately After?

For HTTPS over TCP:

```text
TCP Handshake
      ↓
TCP connection established
      ↓
TLS Handshake
      ↓
Secure encrypted connection
      ↓
HTTP Request
```

So the browser doesn't normally send the actual HTTP request *before* TCP is established.

---

# 9. Where Does NAT Fit?

This is an important connection to your previous topics.

Your laptop might be:

```text
192.168.1.25:53142
```

The home router may translate that using PAT:

```text
192.168.1.25:53142
        ↓
203.x.x.x:62001
```

The server therefore sees the connection coming from the router's external-side address/port.

Conceptually:

```text
MacBook
192.168.1.25:53142
       ↓
    🏠 Router
       ↓ PAT
203.x.x.x:62001
       ↓
93.184.216.34:443
```

The router keeps the translation state so returning TCP packets can be mapped back to your MacBook.

---

# 10. What if the Handshake Fails?

Suppose you see:

```text
SYN
SYN
SYN
SYN
...
```

but no:

```text
SYN-ACK
```

That tells you the TCP connection isn't being successfully established.

Possible causes include:

```text
Server unavailable
Routing problem
Firewall/filtering
Wrong destination/port
Return-path problem
```

You can investigate this using:

```text
traceroute
tcpdump
Wireshark
```

For example, Wireshark can show:

```text
SYN
   ↓
No SYN-ACK
   ↓
Retransmission
```

This is exactly where packet-capture knowledge becomes useful.

---

# 11. Important Exception: HTTP/3

There's an important modern networking detail.

The flow above is for **HTTP over TCP**, such as HTTP/1.1 and HTTP/2.

**HTTP/3 uses QUIC over UDP**, so it does **not** perform a TCP three-way handshake.

Instead:

```text
HTTP/3
   ↓
QUIC
   ↓
UDP
```

So don't memorize:

> "Every website always performs a TCP handshake."

The accurate statement is:

> **A TCP handshake is required when the application is using TCP.**

---

# 🧠 Best Mental Model

Think of the TCP handshake as **opening a reliable communication channel before talking**:

```text
Client:
"Can we connect?"       → SYN

Server:
"Yes, I heard you."     → SYN-ACK

Client:
"I heard you too."     → ACK

          ↓

      TCP Ready ✅
```

Then:

```text
TCP Ready
   ↓
TLS
   ↓
HTTP
```

---

# 🔥 Must Remember

### Three-way handshake:

```text
SYN
↓
SYN-ACK
↓
ACK
```

### It establishes:

```text
TCP connection state
Initial sequence numbers
Acknowledgement state
TCP options
```

### In the Internet journey:

```text
DNS
 ↓
ARP (if needed)
 ↓
Routing / NAT
 ↓
TCP Handshake
 ↓
TLS
 ↓
HTTP
```

### Modern exception:

```text
HTTP/1.1, HTTP/2 → commonly TCP
HTTP/3           → QUIC over UDP
```

---

## 🎯 Module 14 Progress

✅ Browser Cache  
✅ DNS Lookup  
✅ ARP  
✅ DHCP (if needed)  
✅ **TCP Handshake 🤝**  
⬜ TLS Handshake  
⬜ HTTP Request  
⬜ Router Forwarding  
⬜ NAT  
⬜ Load Balancer  
⬜ Web Server

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What is the exact sequence of flag bits exchanged in the TCP 3-way handshake?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> Client sends **SYN** $\rightarrow$ Server replies **SYN + ACK** $\rightarrow$ Client replies **ACK**.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 📡 DHCP — “if needed”](./04_DHCP_if_needed.md) | [**Module 14: Complete Internet Journey + Revision**](./README.md) | [Next: 🔐 TLS Handshake ➡️](./06_TLS_Handshake.md) |

<div align="center">
  <br/>
  <code>[█████████░] 92% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
