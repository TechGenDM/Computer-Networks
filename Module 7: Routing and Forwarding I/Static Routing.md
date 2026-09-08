# 4. Static Routing 🛣️

Now let's understand how an administrator can **manually tell a router where to send traffic**.

## What is Static Routing?

**Static routing = routes manually configured by a network administrator.**

Imagine this network:

```text id="n7x4kq"
Network A          Network B          Network C
192.168.1.0        10.0.0.0           172.16.0.0
     │                 │                    │
     ▼                 ▼                    ▼
    R1 ─────────────── R2 ─────────────── R3
```

Suppose R1 doesn't automatically know how to reach:

```text
172.16.0.0/16
```

The administrator can configure:

> **"To reach `172.16.0.0/16`, send the packet to R2."**

Conceptually:

```text id="gq6xpb"
R1 Routing Table

Destination       Next Hop
172.16.0.0/16  →  R2
```

That's a **static route**.

---

# 🧠 Why would we use static routes?

They're useful when the network is:

- Small
- Simple
- Stable
- Predictable

For example:

```text id="v1j7a3"
Office A ── Router ── Office B
```

There's only one sensible path.

You don't necessarily need a complex dynamic routing protocol.

---

# 🔄 Static vs Dynamic Routing

### Static Routing

Administrator manually configures:

```text id="p5r1j2"
Network X → Router Y
```

### Dynamic Routing

Routers communicate with each other and **learn routes automatically** using routing protocols.

Examples:

```text id="qj7y0b"
OSPF
BGP
RIP
```

So:

```text id="z5ezqg"
Static
   ↓
Human configures routes

Dynamic
   ↓
Routers exchange routing information
```

---

# 🔥 Example

Suppose:

```text id="9f8jcd"
       R1
      /  \
     /    \
    R2────R3
```

And R1 needs to reach the network behind R3.

An administrator might configure:

```text id="q0s8ep"
Destination: 10.10.0.0/16
Next Hop:    R3
```

Now whenever R1 receives:

```text id="zj3k7p"
Destination = 10.10.20.50
```

it finds:

```text id="0d5q6p"
10.10.0.0/16
```

and forwards the packet toward R3.

---

# ⚠️ The disadvantage

Static routing doesn't automatically adapt when the network changes.

Suppose:

```text id="2y0p9j"
R1 ─── R2 ─── R3
```

and the R2–R3 link fails:

```text id="y8m7c2"
R1 ─── R2    X    R3
```

A statically configured route may remain pointing toward the broken path until an administrator changes it or another mechanism detects/handles the failure.

Dynamic routing protocols are designed to **learn and adapt to topology changes**.

---

# 🎯 Remember

> **Static routing = manually configured route.**

> **Dynamic routing = routers learn routes through routing protocols.**

And notice how this connects to what we've already learned:

```text
Destination IP
      ↓
Routing Table
      ↓
Longest Prefix Match
      ↓
Best Route
      ↓
Next Hop
```