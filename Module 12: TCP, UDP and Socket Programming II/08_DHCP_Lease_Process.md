<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 12: TCP, UDP and Socket Programming II* • **Topic 08 of 11** (Global #086)

| [⬅️ Previous: 📡 DHCP — Dynamic Host Configuration Protocol](./07_DHCP.md) | 📑 [**Module Overview**](./README.md) | [Next: 🚪 Default Gateway ➡️](./09_Default_Gateway.md) |
| :--- | :---: | ---: |

---

</div>

# ⏳ DHCP Lease Process

Great — now we go one level deeper into DHCP.

When a DHCP server gives your device an IP address, it usually **doesn't give it permanently**.

Instead, it gives the address for a limited period called a:

> **DHCP Lease**

---

# 1. What is a DHCP Lease?

Suppose your router gives your MacBook:

```text
IP Address: 192.168.1.25
Lease Time: 8 hours
```

This means:

> "You may use `192.168.1.25` for this lease period, subject to renewal."

So:

```text
DHCP
 ↓
192.168.1.25
 ↓
Valid for 8 hours
```

The purpose is to let the DHCP server **manage and reuse addresses efficiently**.

---

# 2. Why not make the IP permanent?

Imagine a network with:

```text
100 IP addresses
500 devices
```

Not all 500 devices are online at once.

If an IP were permanently assigned to every device, many addresses could remain unused.

With leases:

```text
Device joins
   ↓
Gets IP
   ↓
Uses IP temporarily
   ↓
Device leaves
   ↓
Lease eventually expires
   ↓
IP can be reused
```

---

# 3. Lease Lifecycle

The basic lifecycle looks like:

```text
        Get Lease
           ↓
       BOUND ✅
           ↓
      T1 reached
           ↓
      RENEWING
           ↓
      DHCPREQUEST
           ↓
       DHCPACK
           ↓
       New lease
```

If renewal fails:

```text
T1
 ↓
RENEWING
 ↓
No response
 ↓
T2
 ↓
REBINDING
 ↓
Try any available DHCP server
```

If the lease ultimately expires:

```text
Lease expires
      ↓
IP is no longer valid
      ↓
Need to obtain/confirm configuration again
```

---

# 4. T1 — Renewal Time 🔄

DHCP defines a **T1 renewal timer**.

By default, T1 is typically:

```text
50% of the lease duration
```

Example:

```text
Lease = 8 hours

T1 = 4 hours
```

At T1, the client tries to renew the lease with the DHCP server that originally provided it.

Conceptually:

```text
MacBook
   │
   │ DHCPREQUEST
   ↓
Original DHCP Server
```

The client is essentially saying:

> "I'd like to continue using this IP."

---

# 5. DHCPACK — Renewal Successful ✅

Suppose the server replies:

```text
DHCPACK
```

The lease is renewed.

Example:

```text
Original lease:
8 hours

Renewal accepted:
another lease period
```

The timers are recalculated based on the new lease.

So your device can continue using:

```text
192.168.1.25
```

without interruption.

---

# 6. T2 — Rebinding Time

What happens if the original DHCP server doesn't respond?

The client waits until **T2**.

By default:

```text
T2 = 87.5% of lease duration
```

For an 8-hour lease:

```text
T1 = 4 hours
T2 = 7 hours
```

At T2, the client enters:

> **REBINDING**

Now it can try to reach **any available DHCP server**, rather than only the original one.

Conceptually:

```text
        Original DHCP Server
               ❌
               ↑
MacBook
               ↓
       Other DHCP Server
               ✅
```

---

# 7. Why T1 and T2?

This gives DHCP two opportunities.

### T1

> "Let me try to renew with the server that gave me this address."

### T2

> "That server isn't responding. I'll ask any DHCP server that can renew me."

So:

```text
50%                87.5%                 100%
 │                    │                     │
 ↓                    ↓                     ↓
T1                   T2                 Expiration
 │                    │                     │
Renew              Rebind                Stop
```

---

# 8. What Happens When the Lease Expires? ❌

Suppose:

```text
Lease = 8 hours
```

and neither renewal nor rebinding succeeds.

At the end:

```text
Lease expires
       ↓
Client must stop using that leased IP
```

The device may then need to obtain valid configuration again, potentially through the DHCP discovery process.

---

# 9. Does This Happen Every Time You Join Wi-Fi?

Not necessarily.

DHCP clients try to **reuse/confirm previous configuration when appropriate**, rather than blindly doing a full DORA exchange every single time.

For example, a client that reboots while its previous lease is still usable can use the **INIT-REBOOT** behavior to request that previously assigned address.

So don't think:

```text
Every Wi-Fi connection
→ Always full DORA
```

It's more nuanced.

---

# 10. Real Example 🧑‍💻

Suppose your router gives your laptop:

```text
IP:
192.168.1.25

Lease:
8 hours
```

Timeline:

```text
0h
│
│ DHCP ACK
│
├───────────────
│
4h → T1
│    Try renewal
│
├───────────────
│
7h → T2
│    Try rebinding
│
├───────────────
│
8h → Lease expires
```

If renewal works at 4h:

```text
✅ Lease continues
```

If original server doesn't respond but another DHCP server does during rebinding:

```text
✅ Lease continues
```

If nobody responds:

```text
❌ Lease eventually expires
```

---

# 11. DHCP Lease vs DNS TTL

Since we've studied DNS caching, don't mix these up.

### DHCP Lease

```text
How long the client may use
its assigned network configuration/address.
```

### DNS TTL

```text
How long a DNS answer may be
cached before it should be refreshed.
```

They're completely different timers.

---

# 🧠 Best mental model

Imagine a parking spot.

```text
DHCP server
    ↓
"You can use Parking Spot #25
 for the next 8 hours."
```

Before the time is over:

```text
You → "Can I keep it?"
```

At T1:

```text
Try the original manager.
```

At T2:

```text
Original manager unavailable?
Ask another authorized manager.
```

If nobody renews it:

```text
Lease expires
↓
Spot becomes available for reassignment
```

That's basically DHCP leasing.

---

# 🔥 Must Remember

### Lease

```text
IP/configuration
+
time limit
```

### Default timers

```text
T1 ≈ 50% of lease
   ↓
Renew with original DHCP server

T2 ≈ 87.5% of lease
   ↓
Rebind with any available DHCP server

100%
   ↓
Lease expires
```

### Flow

```text
DHCP ACK
   ↓
BOUND
   ↓
T1 → RENEWING
   ↓
success → renewed

failure
   ↓
T2 → REBINDING
   ↓
success → renewed

failure
   ↓
Expiration
```

---

## Module 12 Progress

✅ Sliding Window  
✅ Socket Programming Concepts 🔌  
✅ Client-Server Model  
✅ ARP 🔎  
✅ NAT 🔄  
✅ PAT 🔀  
✅ DHCP 📡  
✅ **Lease Process ⏳**  
⬜ Default Gateway

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What is the four-step acronym for the DHCP lease acquisition process?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> **DORA**: **D**iscover (client broadcast), **O**ffer (server unicast/broadcast), **R**equest (client broadcast), **A**cknowledge (server confirmation).
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 📡 DHCP — Dynamic Host Configuration Protocol](./07_DHCP.md) | [**Module 12: TCP, UDP and Socket Programming II**](./README.md) | [Next: 🚪 Default Gateway ➡️](./09_Default_Gateway.md) |

<div align="center">
  <br/>
  <code>[███████░░░] 76% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
