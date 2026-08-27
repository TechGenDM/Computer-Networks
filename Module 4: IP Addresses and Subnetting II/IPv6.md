# 3. IPv6 🌐

Now we move from **IPv4** to **IPv6**.

The main reason IPv6 was created is simple:

> **IPv4 doesn't have enough addresses for the modern Internet.**

---

## 1. IPv4's limitation

IPv4 uses **32 bits**.

Therefore:

$$
2^{32} = 4,294,967,296
$$

That's about **4.3 billion addresses**.

Sounds huge, right?

But the Internet has billions of devices, and addresses also need to be allocated for networks and infrastructure.

So we needed a much larger address space.

---

# 2. IPv6

IPv6 uses **128 bits**.

Therefore:

$$
2^{128}
$$

possible addresses.

That's an unimaginably large number.

The important point is:

> **IPv6 provides a vastly larger address space than IPv4.**

---

# 3. What does an IPv6 address look like?

IPv4:

```text
192.168.1.25
```

IPv6 looks like:

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

It's written using:

* Hexadecimal
* 8 groups
* Each group = 16 bits

So:

```text
8 × 16 = 128 bits
```

---

# 🔢 Why hexadecimal?

Remember binary:

```text
1010
```

is difficult to read.

Hexadecimal represents 4 binary bits using one character.

For example:

```text
1010 → A
1111 → F
```

So IPv6 can represent 128 bits much more compactly.

---

# ✂️ IPv6 Address Shortening

IPv6 addresses can often be shortened.

Suppose:

```text
2001:0db8:0000:0000:0000:0000:0000:0001
```

Leading zeros in each group can be removed:

```text
2001:db8:0:0:0:0:0:1
```

And consecutive groups of zeros can be replaced by `::`:

```text
2001:db8::1
```

So:

```text
2001:0db8:0000:0000:0000:0000:0000:0001
```

becomes:

```text
2001:db8::1
```

### ⚠️ Important rule

`::` can represent consecutive zero groups **only once** in an IPv6 address.

Otherwise the exact number of omitted groups would be ambiguous.

---

# 🔥 IPv4 vs IPv6

|                | IPv4           | IPv6          |
| -------------- | -------------- | ------------- |
| Address size   | 32 bits        | 128 bits      |
| Example        | `192.168.1.25` | `2001:db8::1` |
| Representation | Decimal        | Hexadecimal   |
| Address space  | ~4.3 billion   | \(2^{128}\)   |
| Broadcast      | ✅              | ❌             |

That last point is important.

IPv6 **doesn't use broadcast** like IPv4.

Instead, IPv6 uses **multicast** and **anycast** mechanisms for many-to-many or one-to-nearest communication patterns.

---

# 🧠 Does IPv6 eliminate NAT?

IPv6 was designed with a huge address space, so devices can have globally unique addresses without requiring IPv4-style address conservation through NAT.

But:

> **IPv6 does not inherently mean "no firewalls."**

Security still requires proper firewalling and access controls.

Also, NAT-like mechanisms can exist in IPv6 environments, but they aren't required for basic address conservation in the way IPv4 NAT commonly is.

---

# 🎯 Mental model

Think:

```text
IPv4
32 bits
↓
~4.3 billion addresses
↓
Address scarcity
```

versus:

```text
IPv6
128 bits
↓
2¹²⁸ addresses
↓
Massive address space
```

You don't need to memorize the gigantic \(2^{128}\) number.

Just remember:

> **IPv4 = 32-bit**

> **IPv6 = 128-bit**

Next up: **Default Gateway** — this connects directly with what we learned earlier about your laptop sending traffic from your LAN to the Internet.
