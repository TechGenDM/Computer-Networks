Absolutely. Let's go one level deeper.

# 5. Subnet Mask

We just learned that an IPv4 address has:

```text
Network Part | Host Part
```

But how does a computer know **where the network part ends and the host part begins?**

👉 **Subnet mask tells us.**

---

## What is a subnet mask?

A **subnet mask** is a 32-bit value used to identify which bits of an IPv4 address belong to the **network** and which belong to the **host**.

Example:

```text
IP Address:
192.168.1.25

Subnet Mask:
255.255.255.0
```

In binary:

```text
IP:
11000000.10101000.00000001.00011001

Mask:
11111111.11111111.11111111.00000000
```

The rule is simple:

> **1 in the mask → Network bit**

> **0 in the mask → Host bit**

So:

```text
11111111.11111111.11111111 | 00000000
          NETWORK           |   HOST
```

Therefore:

```text
Network = 192.168.1
Host    = 25
```

And the network address is:

```text
192.168.1.0
```

---

# 🧠 Why is it called a "mask"?

Because we can use it with the IP address to **mask out the host portion**.

For example:

```text
IP:
192.168.1.25

Mask:
255.255.255.0

Result:
192.168.1.0
```

This operation is actually performed using a **bitwise AND**.

In binary:

```text
IP:
11000000.10101000.00000001.00011001

AND

Mask:
11111111.11111111.11111111.00000000

=

11000000.10101000.00000001.00000000
```

Which is:

```text
192.168.1.0
```

🔥 This is how we mathematically determine the **network address**.

---

# Let's try another example

Suppose:

```text
IP:
172.16.45.100

Mask:
255.255.0.0
```

Binary mask:

```text
11111111.11111111.00000000.00000000
```

So:

```text
Network              Host
───────────────      ─────────────
172.16               45.100
```

Network address:

```text
172.16.0.0
```

---

# Another example

```text
IP:
10.20.30.40

Mask:
255.0.0.0
```

Binary:

```text
11111111.00000000.00000000.00000000
```

Therefore:

```text
Network       Host
──────        ────────────────
10            20.30.40
```

Network address:

```text
10.0.0.0
```

---

# 🔥 Important pattern

You'll often see masks like:

```text
255.0.0.0
255.255.0.0
255.255.255.0
```

These correspond to:

```text
255.0.0.0       → /8
255.255.0.0     → /16
255.255.255.0   → /24
```

And this leads directly to **CIDR notation**, which is much more commonly used today.

---

## 🎯 Remember

The subnet mask answers:

> **"Which part of this IP address represents the network?"**

For:

```text
192.168.1.25
255.255.255.0
```

we have:

```text
192.168.1 | 25
 NETWORK  | HOST
```

Next: **CIDR Notation (`/24`, `/16`, `/8`, etc.)**.

Once you understand CIDR, you'll be able to look at something like `192.168.1.0/24` and immediately understand what it means.
