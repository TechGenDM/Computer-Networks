# 2. Routing Table 🗺️

Now let's look at the **router's decision-making map**.

A **routing table** is a collection of routes that tells a router:

> **"For a destination network, where should I send the packet next?"**

---

## 📋 A simple routing table

Imagine Router R1 has:

| Destination Network | Next Hop | Interface |
|---|---|---|
| `192.168.1.0/24` | Directly connected | `eth0` |
| `10.0.0.0/8` | `192.168.1.1` | `eth1` |
| `172.16.0.0/16` | `192.168.1.2` | `eth2` |
| `0.0.0.0/0` | `192.168.1.254` | `eth3` |

Don't worry about every column yet. The important information is:

```text id="x7g0rj"
Destination → Where the network is
Next Hop    → Where to send it next
Interface   → Which router interface to use
```

---

# 📦 Example

Suppose a packet arrives:

```text id="0zv52m"
Destination IP = 10.20.30.40
```

The router checks its routing table.

It finds:

```text id="o0v6dj"
10.0.0.0/8 → next hop 192.168.1.1
```

So the router essentially decides:

> "This destination belongs to `10.0.0.0/8`; I'll forward it toward `192.168.1.1`."

---

# 🧠 What does "Directly Connected" mean?

Suppose Router R1 has:

```text id="p4k8lq"
Interface:
192.168.1.1/24
```

That means R1 knows:

```text id="8p9gq1"
192.168.1.0/24
```

is directly connected.

If the destination is:

```text id="e9b4p4"
192.168.1.50
```

R1 doesn't need another router as the next hop.

It can deliver the packet directly onto that local network.

```text id="q3x1vz"
R1 ─────────► Host
     Direct
```

---

# 🔥 Routing tables aren't necessarily manually created

Routes can come from different sources.

### 1. Connected routes

Automatically learned from interfaces.

```text id="3o3z4j"
192.168.1.0/24 → Connected
```

### 2. Static routes

An administrator manually configures them.

```text id="3j8zq7"
10.0.0.0/8 → via 192.168.1.2
```

### 3. Dynamic routing protocols

Routers can exchange routing information using protocols such as:

- OSPF
- BGP
- RIP

We'll study these later.

---

# 🚨 One extremely important concept

A routing table doesn't necessarily contain an entry for **every individual IP address**.

It can contain **network prefixes**.

For example:

```text id="x0v9gn"
10.0.0.0/8
```

represents a huge range of addresses.

That's one of the reasons **CIDR and hierarchical IP addressing** are so powerful.

---

# 🧩 How a router uses the table

The process is roughly:

```text id="z1a7tp"
Packet arrives
      ↓
Read destination IP
      ↓
Look through routing table
      ↓
Find matching route(s)
      ↓
Choose best match
      ↓
Determine next hop/interface
      ↓
Forward packet
```

And that **"choose the best match"** step is incredibly important.

What if multiple routes match the same destination?

For example:

```text id="x5y5aq"
10.0.0.0/8
10.20.0.0/16
10.20.30.0/24
```

A destination like:

```text
10.20.30.40
```

matches **all three**.

Which one does the router choose?

👉 **Longest Prefix Match.**

That's our next topic.