# 5. Packet Switching vs Circuit Switching

## Why Do We Need Switching?

Imagine **1,000 people want to communicate** with different people.

**Problem:** You don't want a dedicated physical connection between every pair.

**Solution:** The network needs to **share its infrastructure intelligently**.

There are **two major approaches**:

---

## 📞 1. Circuit Switching

**Concept:** Imagine an **old-fashioned telephone system**.

Before communication starts, the network **establishes a dedicated path** between you and the other person.

```
You
 │
 ▼
Router A
 │
 ▼
Router B
 │
 ▼
Router C
 │
 ▼
Friend
```

### Key Characteristics:

✅ **Dedicated path** reserved for your conversation  
✅ **Fixed bandwidth** allocated to you  
✅ Path stays open even if you're silent  
❌ **Inefficient** for bursty traffic  

### Analogy:

> "Book a dedicated road lane before starting the journey"

```
You ═══════════════════► Destination
        RESERVED LANE
        (nobody else can use)
```

### Example:

Traditional **telephone networks** historically used circuit switching.

Even if you're not talking, the connection holds the reserved capacity.

---

## 📦 2. Packet Switching

**Concept:** The **modern Internet** primarily uses packet switching.

Instead of reserving an entire path, your **data is divided into small pieces called packets**.

### How it works:

Suppose you want to send:
```
HELLOWORLD
```

It becomes:
```
Packet 1 → HEL
Packet 2 → LOW
Packet 3 → ORL
Packet 4 → D
```

Each packet contains information that helps the network deliver it:
- Destination IP address
- Packet number (for reassembly)
- Data payload
- Error checking info

The **packets travel through the network** and are **eventually reassembled**.

```
        Packet 1 ───────┐
You →   Packet 2 ───────┼──→ Destination
        Packet 3 ───────┤
        Packet 4 ───────┘
```

### Analogy:

Imagine a **highway where multiple users share lanes**:

```
You ──┐
A ────┼──► Shared Network ──► Destination
B ────┤
C ────┘
```

Your data gets broken into packets and the network forwards them along with everyone else's packets.

**Different packets can even take different routes!**

---

## 🔥 Why Does the Internet Use Packet Switching?

### The problem with circuit switching:

Internet traffic is **bursty** (not continuous).

### Example browsing pattern:

```
Request (small data)
   ↓
[Waiting...]  ← Reserved capacity unused!
   ↓
Response (large data)
   ↓
[Waiting...]  ← Reserved capacity unused!
```

**Issue:** If we reserved a dedicated connection, much capacity sits idle.

### Solution:

**Packet switching** allows other users to use that idle capacity.

---

## ⚡ Statistical Multiplexing

This is an **important term**.

**Definition:**
> Multiple users **share the same network resources dynamically**.

### Example:

```
User A ──┐
User B ──┤
User C ──┼──► Shared Link (1 Gbps)
User D ──┤
User E ──┘
```

**When User A sends packets:**
- User A's packets use the link

**When User A is idle:**
- Other users' packets use the link

**Result:** Much higher utilization than circuit switching!

---

## ⚠️ But Packet Switching Has a Problem

Because everyone **shares the network**, sometimes **too many packets arrive at once**.

### Example:

```
Link capacity = 100 packets/second
Arriving traffic = 200 packets/second
```

The router can't transmit everything immediately.

### What happens:

```
       Router
         │
Packets →│→→→→  (incoming)
 ↓ ↓ ↓ ↓ │
┌─────────────┐
│ Queue █████ │  ← Packets wait here!
└──────┬──────┘
       ↓
    Network
```

**Consequences:**
- **Queuing delay** increases
- **Packet loss** can occur if queue is full
- **Congestion** happens
- **Throughput** is reduced

**This is a fundamental trade-off:**

```
Packet Switching Benefits:
✅ High utilization
✅ Efficient for bursty traffic
✅ Supports many users

Packet Switching Drawbacks:
❌ Variable delay
❌ Possible packet loss
❌ Congestion during peaks
```

---

## 🔄 Circuit vs Packet Switching Comparison

| Aspect | Circuit Switching | Packet Switching |
|--------|-------------------|------------------|
| **Connection** | Dedicated | Shared |
| **Resources** | Reserved upfront | Dynamically allocated |
| **Data format** | Continuous stream | Packets/datagrams |
| **Utilization** | Lower (for bursty traffic) | Higher |
| **Delay** | Predictable | Variable |
| **Reliability** | Guaranteed capacity | Best-effort |
| **Cost** | Higher (reserved) | Lower (shared) |
| **Use case** | Real-time (voice) | Data (Internet) |
| **Internet** | ❌ Not primary model | ✅ Primary model |

---

## 🧠 Simple Memory Aid

### Circuit Switching:
> "Reserve first, communicate later"

Like **booking a movie ticket** in advance.

### Packet Switching:
> "Break data into packets and share the network"

Like **sharing a highway** where everyone drives as needed.

---

## 🎯 Important Corrections

**❌ Don't think:**
> "Each packet takes a completely different route"

**✅ Do think:**
> The network does **NOT** reserve an exclusive end-to-end path for your communication.

Packets can take different paths depending on:
- Network conditions
- Routing decisions
- Congestion  
- Link failures

But they might also follow the same path if that's optimal.

---

## Real-World Impact

### When you browse the Web:

```
Browser
   ↓ (Sends HTTP request packet)
Internet (Packet switching in action!)
   ↓ (Packet travels through multiple routers)
Router → Router → Router → Server
   ↓ (Server sends response packets)
Internet (Packet switching again!)
   ↓ (Response travels back through network)
Browser (Receives and reassembles packets)
   ↓
Display webpage
```

**All of this relies on packet switching.**

Without it, the Internet would be **far less efficient** and **far more expensive**.

---

## 🎯 Key Takeaways

1. **Circuit switching** = Dedicated path (old phones)
2. **Packet switching** = Shared infrastructure (modern Internet)
3. **Packets** = Small data units with destination info
4. **Statistical multiplexing** = Dynamic resource sharing
5. **Trade-off** = Higher utilization but variable delay/congestion

---

**Next:** 🚦 **Network Delay**

Now that we understand packets, we'll explore a **very practical question**:

> *If I send a packet from India to a server in the US, why doesn't it arrive instantly?*

We'll break network delay into **four types**: processing, queuing, transmission, and propagation delay.
