# 6. Packet Forwarding 📦➡️

Now let's see what a **router actually does when a packet arrives**.

Remember:

> **Routing = deciding the path.**  
> **Forwarding = actually moving the packet to the next hop.**

---

## 🌐 Example Network

```text
Laptop
  │
  ▼
 R1 ───── R2 ───── R3
                  │
                  ▼
                Server
```

Your laptop sends a packet destined for the server.

The packet arrives at **R1**.

---

## Step 1 — Receive the Frame

R1 receives a Layer-2 frame from the previous link.

Conceptually:

```text
┌────────────────────────────────┐
│ Ethernet Header                │
│ IP Packet                      │
│ TCP Segment + Data             │
└────────────────────────────────┘
```

R1 processes the incoming frame.

---

## Step 2 — Examine Destination IP

R1 looks at the **destination IP address inside the IP packet**.

For example:

```text
Destination IP = 10.20.30.40
```

The router doesn't care about the application data to make this basic forwarding decision.

It primarily needs the **destination IP**.

---

## Step 3 — Check Routing Table

R1 searches its routing information:

```text id="0zj8ju"
Destination          Next Hop
10.20.30.0/24    →   R2
0.0.0.0/0         →   ISP
```

`10.20.30.40` matches:

```text
10.20.30.0/24
```

So R1 chooses:

```text
Next Hop → R2
```

---

## Step 4 — Longest Prefix Match

If several routes match, R1 uses the **most specific one**.

For example:

```text id="9xj5m3"
10.0.0.0/8
10.20.0.0/16
10.20.30.0/24  ← chosen
```

This is the **Longest Prefix Match** rule we just learned.

---

## Step 5 — Determine Outgoing Interface

R1 determines:

> "Which interface should I use to reach R2?"

For example:

```text id="cbt0k0"
R1
├── eth0 → Laptop network
└── eth1 → R2
```

So it selects:

```text
eth1
```

---

## Step 6 — Create a New Layer-2 Frame

This is a **very important detail**.

The IP packet is carried across the **next link** using a new Layer-2 frame.

Conceptually:

```text id="4f2v8m"
Incoming:

[MAC Laptop → MAC R1 | IP | TCP | Data]

              R1
              ↓

Outgoing:

[MAC R1 → MAC R2 | IP | TCP | Data]
```

Notice:

### MAC addresses change.

But the IP destination remains the same for the routed journey, aside from fields that routers may modify.

---

## Step 7 — Forward the Packet

R1 transmits the new frame toward R2.

```text id="9a5n6j"
R1
 │
 │ New Frame
 ▼
R2
```

R2 then repeats the process:

```text
Receive
   ↓
Look at destination IP
   ↓
Routing table
   ↓
Longest prefix match
   ↓
Choose interface/next hop
   ↓
New Layer-2 frame
   ↓
Forward
```

And eventually:

```text id="7p0d7e"
R1 → R2 → R3 → Server
```

---

# 🔥 One subtle but important point

A router doesn't normally **recalculate the entire route from scratch for every packet**.

The router already has forwarding/routing information.

For each packet, it essentially performs:

```text id="q0p7j2"
Destination IP
      ↓
Forwarding table lookup
      ↓
Best matching prefix
      ↓
Next hop + outgoing interface
      ↓
Forward
```

This needs to happen **extremely quickly**, because routers may process huge numbers of packets every second.

---

# 🧠 Routing vs Forwarding — Final distinction

### Routing

```text
"Which route should I use?"
```

Uses routing information/protocols to determine routes.

### Forwarding

```text
"This packet has arrived.
Where do I send it next?"
```

Uses the forwarding table to make the immediate forwarding decision.

---

## 🎯 Complete mental model

```text
                 ROUTER

Packet arrives
      ↓
Read Destination IP
      ↓
Lookup forwarding table
      ↓
Longest Prefix Match
      ↓
Next Hop + Interface
      ↓
Build new Layer-2 frame
      ↓
Send to next hop
```