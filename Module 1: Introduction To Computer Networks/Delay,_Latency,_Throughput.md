# 6. Network Delay

## Why Isn't the Network Instantaneous?

When your laptop sends a packet:

```
Laptop ───────────────► Server
```

The packet doesn't arrive instantly.

The **total time spent** is called **end-to-end delay**.

---

## The Formula for End-to-End Delay

$$d_{end-to-end} = d_{processing} + d_{queuing} + d_{transmission} + d_{propagation}$$

There are **4 types of delay**, and understanding each is critical.

---

## 🧠 Easy Memory Aid: **P-Q-T-P**

| Delay | Question | Answer |
|-------|----------|--------|
| **Processing** | "What should I do?" | Examine & decide |
| **Queuing** | "How long do I wait?" | Time in queue |
| **Transmission** | "How long to push bits?" | Packet size ÷ Link speed |
| **Propagation** | "How long to travel?" | Distance ÷ Speed of signal |

---

## ① Processing Delay

### What is it?

When a packet reaches a router, the router needs to **examine it**.

```
Packet
  ↓
Router receives
  ↓
Check destination IP
  ↓
Determine next hop
  ↓
Update TTL
  ↓
Recalculate checksum
  ↓
Forward decision
```

The **time required for all this** is **processing delay**.

### How much?

Usually **very small** (microseconds to milliseconds).

Modern routers are optimized for this.

### Think:

> "Let me inspect this packet and decide what to do with it."

---

## ② Queuing Delay

### What is it?

Suppose packets **arrive faster than the outgoing link can transmit** them.

They **wait in a queue**.

```
       Router
         │
Packets →│→→→→  (incoming, multiple packets/second)
 ↓ ↓ ↓ ↓ │
┌─────────────────┐
│ Output Queue    │
│ █████████████   │  ← Packets waiting!
└──────┬──────────┘
       ↓
  Outgoing Link (slower)
```

The **time a packet waits before transmission** is **queuing delay**.

### Key Insight:

**Queuing delay depends on network congestion.**

```
Low traffic scenario:
Queue: ░         (nearly empty)
Delay: Very low

High traffic scenario:
Queue: ██████████  (full)
Delay: Very high

Overload scenario:
Queue: ████████████ (overflow!)
Result: Packet LOSS
```

### Real-World Impact:

- During peak hours → high queuing delay
- During off-peak → low queuing delay
- This is why Internet speeds vary throughout the day!

### Think:

> "How many packets are ahead of me in the queue?"

---

## ③ Transmission Delay

### What is it?

This is often **misunderstood**.

Transmission delay is the time required to **push ALL the packet's bits onto the link**.

It's **NOT** about how far the bits travel. That's propagation delay.

### Formula:

$$d_{transmission} = \frac{L}{R}$$

Where:
- **L** = packet size in **bits**
- **R** = link transmission rate in **bits/second**

### Example 1:

```
Packet size = 1,000 bits
Link speed = 1,000,000 bits/second

d_transmission = 1000 / 1,000,000 = 0.001 seconds = 1 millisecond
```

So it takes **1 ms** to push all the bits onto the link.

### Example 2:

```
Packet size = 1,500 bits (typical Ethernet)
Link speed = 100 Mbps = 100,000,000 bits/sec

d_transmission = 1500 / 100,000,000 = 0.000015 sec = 15 microseconds
```

### Physical Analogy:

Imagine a **long pipe** and you're pushing water into it.

**Transmission delay** = How long does it take to **push ALL the water into the pipe?**

```
┌──────────────────────┐
│ Pipe (empty)         │
│ ├──────────────────┤ │  ← Transmission delay
│ │ Water being      │ │
│ │ pushed in        │ │
│ └──────────────────┘ │
└──────────────────────┘
     ↑
  Start pushing
```

### Think:

> "How long does it take to push all bits of this packet onto the link?"

---

## ④ Propagation Delay

### What is it?

Once the bits are **pushed onto the physical link**, they need to **physically travel** through it.

That travel takes time.

### Formula:

$$d_{propagation} = \frac{distance}{propagation\_speed}$$

### Example:

```
Distance from India to USA = ~12,000 km
Propagation speed (fiber optic) = ~200,000 km/sec

d_propagation = 12,000 / 200,000 = 0.06 seconds = 60 milliseconds
```

So bits take about **60 ms** to physically travel from India to USA.

### The Signal Travels Through:

- Fiber optic cables (~2/3 speed of light)
- Copper wires (~2/3 speed of light)
- Wireless medium (≈ speed of light)

### Physical Analogy:

Using the same **pipe analogy**:

**Propagation delay** = Once inside, how long does water **travel to the other end?**

```
Sender                          Receiver
  │                                │
  │─────── Physical Pipe ─────────│
  │                                │
  ├─ Transmission ─┤              │
  │                ├── Propagation ──►
```

### Think:

> "How long do the bits physically travel through the medium?"

---

## 🚨 Transmission vs Propagation: THE CRITICAL DIFFERENCE

This distinction is **VERY IMPORTANT** and often confused.

### Summary:

| Aspect | Transmission | Propagation |
|--------|--------------|------------|
| **What** | Pushing bits onto link | Bits physically traveling |
| **Formula** | L / R | Distance / Speed |
| **Depends on** | Packet size & link speed | Distance & signal speed |
| **Example** | 1 ms for 1000-bit packet on 1 Mbps link | 60 ms for 12,000 km via fiber |
| **Controllable** | Yes (vary packet size/link speed) | No (fixed by physics) |

### Real numbers:

```
Transmission delay:
- Small packets on fast links: microseconds
- Large packets on slow links: milliseconds

Propagation delay:
- Local network: nanoseconds  
- City-wide: microseconds
- Intercontinental: milliseconds
```

---

## 🔥 The Complete End-to-End Delay

Your packet travels through multiple routers:

```
Laptop ─────► Router1 ─────► Router2 ─────► Router3 ─────► Server
      (delay)         (delay)         (delay)         (delay)

At EACH hop:
- Processing delay (examine packet)
- Queuing delay (wait in queue)
- Transmission delay (push bits on link)
- Propagation delay (travel through link)
```

**Total delay** = Sum of all delays at all hops.

### Example calculation:

```
Laptop → Router (local LAN):
- Processing: 0.1 ms
- Queuing: 0.5 ms  
- Transmission: 0.01 ms
- Propagation: 0.001 ms
Subtotal: 0.611 ms

Router → ISP Router:
- Processing: 0.1 ms
- Queuing: 2 ms (more congested)
- Transmission: 0.05 ms
- Propagation: 1 ms
Subtotal: 3.15 ms

... (more hops) ...

Total: ~50-100 ms for intercontinental
```

---

## ⚠️ Important Insight

**Latency ≠ Bandwidth**

A connection can have:

```
✅ High bandwidth + High latency
   (Fast transfer rate, but slow to start)

✅ Low bandwidth + Low latency  
   (Slow transfer rate, but quick to start)

❌ We want low latency + high bandwidth
   (But can't control distance/physics for latency)
```

**Real-world example:**
- Downloading a file: Bandwidth matters more
- Playing online game: Latency matters more
- Video call: Both matter!

---

## 🎯 Key Takeaways

1. **Processing delay** = Router examines packet (tiny)
2. **Queuing delay** = Packet waits in queue (varies with congestion)
3. **Transmission delay** = Pushing bits onto link (depends on packet size & link speed)
4. **Propagation delay** = Bits physically travel (fixed by physics & distance)
5. **Total delay** = Sum of all four
6. **You can't eliminate propagation delay** (limited by speed of light)
7. **Network congestion increases queuing delay** (major impact on real-world delays)

---

## 💡 Practical Implications

Why is your Internet slower during peak hours?
- ❌ Not transmission delay (that's fixed)
- ✅ **Queuing delay increases** due to network congestion

Why is a local file transfer faster than downloading from overseas?
- ✅ Lower **propagation delay** (shorter distance)
- ✅ Lower **queuing delay** (less network hops)

Why is video streaming buffered?
- ❌ Not individual packet delay (milliseconds)
- ✅ **Queuing delays** when bandwidth is insufficient

---

**Next:** 🏎️ **Bandwidth, Throughput & Bottleneck**

Now that we understand delay, we'll tackle three related but distinct concepts that are constantly confused:

- **Bandwidth** = Maximum capacity
- **Throughput** = Actual performance  
- **Bottleneck** = Limiting factor
