# 6. CIDR Notation 🌐

Now we're going to replace long subnet masks like:

```text
255.255.255.0
```

with a much shorter notation:

```text
/24
```

This is called **CIDR**.

> **CIDR = Classless Inter-Domain Routing**

---

## What does `/24` mean?

Take:

```text
192.168.1.25/24
```

The `/24` means:

> **The first 24 bits belong to the network portion.**

IPv4 has 32 bits total:

```text
Network                 Host
<──────── 24 bits ────> <─8─>

11111111.11111111.11111111.00000000
```

Therefore:

```text
/24 = 24 network bits + 8 host bits
```

---

# 🔄 CIDR ↔ Subnet Mask

The most common ones:

|  CIDR | Subnet Mask       |
| ----: | ----------------- |
|  `/8` | `255.0.0.0`       |
| `/16` | `255.255.0.0`     |
| `/24` | `255.255.255.0`   |
| `/25` | `255.255.255.128` |
| `/26` | `255.255.255.192` |
| `/27` | `255.255.255.224` |
| `/28` | `255.255.255.240` |
| `/29` | `255.255.255.248` |
| `/30` | `255.255.255.252` |

The first three are particularly important initially.

---

# 🧮 How many hosts?

Here's the formula:

[
\text{Total addresses} = 2^{\text{host bits}}
]

Since IPv4 has 32 bits:

[
\text{Host bits} = 32 - \text{prefix length}
]

### Example: `/24`

```text
32 - 24 = 8 host bits
```

Therefore:

[
2^8 = 256
]

Total addresses = **256**

For a traditional IPv4 subnet:

```text
Network address → 1
Broadcast address → 1
```

So:

[
256 - 2 = 254
]

**254 usable host addresses.**

---

# Example: `/26`

```text
192.168.1.0/26
```

Host bits:

[
32 - 26 = 6
]

Total addresses:

[
2^6 = 64
]

Traditionally usable hosts:

[
64 - 2 = 62
]

So:

> **/26 → 64 total addresses → 62 usable host addresses**

---

# 🔥 Notice the pattern

As the prefix gets **larger**, the network gets **smaller**.

```text
/24 → 256 addresses
/25 → 128
/26 → 64
/27 → 32
/28 → 16
/29 → 8
/30 → 4
```

Why?

Because every additional network bit takes away one host bit.

---

## 🧠 Think of it like this

```text
/24

Network: ████████████████████████
Host:    ████████
```

8 host bits → lots of addresses.

But:

```text
/28

Network: ████████████████████████████
Host:    ████
```

Only 4 host bits → much smaller subnet.

---

# ⚠️ One important distinction

Don't confuse:

```text
192.168.1.0/24
```

with:

```text
192.168.1.25/24
```

The first commonly represents the **network itself**.

The second represents a **host address within that network**.

Both have the same `/24` prefix, meaning they belong to the same subnet:

```text
192.168.1.0/24
       │
       ├── 192.168.1.1
       ├── 192.168.1.2
       ├── ...
       └── 192.168.1.254
```

---

# 🎯 The formula you should remember

For an IPv4 prefix `/n`:

[
\boxed{\text{Host bits}=32-n}
]

[
\boxed{\text{Total addresses}=2^{32-n}}
]

For traditional subnetting:

[
\boxed{\text{Usable hosts}=2^{32-n}-2}
]

We'll later discuss exceptions such as `/31` and `/32`.

---

### Quick example

What does:

**`10.0.0.0/16`**

mean?

* Network bits = **16**
* Host bits = **16**
* Total addresses = (2^{16} = 65,536)
* Traditional usable hosts = **65,534**

And now we've reached the point where **subnetting itself** starts. That's where we'll take one large network and divide it into multiple smaller networks.
