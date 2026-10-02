<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 12: TCP, UDP and Socket Programming II* • **Topic 05 of 11** (Global #083)

| [⬅️ Previous: 🔎 ARP — Address Resolution Protocol](./04_ARP.md) | 📑 [**Module Overview**](./README.md) | [Next: 🔀 PAT — Port Address Translation ➡️](./06_PAT.md) |
| :--- | :---: | ---: |

---

</div>

# 🔄 NAT — Network Address Translation

Now we get to a concept you encounter **every time a device on your home Wi-Fi accesses the Internet**.

The core idea:

> **NAT translates IP addresses between different networks, commonly between private addresses inside a local network and a public address used on the Internet.**

---

# 1. Why do we need NAT?

Your home might have dozens of devices:

```text
Laptop    → 192.168.1.10
Phone     → 192.168.1.11
TV        → 192.168.1.12
MacBook   → 192.168.1.13
```

These are **private IPv4 addresses**.

They aren't globally routable on the public Internet.

But your ISP gives your home router a public address:

```text
Public IP → 203.0.113.50
```

So we have:

```text
PRIVATE NETWORK                INTERNET

192.168.1.10 ─┐
192.168.1.11 ─┤
192.168.1.12 ─┤
192.168.1.13 ─┤
              ↓
         🏠 Router
              ↓
       203.0.113.50
              ↓
          Internet
```

NAT allows the router to translate between these address spaces.

---

# 2. What does NAT actually do?

Suppose your laptop:

```text
192.168.1.10
```

wants to contact a server:

```text
142.250.x.x
```

Before leaving your network, the router can translate the source address.

Conceptually:

```text
Before NAT:

Source IP      = 192.168.1.10
Destination IP = 142.250.x.x
```

After NAT:

```text
Source IP      = 203.0.113.50
Destination IP = 142.250.x.x
```

The Internet sees the public source address.

---

# 3. NAT Translation Table

The router needs to remember the translation.

For example:

```text
Private              Public
192.168.1.10    →    203.0.113.50
```

Then when the server responds:

```text
Server
  ↓
203.0.113.50
  ↓
Router
```

the router knows which internal device should receive the response.

---

# 4. But there's a problem...

What happens when **multiple devices** use the same public IP?

For example:

```text
Laptop → 203.0.113.50
Phone  → 203.0.113.50
TV     → 203.0.113.50
```

How does the router distinguish their connections?

That's where **PAT** comes in.

So:

```text
NAT
 ↓
Address translation

PAT
 ↓
Address + Port translation
```

We'll study PAT next.

---

# 5. NAT and Ports

Suppose your laptop has:

```text
192.168.1.10:5000
```

and connects to:

```text
142.250.x.x:443
```

The router might translate:

```text
192.168.1.10:5000
        ↓
203.0.113.50:62001
```

Now the Internet sees:

```text
203.0.113.50:62001 → 142.250.x.x:443
```

The router records the mapping so that the response can be sent back to:

```text
192.168.1.10:5000
```

This particular form of many-private-devices-sharing-one-public-IP is usually called **PAT (Port Address Translation)**, also commonly referred to as **NAT overload**.

---

# 6. Private vs Public Address

Let's connect this to what you learned earlier.

### Private IP

Examples:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

Used inside private networks.

### Public IP

Globally routable address assigned for Internet communication.

NAT commonly sits between them:

```text
Private IP
    ↓
   NAT
    ↓
Public IP
```

---

# 7. Where does NAT happen?

Usually on a **router/firewall at the edge of the private network**.

For a home network:

```text
                  Internet
                     │
              Public IP
                     │
                🏠 Router
               NAT/PAT
                     │
           ┌─────────┼─────────┐
           ↓         ↓         ↓
        Laptop     Phone       TV
     192.168.1.x 192.168.1.x 192.168.1.x
```

The home router is therefore doing several jobs at once:

```text
Routing
NAT/PAT
DHCP
Firewalling
Wi-Fi access
```

We'll connect these concepts later.

---

# 8. Is NAT a Firewall?

**No.**

This is an important distinction.

NAT's primary job is:

> **Address/port translation.**

A firewall's job is:

> **Allow or block traffic according to security rules.**

A home router commonly performs both, which is why they can feel like the same thing.

---

# 9. Static NAT vs Dynamic NAT

At a high level, NAT doesn't have to mean "many devices share one address."

### Static NAT

A fixed mapping:

```text
192.168.1.10
      ↓
203.0.113.10
```

One private address consistently maps to one public address.

### Dynamic NAT

A private address is mapped to an available public address from a pool.

```text
Private IP
    ↓
NAT pool
    ↓
Available public IP
```

### PAT

Multiple private devices can share **one public IP**, distinguished using ports.

```text
192.168.1.10:5000 → 203.0.113.50:62001
192.168.1.11:5000 → 203.0.113.50:62002
192.168.1.12:5000 → 203.0.113.50:62003
```

This is what you'll most commonly see in home networks.

---

# 10. Complete Home Example 🏠

Your MacBook:

```text
Private IP:
192.168.1.10

Source port:
53021
```

Google server:

```text
142.250.x.x:443
```

Your router performs translation:

```text
192.168.1.10:53021
          ↓
NAT/PAT
          ↓
203.0.113.50:62001
          ↓
142.250.x.x:443
```

Server responds:

```text
142.250.x.x:443
          ↓
203.0.113.50:62001
```

Router checks its translation state:

```text
203.0.113.50:62001
          ↓
192.168.1.10:53021
```

and forwards the data to your MacBook.

---

# 🧠 Best mental model

Think of your house having many internal phone extensions:

```text
Room 1 → extension 101
Room 2 → extension 102
Room 3 → extension 103
```

The outside world sees only the **main phone number**.

The router keeps track of which internal device corresponds to each connection.

```text
Private network
      ↓
   🏠 Router
      ↓
One public identity
      ↓
Internet
```

---

# 🔥 Must Remember

> **NAT translates network addresses, commonly private IPv4 addresses into public addresses at a network boundary.**

Remember:

```text
Private IP
    ↓
   NAT
    ↓
Public IP
```

And especially:

```text
NAT → address translation
PAT → address + port translation
```

For a typical home network, **PAT is what allows many devices to share one public IPv4 address simultaneously.**

---

## Module 12 Progress

✅ Sliding Window  
✅ Socket Programming Concepts 🔌  
✅ Client-Server Model  
✅ ARP 🔎  
✅ **NAT 🔄**  
⬜ PAT  
⬜ DHCP  
⬜ Lease Process  
⬜ Default Gateway

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What is the difference between Static NAT and Dynamic NAT?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> Static NAT maps one private IP to one fixed public IP permanently (often for internal web servers). Dynamic NAT maps a private IP to an available public IP from a temporary pool.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🔎 ARP — Address Resolution Protocol](./04_ARP.md) | [**Module 12: TCP, UDP and Socket Programming II**](./README.md) | [Next: 🔀 PAT — Port Address Translation ➡️](./06_PAT.md) |

<div align="center">
  <br/>
  <code>[███████░░░] 74% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
