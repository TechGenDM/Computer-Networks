# 2. Variable Length Subnetting (VLSM) 🔀

**VLSM = Variable Length Subnet Masking**

It's basically **subnetting with different-sized subnets**.

In our previous example, we divided:

```text
192.168.1.0/24
```

into `/26` subnets, meaning **every subnet had the same size**.

That's called **FLSM (Fixed Length Subnet Masking)**.

But what if departments need different numbers of hosts?

```text
Engineering → 100 hosts
HR          → 30 hosts
Management  → 10 hosts
```

Giving everyone a `/26` would waste addresses.

VLSM solves this.

---

## 🧠 The basic idea

We give each department a subnet based on its actual requirement:

```text
Engineering → /25 → 128 addresses
HR          → /27 → 32 addresses
Management  → /28 → 16 addresses
```

So:

```text
Large requirement  → Large subnet
Small requirement  → Small subnet
```

---

# Example

Suppose we have:

```text
192.168.1.0/24
```

And need:

```text
A → 100 hosts
B → 50 hosts
C → 20 hosts
```

Start with the **largest requirement**.

### A: 100 hosts

Need at least 100 usable addresses.

`/25` gives:

$$
2^7 = 128
$$

128 total → **126 usable**

So:

```text
A → 192.168.1.0/25
```

Range:

```text
192.168.1.0 – 192.168.1.127
```

---

### B: 50 hosts

`/26` gives:

$$
2^6 = 64
$$

64 total → **62 usable**

Next available block:

```text
B → 192.168.1.128/26
```

Range:

```text
192.168.1.128 – 192.168.1.191
```

---

### C: 20 hosts

`/27` gives:

$$
2^5 = 32
$$

32 total → **30 usable**

Next available:

```text
C → 192.168.1.192/27
```

Range:

```text
192.168.1.192 – 192.168.1.223
```

We still have:

```text
192.168.1.224 – 192.168.1.255
```

available for future use.

---

# 🔥 Why VLSM is useful

Without VLSM:

```text
100 hosts → /25
50 hosts  → /25 ❌ waste
20 hosts  → /25 ❌ huge waste
```

With VLSM:

```text
100 hosts → /25
50 hosts  → /26
20 hosts  → /27
```

Much more efficient.

---

## 🎯 Remember

> **FLSM = same-size subnets**

> **VLSM = different-size subnets**

And the golden rule when designing VLSM:

> **Allocate the largest subnet first, then progressively smaller ones.**

VLSM is useful to understand, but don't worry if it's not your immediate priority. The next topic—**IPv6**—is much more important for understanding modern IP networking.
