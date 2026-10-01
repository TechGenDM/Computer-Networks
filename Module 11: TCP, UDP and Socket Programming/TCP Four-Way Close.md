# 🔚 TCP Four-Way Close

We’ve established a TCP connection using:

```text
SYN → SYN-ACK → ACK
```

Now we need to **terminate it gracefully**.

TCP normally uses a **four-segment exchange** for a graceful close because TCP is **full-duplex**: each direction of the connection is closed independently.

---

# 1. The Basic Flow

```text
Client                         Server
  │                              │
  │ ─────── FIN ───────────────→ │
  │                              │
  │ ←──────── ACK ────────────── │
  │                              │
  │ ←──────── FIN ────────────── │
  │                              │
  │ ─────── ACK ───────────────→ │
  │                              │
  │       Connection Closed      │
```

Remember:

```text
FIN → "I'm finished sending."
ACK → "I received your FIN."
```

---

# 2. Step 1 — FIN 📤

Suppose the **client** wants to close the connection.

It sends:

```text
Client → Server
FIN
```

FIN means:

> **"I have no more data to send in this direction."**

The client moves into a state such as:

```text
FIN-WAIT-1
```

---

# 3. Step 2 — ACK 📥

The server receives the FIN and acknowledges it:

```text
Server → Client
ACK
```

Meaning:

> **"I received your FIN."**

The client can now know that its request to close the sending direction was received.

But the connection isn't necessarily completely closed yet.

Why?

Because the server may still have data to send to the client.

---

# 4. Step 3 — Server Sends FIN

Once the server has finished sending its remaining data, it sends its own:

```text
Server → Client
FIN
```

This means:

> **"I'm finished sending too."**

---

# 5. Step 4 — Final ACK 📤

The client responds:

```text
Client → Server
ACK
```

Meaning:

> **"I received your FIN."**

Now both directions have been closed.

---

# 🧠 Why FOUR steps?

This is the key question.

TCP is **full-duplex**:

```text
Client ─────────→ Server
Client ←───────── Server
```

The client and server each have their **own sending direction**.

So closing means:

```text
Client says:
"I'm done sending."     → FIN

Server says:
"I heard you."           → ACK

Server later says:
"I'm done sending too."  → FIN

Client says:
"I heard you."           → ACK
```

Therefore, the normal graceful close is:

```text
FIN
↓
ACK
↓
FIN
↓
ACK
```

---

# 6. Why can it sometimes look like only 3 packets?

An important detail:

The **ACK and FIN from the same endpoint can sometimes be combined** into one TCP segment.

For example:

```text
Client → FIN
Server → ACK
Server → FIN + ACK
Client → ACK
```

So you'll often hear:

> "TCP uses a four-way close."

That's the conceptual sequence, even though actual packets can sometimes be combined.

---

# 7. Sequence Numbers

Just like the handshake, TCP uses sequence and acknowledgment numbers during connection termination.

Conceptually:

```text
Client → FIN, Seq = 1000
Server → ACK, Ack = 1001
Server → FIN, Seq = 5000
Client → ACK, Ack = 5001
```

The important idea:

> **A FIN consumes one sequence number.**

So:

```text
FIN Seq = 1000
ACK = 1001
```

---

# 8. What about `TIME_WAIT`? ⏳

This is a very important TCP state.

After the client sends the final ACK, it normally enters:

```text
TIME_WAIT
```

rather than immediately becoming fully closed.

Why?

Primarily to allow time for delayed segments from the old connection to expire and to ensure that a retransmitted final FIN can still be acknowledged.

So conceptually:

```text
Final ACK
   ↓
TIME_WAIT
   ↓
CLOSED
```

The exact duration depends on the TCP implementation and configuration.

---

# 9. Graceful Close vs Abrupt Close

### Graceful close

Uses:

```text
FIN / ACK
```

This allows both sides to finish sending data properly.

### Abrupt termination

TCP also has the:

```text
RST
```

flag.

An RST can terminate/reset a connection abruptly rather than performing the normal FIN-based shutdown.

For example, you may see:

```text
Connection reset
```

when a TCP connection is reset.

---

# 10. Three-Way Handshake vs Four-Way Close

This is a very useful comparison:

### Connection establishment

```text
SYN
↓
SYN-ACK
↓
ACK
```

### Graceful termination

```text
FIN
↓
ACK
↓
FIN
↓
ACK
```

Mental shortcut:

```text
START → SYN
END   → FIN
```

---

# 11. Real-World Example

Suppose your browser has a TCP connection to a web server:

```text
Browser
192.168.1.10:53021
       │
       │ TCP
       ↓
Server
203.0.113.10:443
```

After communication finishes:

```text
Browser → FIN
Server  → ACK
Server  → FIN
Browser → ACK
```

Then the connection is closed gracefully.

---

# 🔥 Must remember

```text
FIN = "I'm finished sending."

ACK = "I received that."

TCP graceful close:
FIN
ACK
FIN
ACK
```

Why four?

> **Because TCP is full-duplex, so each direction is shut down independently.**

And:

```text
Three-way handshake → establish connection
Four-way close      → gracefully terminate connection
```

---

## Module 11 Progress

✅ Transport Layer  
✅ Ports 🔌  
✅ TCP 🛡️  
✅ UDP ⚡  
✅ Three-way Handshake 🤝  
✅ **Four-way Close 🔚**  
⬜ Reliability  
⬜ Flow Control  

**Next → TCP Reliability 🛡️** — we'll put together **sequence numbers + ACKs + retransmissions + checksums + duplicate/out-of-order handling** and see how TCP actually manages to deliver data reliably over an unreliable IP network.