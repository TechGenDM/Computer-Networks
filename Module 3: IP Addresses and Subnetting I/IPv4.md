# 2. IPv4 🌐

Now let's understand **IPv4**, the most commonly encountered IP addressing system.

## What is IPv4?

**IPv4 = Internet Protocol version 4.**

An IPv4 address is **32 bits** long.

Those 32 bits are divided into **4 groups of 8 bits**, called **octets**.

Example:

```text
192.168.1.25
```

Structure:

```text
192     .168     .1       .25
 ↓       ↓       ↓        ↓
8 bits  8 bits  8 bits   8 bits

       = 32 bits
```

---

## 🔢 Why numbers from 0 to 255?

Each octet contains **8 bits**.

An 8-bit number can represent:

```text
00000000 → 0
11111111 → 255
```

So every IPv4 octet ranges from:

> **0 → 255**

That's why an IPv4 address looks like:

```text
0.0.0.0
```

through:

```text
255.255.255.255
```

Not every address in that range is usable as an ordinary host address, though.

---

# 🧠 Example

Take:

```text
192.168.1.25
```

In binary:

```text
192 → 11000000
168 → 10101000
  1 → 00000001
 25 → 00011001
```

So:

```text
192.168.1.25

= 11000000.10101000.00000001.00011001
```

That's the actual **32-bit IPv4 address**.

---

# 📦 Why use dotted decimal?

Technically, computers work with:

```text
11000000101010000000000100011001
```

But humans don't want to read that all day 😄.

So we represent every 8 bits as a decimal number:

```text
11000000 . 10101000 . 00000001 . 00011001
   192   .   168    .    1     .    25
```

Much easier.

---

# ⚠️ Very important: IPv4 address ≠ automatically "host address"

Here's something we'll soon explore deeply.

An IPv4 address has two conceptual parts:

```text
┌───────────────────────┬──────────────┐
│      Network Part     │  Host Part   │
└───────────────────────┴──────────────┘
```

For example, depending on the subnet mask:

```text
192.168.1.25
```

could conceptually mean:

```text
Network: 192.168.1.0
Host:    25
```

But **you cannot determine the network/host split from the IP address alone**.

You also need the **subnet mask / prefix length**.

That's going to be the next important concept.

---

## 🎯 Remember

> **IPv4 = 32-bit address**

> **4 octets × 8 bits = 32 bits**

> **Each octet = 0–255**

Example:

```text
192.168.1.25
↓
11000000.10101000.00000001.00011001
```

Next we'll do the **Binary Refresher**. Don't skip this one—we'll use binary heavily when we start **subnetting**, so getting comfortable with it now will make the later topics much easier.
