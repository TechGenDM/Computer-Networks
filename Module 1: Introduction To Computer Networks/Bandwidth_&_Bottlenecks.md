# 7. Bandwidth, Throughput & Bottleneck

These three are **very important** and often confused, even among experienced developers.

Let's clarify each one.

---

## ① Bandwidth

### Definition:

**Bandwidth = Maximum data-carrying capacity of a link.**

Suppose your Internet connection is advertised as:

**100 Mbps**

That means the link can theoretically transmit up to:

> **100 million bits per second**

### Analogy:

Think of bandwidth as the **width of a highway**:

```
Small bandwidth (narrow highway):
🚗 🚗 🚗

Large bandwidth (wide highway):
🚗 🚗 🚗 🚗 🚗 🚗 🚗 🚗
```

**More bandwidth → more data can potentially be transferred simultaneously**

### Key Point:

> **Bandwidth is a PROPERTY of the link, not current performance.**

It's like the speed limit on a highway. The limit doesn't change, but actual traffic flow does.

---

## ② Throughput

### Definition:

**Throughput = The actual rate at which data is successfully delivered.**

### Example:

Suppose:
```
Bandwidth = 100 Mbps (what the ISP advertises)
Actual transfer = 72 Mbps (what you actually achieve)
```

Then:
> **Throughput = 72 Mbps**

### Why isn't throughput always equal to bandwidth?

Because of factors like:

- **Network congestion** (too many packets)
- **Protocol overhead** (headers, checksums, etc.)
- **Packet loss** (some packets don't make it)
- **Server limitations** (server can't send faster)
- **Wi-Fi interference** (wireless issues)
- **Other users sharing** the connection

### Key Point:

```
Bandwidth = Maximum CAPACITY (theoretical)
Throughput = Actual PERFORMANCE (measured)

Throughput ≤ Bandwidth (always)
```

### Real-World Example:

Your ISP advertises: **1 Gbps**

But in practice, you measure:
- **Best case** (off-peak): 900 Mbps
- **Typical case** (regular hours): 700 Mbps
- **Peak hours** (evening): 400 Mbps

The bandwidth stays at **1 Gbps**, but **throughput varies**.

---

## ③ Bottleneck

### Definition:

A **bottleneck** is the **slowest/limiting part** of a communication path.

### Example:

```
Your Laptop
    │
    │ 1 Gbps (fast)
    ▼
 Router
    │
    │ 1 Gbps (fast)
    ▼
 ISP
    │
    │ 100 Mbps  ← 🚨 BOTTLENECK!
    ▼
 Server
    │
    │ 10 Gbps (very fast)
    ▼
```

Even though some links support **1 Gbps**, the **100 Mbps link limits** the overall flow.

**Maximum throughput = 100 Mbps** (limited by bottleneck)

### Water Pipe Analogy:

```
████████████████  ← Wide pipe (1 Gbps)
        ↓
████              ← Narrow pipe (100 Mbps) ← BOTTLENECK
        ↓
████████████████  ← Wide pipe (10 Gbps)
```

The narrow pipe limits the flow, even if the other pipes are wide.

### Key Point:

> **Chain is only as strong as its weakest link**

---

## 🔥 Putting All Three Together

### Complete Example:

```
Laptop → Router → ISP → Server

Links:
Laptop → Router = 1 Gbps
Router → ISP    = 500 Mbps
ISP → Server    = 100 Mbps  ← BOTTLENECK
```

### Analysis:

- **Bandwidth** (max capacity): 1 Gbps? 500 Mbps? 100 Mbps?
  - Answer: **100 Mbps** (bottleneck!)

- **Maximum Throughput** (what you could achieve):
  - Answer: **100 Mbps** (limited by bottleneck)

- **Actual Throughput** (with congestion/overhead):
  - Answer: **70-90 Mbps** (even less than bottleneck bandwidth)

---

## ⚠️ Common Misconceptions

### ❌ DON'T say:

> "Bandwidth = Internet speed"

### ✅ DO say:

> "Bandwidth is the **maximum capacity**. Your observed speed is the **actual throughput**, which is usually lower due to various factors."

### Example:

```
ISP advertises: 100 Mbps
Actual speeds:  60-85 Mbps
Why the difference?
- Protocol overhead
- Network congestion  
- Distance from server
- Link quality
```

---

## 💡 Practical Scenarios

### Scenario 1: Downloading a Large File

```
ISP bandwidth = 100 Mbps
Your throughput = 80 Mbps
```

**Question:** Why not 100 Mbps?

**Answer:** 
- Protocol overhead (~5-10%)
- Network congestion
- Server not sending at full speed
- TCP slow-start algorithm

---

### Scenario 2: Video Streaming

```
Video quality: 4K (25 Mbps required)
Your connection: 100 Mbps bandwidth
Actual throughput: 40 Mbps
```

**Question:** Why is it buffering?

**Answer:**
- Even though bandwidth is 100 Mbps
- Actual throughput dropped to 40 Mbps
- Not enough to stream 4K continuously
- Need to buffer or reduce quality

---

### Scenario 3: Multiple Users on One ISP

```
ISP bandwidth: 100 Mbps (shared among all users)

User A: 30 Mbps
User B: 35 Mbps
User C: 20 Mbps
User D: 15 Mbps
Total: 100 Mbps

If User E joins (need 10 Mbps):
Everyone's throughput drops!
```

The **100 Mbps bandwidth** is shared, so individual throughput decreases.

---

## 🧠 One More Concept: Goodput

You may encounter this term later:

> **Goodput = Useful application data successfully delivered per unit time**

Some transmitted bits are:
- Protocol overhead (headers, checksums)
- Retransmissions (if packets are lost)

So conceptually:

```
Bandwidth
    ↓ (max capacity)
Throughput
    ↓ (actual achieved)
Goodput
    ↓ (useful data only)
```

**Relationship:**
```
Goodput ≤ Throughput ≤ Bandwidth
```

### Example:

```
Bandwidth: 100 Mbps (link capacity)
Throughput: 80 Mbps (actual measured)
Goodput: 75 Mbps (useful application data after overhead)
```

---

## 🎯 Quick Mental Model

| Concept | Think of it as | Example |
|---------|---|---|
| **Bandwidth** | **Speed limit** on a highway | 100 Mbps |
| **Throughput** | **Actual traffic flow** on that highway | 72 Mbps (due to congestion) |
| **Bottleneck** | **Narrowest section** limiting flow | A 50 Mbps section in the route |
| **Goodput** | **Useful cargo** delivered (not overhead) | 68 Mbps (after protocol overhead) |

---

## 🚗 Complete Highway Analogy

```
Speed limit (Bandwidth): 100 km/h

Road 1: 100 km/h
Road 2: 100 km/h
Road 3: 50 km/h  ← BOTTLENECK (reduced speed limit)
Road 4: 100 km/h

Actual traffic flow (Throughput): Limited to 50 km/h
Useful cargo delivery (Goodput): Slightly less due to empty vehicles, tolls, etc.
```

---

## Real-World Impact

### For Web Developers:
- Understanding bandwidth helps optimize asset delivery
- Understanding throughput helps manage user expectations  
- Identifying bottlenecks helps optimize your stack

### For Backend Engineers:
- Bandwidth affects how much data you can transmit
- Throughput affects actual performance users experience
- Bottlenecks can be application code, network, database, etc.

### For DevOps/Cloud:
- CDNs improve throughput by reducing distance (lower latency, more bandwidth)
- Load balancing removes bottlenecks
- Auto-scaling helps handle bandwidth increases

---

## 🎯 Key Takeaways

1. **Bandwidth** = Maximum capacity (property of the link)
2. **Throughput** = Actual measured performance (varies)
3. **Bottleneck** = The slowest part limiting overall throughput
4. **Goodput** = Useful application data (less than throughput due to overhead)
5. **They're all different** but related
6. **Throughput is what users care about**, not bandwidth

---

**Next:** We'll examine the **Network Devices** (Hub, Switch, Router, Modem) that implement these concepts at the hardware level.
