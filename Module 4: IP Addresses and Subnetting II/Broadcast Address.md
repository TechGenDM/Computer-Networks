# 5. Network Address vs Broadcast Address 📡

This is a **very important subnetting concept**.

Suppose we have:

```text
192.168.1.0/24
```

This network contains:

```text
192.168.1.0 → 192.168.1.255
```

But not every address is assigned to a normal host.

There are two special addresses:

* **Network Address**
* **Broadcast Address**

---

# ① Network Address

The **Network Address identifies the subnet itself**.

For:

```text
192.168.1.0/24
```

the network address is:

```text
192.168.1.0
```

It represents:

> "The entire 192.168.1.0/24 network."

It's not normally assigned to an individual host.

---

# ② Broadcast Address

The **Broadcast Address** is used to send a packet to **all hosts on that IPv4 subnet**.

For:

```text
192.168.1.0/24
```

the broadcast address is:

```text
192.168.1.255
```

Conceptually:

```text
        Broadcast
             ↓
        192.168.1.255
             │
      ┌──────┼──────┐
      ↓      ↓      ↓
    PC 1   PC 2   PC 3
```

Every host on that subnet can receive the broadcast.

---

# 🧠 Why `.0` and `.255`?

Look at the binary host portion.

For `/24`:

```text
Network bits              Host bits
11111111.11111111.11111111 | 00000000
```

### Network address

Set all host bits to **0**:

```text
00000000
```

Therefore:

```text
192.168.1.0
```

### Broadcast address

Set all host bits to **1**:

```text
11111111
```

Therefore:

```text
192.168.1.255
```

So:

> **Network = all host bits 0**

> **Broadcast = all host bits 1**

🔥 This rule works for IPv4 subnetting in general.

---

# Example: `/26`

Let's use something more interesting:

```text
192.168.1.0/26
```

We already know `/26` gives:

$$
2^6 = 64
$$

addresses per subnet.

Therefore:

```text
Network Address → 192.168.1.0
Broadcast        → 192.168.1.63
Usable Hosts     → 192.168.1.1 – 192.168.1.62
```

Next subnet:

```text
192.168.1.64/26
```

Here:

```text
Network Address → 192.168.1.64
Broadcast        → 192.168.1.127
Usable Hosts     → 192.168.1.65 – 192.168.1.126
```

And:

```text
192.168.1.128/26
```

becomes:

```text
Network Address → 192.168.1.128
Broadcast        → 192.168.1.191
Usable Hosts     → 192.168.1.129 – 192.168.1.190
```

---

# 🔥 General Pattern

For any normal IPv4 subnet:

```text
┌──────────────┬──────────────────┬───────────────┐
│ Network Addr │ Usable Host Addr │ Broadcast Addr│
└──────────────┴──────────────────┴───────────────┘
```

For example:

```text
192.168.1.64/26

Network    → .64
Hosts      → .65 – .126
Broadcast  → .127
```

---

## ⚠️ Don't memorize `.0` = Network and `.255` = Broadcast

That's only true for this particular `/24` example.

For `/26`:

```text
Network    → .0
Broadcast  → .63
```

For the next `/26`:

```text
Network    → .64
Broadcast  → .127
```

So the correct rule is:

> **Network address = all host bits 0**

> **Broadcast address = all host bits 1**

---

### 🎯 Quick mental model

```text
192.168.1.64/26

64  → Network
65
66
...
126 → Host
127 → Broadcast
```

That's the pattern you want to recognize instantly.

Next we'll finish the module with **Introduction to NAT (Network Address Translation)**, which connects directly to our earlier discussion of **private IPs, public IPs, and your home router**.
