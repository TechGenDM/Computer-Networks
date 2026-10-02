<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 11: TCP, UDP and Socket Programming* • **Topic 03 of 08** (Global #073)

| [⬅️ Previous: 🔌 Ports](./02_Ports.md) | 📑 [**Module Overview**](./README.md) | [Next: ⚡ UDP — User Datagram Protocol ➡️](./04_UDP.md) |
| :--- | :---: | ---: |

---

</div>

# 🛡️ TCP — Transmission Control Protocol

Now we get to one of the **most important protocols in networking**.

You already know:

```text
IP   → Which machine?
Port → Which application?
```

TCP adds something more:

> **Reliable, ordered, connection-oriented communication between applications.**

---

# 1. What is TCP?

**TCP (Transmission Control Protocol)** is a **connection-oriented transport-layer protocol**.

It provides mechanisms for:

```text
✅ Reliable delivery
✅ Ordered delivery
✅ Error detection
✅ Retransmission
✅ Flow control
✅ Congestion control
✅ Full-duplex communication
```

So instead of simply throwing packets onto the network, TCP tries to provide an **ordered and reliable byte stream** to the application.

---

# 2. TCP is Connection-Oriented

Before data is normally exchanged, TCP establishes a connection.

Conceptually:

```text
Client
   │
   │ Establish connection
   ↓
Server
   │
   │ Connection established
   ↓
Data exchange
```

This establishment process is called the:

> **Three-way handshake**

We'll study that separately next.

---

# 3. TCP Provides a Byte Stream

This is a very important concept.

Suppose your application sends:

```text
Hello World
```

TCP treats the application data as a **continuous stream of bytes**.

It doesn't preserve application message boundaries in the way UDP datagrams do.

For example, the application might do:

```text
send("Hello")
send(" World")
```

The receiving TCP application reads from a byte stream; it isn't guaranteed to receive those as exactly two separate chunks.

Think:

```text
Application
   ↓
BYTE STREAM
   ↓
TCP
```

---

# 4. TCP Reliability 🔐

Networks can lose packets.

Suppose you send:

```text
Segment 1
Segment 2
Segment 3
Segment 4
```

but Segment 3 disappears:

```text
1 ✅
2 ✅
3 ❌
4 ✅
```

TCP can detect that something is missing and arrange for the lost data to be retransmitted.

Conceptually:

```text
Sender ── 1 ──→ Receiver ✅
Sender ── 2 ──→ Receiver ✅
Sender ── 3 ──→ Receiver ❌
Sender ── 4 ──→ Receiver ✅

           ↓

TCP detects missing data

           ↓

Retransmit missing data
```

That's one reason TCP is called **reliable**.

---

# 5. Sequence Numbers 🔢

How does TCP know what data is missing or out of order?

It uses **sequence numbers**.

Imagine:

```text
Segment A → sequence 1000
Segment B → sequence 1500
Segment C → sequence 2000
```

The receiver can determine the ordering of the byte stream.

Suppose it receives:

```text
1000 ✅
2000 ✅
```

but:

```text
1500 ❌
```

It knows there's missing data.

Sequence numbers are therefore central to:

```text
✅ Ordering
✅ Detecting missing data
✅ Tracking received bytes
```

---

# 6. Acknowledgements (ACKs)

TCP uses acknowledgements to tell the sender what data has been received.

Conceptually:

```text
Sender
  │
  │ Data
  ↓
Receiver
  │
  │ ACK
  ↓
Sender
```

For example:

```text
Sender → Data
Receiver → ACK
```

The ACK tells the sender:

> "I have successfully received data up to this point."

---

# 7. Retransmission

Suppose the sender doesn't receive the expected acknowledgement.

TCP can infer that data may have been lost and retransmit it.

```text
Sender
   │
   │ Segment
   ↓
  ❌ lost
   │
   │ no ACK
   ↓
Timeout / loss detection
   ↓
Retransmit
```

This is a major part of TCP reliability.

---

# 8. Ordered Delivery

Packets don't necessarily travel through the network at exactly the same speed.

Suppose:

```text
Sender sends:

A
B
C
```

The receiver could physically receive:

```text
A
C
B
```

TCP uses sequence numbers and buffering to reconstruct the correct byte order for the application:

```text
A
B
C
```

So the application gets an **ordered byte stream**.

---

# 9. Error Detection

TCP includes a **checksum** to detect corruption in a TCP segment.

So if data is altered during transmission:

```text
Original
   ↓
Network
   ↓
Corrupted ❌
   ↓
Checksum doesn't match
```

TCP can detect the problem.

Important:

> The checksum detects corruption; reliability also depends on ACKs, sequence numbers, retransmission, etc.

---

# 10. Full-Duplex Communication

TCP allows both sides to send data simultaneously.

```text
Client ─────────────→ Server
       ←─────────────
```

For example:

```text
Client sends request
Server sends response

Both directions can carry data.
```

This is called **full-duplex** communication.

---

# 11. TCP Connection Example

Suppose your browser connects to an HTTPS server:

```text
Client:
192.168.1.10:53021

Server:
203.0.113.10:443
```

TCP establishes a connection between these endpoints.

Conceptually:

```text
192.168.1.10:53021
        │
        │ TCP connection
        ↓
203.0.113.10:443
```

Then data can flow in both directions.

---

# 12. TCP and HTTP

You've already learned HTTP.

A traditional HTTP/1.1 or HTTP/2 stack looks roughly like:

```text
HTTP
 ↓
TCP
 ↓
IP
 ↓
Ethernet / Wi-Fi
```

So when your browser sends an HTTP request:

```text
GET /products
```

the application data is handed to TCP.

TCP handles:

```text
Segmentation
Sequence numbers
ACKs
Retransmission
Ordering
Flow control
Congestion control
```

Then IP handles routing.

---

# 13. TCP is Not the Same as IP

Don't mix these up.

### IP

Responsible for things like:

```text
Addressing
Routing
Best-effort packet delivery
```

### TCP

Responsible for things like:

```text
Reliable transport
Ordering
Retransmission
Flow control
Congestion control
```

Mental model:

```text
IP:
"Let's try to get this packet to that machine."

TCP:
"Let's make sure the application gets an ordered,
reliable stream of bytes."
```

---

# 14. TCP Segment

TCP divides the byte stream into units called **segments**.

A simplified TCP segment looks like:

```text
┌─────────────────────────────┐
│ Source Port                 │
│ Destination Port            │
├─────────────────────────────┤
│ Sequence Number             │
│ Acknowledgment Number       │
├─────────────────────────────┤
│ Flags                       │
│ Window Size                 │
├─────────────────────────────┤
│ Checksum                    │
│ ...                         │
├─────────────────────────────┤
│ Application Data            │
└─────────────────────────────┘
```

You don't need to memorize every TCP header field yet.

The important ones for now are:

```text
Ports
Sequence Number
Acknowledgment Number
Flags
Window
Checksum
```

---

# 15. TCP's Three Big Guarantees/Mechanisms

A useful way to organize TCP in your mind:

### Reliability

```text
Sequence numbers
+
ACKs
+
Retransmissions
```

### Flow Control

```text
"Don't send faster than the receiver can handle."
```

### Congestion Control

```text
"Don't overwhelm the network."
```

Your module has **Flow Control** later, and congestion control is closely related but isn't listed in your screenshot's topic sequence.

---

# 🧠 Best mental model

Imagine sending a valuable package.

UDP is roughly:

> "Here's the package. Send it."

TCP is more like:

> "Let's establish communication, number the pieces, confirm what arrived, resend missing pieces, keep them in order, and avoid overwhelming the receiver/network."

---

# 🔥 Must remember

```text
TCP = Transmission Control Protocol

TCP is:
✅ Connection-oriented
✅ Reliable
✅ Ordered
✅ Full-duplex
✅ Byte-stream based

TCP uses:
→ Sequence numbers
→ ACKs
→ Retransmission
→ Checksum
→ Flow control
→ Congestion control
```

And:

```text
IP  → host-to-host delivery
TCP → process-to-process reliable byte stream
```

---

## Module 11 Progress

✅ Transport Layer  
✅ Ports 🔌  
✅ **TCP 🛡️**  
⬜ UDP  
⬜ Three-way Handshake 🤝  
⬜ Four-way Close

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *Why is TCP referred to as a 'byte-stream' protocol rather than a 'message-oriented' protocol?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> TCP does not preserve application message boundaries. If an app writes two 50-byte messages, TCP may deliver them as one 100-byte segment or four 25-byte segments.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🔌 Ports](./02_Ports.md) | [**Module 11: TCP, UDP and Socket Programming**](./README.md) | [Next: ⚡ UDP — User Datagram Protocol ➡️](./04_UDP.md) |

<div align="center">
  <br/>
  <code>[██████░░░░] 65% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
