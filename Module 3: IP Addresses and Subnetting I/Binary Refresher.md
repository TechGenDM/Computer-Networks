# 3. Binary Refresher 🔢

Don't worry—we only need the **basics of binary** for networking. Once you understand these, subnetting becomes much easier.

---

## 1. Decimal vs Binary

Humans normally use **decimal (base 10)**:

```text
0 1 2 3 4 5 6 7 8 9
```

Computers fundamentally work with **binary (base 2)**:

```text
0 1
```

Every binary digit is called a **bit**.

---

# 2. Binary Place Values

For 8 bits, the positions are:

```text
128  64  32  16  8  4  2  1
```

Why?

Each position is a power of 2:

```text
2⁷  2⁶  2⁵  2⁴  2³  2²  2¹  2⁰
128  64  32  16   8   4   2   1
```

So an 8-bit binary number looks like:

```text
0 0 0 0 0 0 0 0
↑               ↑
128             1
```

---

# 3. Convert Binary → Decimal

Take:

```text
11000000
```

Put the place values underneath:

```text
1 1 0 0 0 0 0 0
│ │ │ │ │ │ │ │
128 64 32 16 8 4 2 1
```

Only the positions containing **1** count:

```text
128 + 64 = 192
```

Therefore:

> `11000000` = **192**

---

## Another example

```text
10101000
```

Calculate:

```text
128 + 32 + 8
= 168
```

Therefore:

> `10101000` = **168**

That's why:

```text
192.168.1.25
```

starts as:

```text
11000000.10101000...
```

---

# 4. Convert Decimal → Binary

Let's convert:

**25**

Look at the place values:

```text
128 64 32 16 8 4 2 1
```

Can 25 contain 128? ❌ → `0`

64? ❌ → `0`

32? ❌ → `0`

16? ✅ → `1`

Remaining:

```text
25 - 16 = 9
```

8? ✅ → `1`

Remaining:

```text
9 - 8 = 1
```

4? ❌ → `0`

2? ❌ → `0`

1? ✅ → `1`

Therefore:

```text
25 = 00011001
```

---

# 🔥 The numbers you MUST know

For subnetting, memorize this table:

|     Binary | Decimal |
| ---------: | ------: |
| `10000000` |     128 |
| `01000000` |      64 |
| `00100000` |      32 |
| `00010000` |      16 |
| `00001000` |       8 |
| `00000100` |       4 |
| `00000010` |       2 |
| `00000001` |       1 |

And some useful combinations:

```text
128 + 64 = 192
128 + 32 = 160
128 + 16 = 144
128 + 8  = 136
128 + 4  = 132
128 + 2  = 130
128 + 1  = 129
```

---

# 🧠 The most important networking pattern

You'll see subnet masks like:

```text
255.255.255.0
```

In binary:

```text
11111111.11111111.11111111.00000000
```

Notice:

```text
255 = 11111111
```

and:

```text
0 = 00000000
```

This is going to become **extremely important** when we learn subnet masks.

---

## 🎯 Quick test

Try these mentally:

### 1. What is:

```text
11111111
```

### 2. What is:

```text
00001111
```

### 3. Convert:

```text
200
```

to 8-bit binary.

If you can do those, you're ready for **Network ID vs Host ID**.
