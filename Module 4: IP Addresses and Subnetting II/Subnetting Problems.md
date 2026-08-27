Absolutely. We’re now entering **Module 4: IP Addresses and Subnetting II**.

This module is where the concepts from Module 3 become **practical**. We'll do subnetting problems carefully rather than just memorizing formulas.

### 🌐 Module 4 Roadmap

1. **Subnetting Problems** ⭐
2. **Variable Length Subnetting (VLSM)** *(Optional)*
3. **IPv6**
4. **Default Gateway**
5. **Broadcast Address**
6. **Network Address**
7. **Introduction to NAT**

Let's start with the most important one.

# 1. Subnetting Problems

## First: What is subnetting?

**Subnetting = dividing one larger IP network into multiple smaller networks.**

Suppose we have:

```text
192.168.1.0/24
```

A `/24` gives us:

```text
256 total addresses
254 traditional usable hosts
```

But suppose a company has different departments:

```text
Engineering → 100 devices
HR          → 50 devices
Marketing   → 50 devices
```

We don't necessarily want everyone in one giant network.

We can divide:

```text
192.168.1.0/24
        ↓
 ┌──────┼──────┐
 ↓      ↓      ↓
Subnet A  Subnet B  Subnet C
```

That's subnetting.

---

# 🧮 The basic idea

Remember:

```text
/24
```

means:

```text
24 Network bits | 8 Host bits
```

If we change it to:

```text
/26
```

we now have:

```text
26 Network bits | 6 Host bits
```

We **borrowed 2 bits from the host portion** to create more networks.

Those 2 bits give:

$$
2^2 = 4
$$

subnets.

And each subnet has:

$$
2^6 = 64
$$

total addresses.

So:

```text
/24
   ↓ subnet into /26

4 subnets
64 addresses each
62 traditional usable hosts each
```

---

# 🔥 Let's actually divide it

Original:

```text
192.168.1.0/24
```

Convert to `/26`.

The subnet size is:

$$
2^{32-26}=64
$$

So the subnet boundaries occur every **64** addresses.

Therefore:

### Subnet 1

```text
192.168.1.0/26
```

Range:

```text
192.168.1.0
        ↓
192.168.1.63
```

### Subnet 2

```text
192.168.1.64/26
```

Range:

```text
192.168.1.64
        ↓
192.168.1.127
```

### Subnet 3

```text
192.168.1.128/26
```

Range:

```text
192.168.1.128
        ↓
192.168.1.191
```

### Subnet 4

```text
192.168.1.192/26
```

Range:

```text
192.168.1.192
        ↓
192.168.1.255
```

So:

```text
192.168.1.0/24

├── 192.168.1.0/26
├── 192.168.1.64/26
├── 192.168.1.128/26
└── 192.168.1.192/26
```

---

# 🧠 Network & Broadcast

Each subnet has a **network address** and **broadcast address**.

For the first subnet:

```text
192.168.1.0/26
```

```text
Network address  → 192.168.1.0
Usable hosts     → 192.168.1.1 – 192.168.1.62
Broadcast        → 192.168.1.63
```

Second:

```text
Network address  → 192.168.1.64
Usable hosts     → 192.168.1.65 – 192.168.1.126
Broadcast        → 192.168.1.127
```

And so on.

---

# 🎯 The shortcut

For many subnetting questions, calculate:

### 1. Number of host bits

$$
32-\text{prefix}
$$

### 2. Addresses per subnet

$$
2^{\text{host bits}}
$$

### 3. Subnet increment

Usually:

$$
\text{Subnet increment} =
\text{addresses per subnet}
$$

For `/26`:

$$
32-26=6
$$

$$
2^6=64
$$

So the subnet boundaries are:

```text
0
64
128
192
```

---

## ⚠️ One important thing

Don't blindly memorize:

> `/26 = 64 addresses`

Instead understand **why**:

```text
IPv4 = 32 bits

/26 → 26 network bits
     → 6 host bits

2⁶ = 64
```

Once this clicks, you can calculate almost any subnet size.

---

### 🧪 Your turn

Given:

**`192.168.10.0/27`**

Tell me:

1. How many **host bits**?
2. How many **total addresses per subnet**?
3. How many **traditional usable host addresses**?
4. What are the **first four subnet network addresses**?

Take your time—this is the first proper subnetting problem.
