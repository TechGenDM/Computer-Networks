Perfect. This is where **IPv4 addressing starts to make real sense**.

# 4. Network ID vs Host ID

Take an IPv4 address:

```text
192.168.1.25
```

An IP address doesn't simply mean "this particular computer."

It has two conceptual parts:

```text
┌──────────────────────┬─────────────┐
│      Network ID      │   Host ID   │
└──────────────────────┴─────────────┘
```

### Network ID

Identifies **which network** the device belongs to.

### Host ID

Identifies **which device/interface inside that network**.

---

## 🏠 Think of an address

Imagine:

```text
Apartment 25
Building 1
Street X
```

The **building/street** tells you the location.

The **apartment number** identifies the specific place within it.

Similarly:

```text
Network ID → Which network?
Host ID    → Which host in that network?
```

---

# But there's a catch...

Look at:

```text
192.168.1.25
```

Can I immediately say:

```text
Network = 192.168.1
Host = 25
```

❌ **Not necessarily.**

We need to know **where the boundary between network and host is**.

And that boundary is determined by the:

> **Subnet Mask**

For example, if the subnet is:

```text
255.255.255.0
```

then:

```text
IP:          192.168.1.25
Subnet mask: 255.255.255.0
```

In binary:

```text
IP:
11000000.10101000.00000001.00011001

Mask:
11111111.11111111.11111111.00000000
```

The `1`s in the mask represent the **network portion**:

```text
11111111.11111111.11111111 | 00000000
        NETWORK             | HOST
```

Therefore:

```text
Network ID = 192.168.1.0
Host portion = 25
```

---

# 🔥 Why do we need this division?

Imagine a router receives:

```text
Destination: 192.168.1.25
```

The router can determine that:

```text
192.168.1.0/24
```

is the destination network.

Instead of needing a separate routing entry for:

```text
192.168.1.1
192.168.1.2
192.168.1.3
...
192.168.1.254
```

it can have one route for the entire network:

```text
192.168.1.0/24
```

That's the power of **hierarchical addressing**.

---

# 🧠 Example

Suppose we have:

```text
Network: 192.168.1.0/24
```

Then the addresses conceptually look like:

```text
192.168.1.0   → Network address
192.168.1.1   → Host
192.168.1.2   → Host
192.168.1.3   → Host
...
192.168.1.254 → Host
192.168.1.255 → Broadcast
```

For this common `/24` example:

* Network portion = first **24 bits**
* Host portion = remaining **8 bits**

So there are:

[
2^8 = 256
]

total addresses.

Traditionally, two are reserved:

* `.0` → Network address
* `.255` → Broadcast address

So:

[
256 - 2 = 254
]

usable host addresses.

---

# 🎯 The key idea

Don't memorize:

> "First 3 octets are always Network ID."

That's **wrong**.

The network/host boundary depends on the **subnet mask or CIDR prefix**.

For example:

```text
192.168.1.25/24
```

has a different boundary than:

```text
192.168.1.25/16
```

We'll learn exactly how to calculate this in the next two topics:

**Subnet Mask → CIDR Notation**

Once those are clear, subnetting will become much easier.
