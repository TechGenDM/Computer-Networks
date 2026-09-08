# 3. Longest Prefix Match 🎯

This is one of the **most important concepts in IP routing**.

The basic rule is:

> **When multiple routes match a destination IP, the router chooses the route with the longest matching prefix.**

Let's make that very simple.

---

## 🧩 Example

Suppose a router has:

```text id="3nq5bd"
10.0.0.0/8
10.20.0.0/16
10.20.30.0/24
```

And receives a packet for:

```text id="7pxb7h"
10.20.30.40
```

Which routes match?

### `/8`

```text
10.x.x.x
```

✅ Matches.

### `/16`

```text
10.20.x.x
```

✅ Matches.

### `/24`

```text
10.20.30.x
```

✅ Matches.

So we have:

```text
/8
/16
/24
```

Which one is **most specific**?

👉 `/24`

Therefore the router chooses:

```text id="5m3bdy"
10.20.30.0/24
```

---

# 🧠 Why "longest"?

The `/24` prefix specifies **24 bits** of the destination.

The `/16` specifies only 16.

The `/8` specifies only 8.

So:

```text
/24 → more specific
/16 → less specific
/8  → very broad
```

Think:

> **More prefix bits = more specific route.**

---

# 🏠 Real-world analogy

Imagine you're looking for an address:

```text id="mup8dz"
India
 └── Karnataka
      └── Bengaluru
           └── Electronic City
                └── Building X
```

If someone tells you:

> "It's somewhere in India."

That's useful, but broad.

If they tell you:

> "Building X, Electronic City, Bengaluru."

That's much more specific.

Routing works similarly.

```text id="h9b0zt"
/8  → "Somewhere in 10.x.x.x"
/16 → "Somewhere in 10.20.x.x"
/24 → "Specifically 10.20.30.x"
```

The router chooses the **most specific information available**.

---

# 🔥 Another example

Routing table:

```text id="8b1a1u"
192.168.0.0/16    → Router A
192.168.1.0/24    → Router B
192.168.1.128/25  → Router C
0.0.0.0/0         → Router D
```

Destination:

```text id="xv2u8g"
192.168.1.150
```

It matches:

```text
192.168.0.0/16    ✅
192.168.1.0/24    ✅
192.168.1.128/25  ✅
0.0.0.0/0         ✅
```

Longest prefix:

```text
/25
```

Therefore:

> **Router C wins.**

---

# 🌟 What about `0.0.0.0/0`?

This is extremely important.

`0.0.0.0/0` matches **every IPv4 destination**.

Why?

Because `/0` means:

```text
0 network bits
32 host bits
```

There are no required prefix bits, so every IPv4 address matches it.

That's why it's commonly used as the:

> **Default route**

If no more specific route exists, the router can use the `/0` route.

We'll cover this properly in the next topic.

---

# 🎯 The rule you should memorize

When a packet arrives:

```text
Destination IP
      ↓
Find all matching routes
      ↓
Choose the route with the
LARGEST PREFIX LENGTH
      ↓
Forward packet
```

For example:

```text
/8
/16
/24
/25  ← 🏆 Longest prefix → chosen
```

### One-line memory trick:

> **Longest Prefix = Most Specific Route**

This concept is **fundamental to how IP routers make forwarding decisions**.