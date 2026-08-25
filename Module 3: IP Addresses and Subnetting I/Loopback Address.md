# 8. Loopback Address 🔄

This is a small topic, but **very useful for you as a developer**.

## What is Loopback?

A **loopback address** is used when a device wants to communicate **with itself**.

Instead of:

```text
Laptop → Router → Internet → Laptop
```

the communication stays inside the same machine:

```text
Application A
     ↓
   Network Stack
     ↓
Application B
```

No physical network interface or external network is required.

---

## 🔑 IPv4 Loopback

The entire range:

```text
127.0.0.0/8
```

is reserved for loopback.

The address you'll use most often is:

```text
127.0.0.1
```

It's commonly called:

> **localhost**

So:

```text
127.0.0.1 = localhost
```

---

# 💻 Why do developers use `127.0.0.1`?

Suppose you're developing a backend using Node.js:

```text
http://localhost:3000
```

Your browser might be communicating with:

```text
127.0.0.1:3000
```

The `3000` is the **port number**, which we'll study later.

Conceptually:

```text
┌─────────────────────────────┐
│          Your PC            │
│                             │
│ Browser ─────► Backend      │
│          127.0.0.1:3000     │
│                             │
└─────────────────────────────┘
```

The request doesn't need to leave your machine.

---

# 🧠 `localhost` vs `127.0.0.1`

Usually:

```text
localhost
   ↓
DNS/hosts resolution
   ↓
127.0.0.1
```

But technically, `localhost` is a **hostname**, while `127.0.0.1` is an **IPv4 address**.

For example:

```text
http://localhost:3000
```

and:

```text
http://127.0.0.1:3000
```

often reach the same local service.

---

# ⚠️ Very important for development

Consider your backend running at:

```text
127.0.0.1:3000
```

Your laptop can access it.

But another device on your Wi-Fi generally **cannot access your laptop's `127.0.0.1:3000`**.

Why?

Because `127.0.0.1` always means:

> **"This device itself."**

On your phone:

```text
127.0.0.1
```

means **your phone**, not your laptop.

On your laptop:

```text
127.0.0.1
```

means **your laptop**.

---

## 🔥 Compare these

Suppose your laptop has:

```text
Private IP:
192.168.1.10
```

And your backend runs on port 3000.

### From your laptop:

```text
localhost:3000
```

works.

### From your phone:

```text
localhost:3000
```

❌ This points to the phone itself.

Instead, you might use:

```text
192.168.1.10:3000
```

provided the server is listening on an appropriate interface and your firewall/network settings allow it.

---

# 🎯 Mental model

```text
127.0.0.1
    ↓
"This machine"
    ↓
Loopback
    ↓
No external network required
```

And:

```text
192.168.1.10
    ↓
"This device on my LAN"
    ↓
Can potentially be reached by other devices
```