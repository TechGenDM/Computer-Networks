# 📡 DHCP — Dynamic Host Configuration Protocol

Now we come to something that happens **automatically every time your laptop or phone joins most Wi-Fi networks**.

You connect to Wi-Fi, and suddenly your device has:

```text
IP address
Subnet mask
Default gateway
DNS server
```

Who gives all of this to your device?

> **DHCP.**

---

# 1. What is DHCP?

**DHCP (Dynamic Host Configuration Protocol)** automatically provides network configuration to devices.

Instead of manually configuring:

```text
IP:      192.168.1.10
Mask:    255.255.255.0
Gateway: 192.168.1.1
DNS:     8.8.8.8
```

your device asks a DHCP server to provide the required settings.

Mental model:

```text
Device
   ↓
"Can I get network configuration?"
   ↓
DHCP Server
   ↓
IP + Mask + Gateway + DNS + ...
```

---

# 2. Why do we need DHCP?

Imagine a college network with:

```text
5,000 devices
```

An administrator would not want to manually assign:

```text
Device 1 → 10.0.0.1
Device 2 → 10.0.0.2
Device 3 → 10.0.0.3
...
```

DHCP automates this.

It can:

```text
✅ Assign IP addresses
✅ Provide subnet mask/prefix
✅ Provide default gateway
✅ Provide DNS server information
✅ Specify lease duration
```

---

# 3. DHCP Uses UDP

DHCP commonly uses:

```text
UDP port 67 → DHCP server
UDP port 68 → DHCP client
```

So:

```text
Client ── UDP ──→ DHCP Server
```

Why UDP?

A new client may not yet have:

```text
an IP address
```

so it needs a simple mechanism that works before normal IP communication is fully configured.

---

# 4. The Famous DORA Process 🚀

DHCP's initial address-allocation process is commonly remembered as:

```text
D → Discover
O → Offer
R → Request
A → Acknowledge
```

### DORA

```text
Discover
   ↓
Offer
   ↓
Request
   ↓
ACK
```

Let's trace it.

---

# 5. Step 1 — DHCP Discover 📢

Your laptop joins the Wi-Fi network but doesn't yet have an IP address.

It broadcasts a DHCP Discover message:

> **"Is there a DHCP server that can configure me?"**

Conceptually:

```text
Laptop
  │
  │ DHCP Discover 📢
  ↓
Local Network
```

The client may use:

```text
Source IP:      0.0.0.0
Destination IP: 255.255.255.255
```

because it doesn't yet know its usable IP configuration.

---

# 6. Step 2 — DHCP Offer 📩

A DHCP server receives the Discover and offers an address.

For example:

```text
DHCP Server:

"I can offer you:
IP = 192.168.1.25
Mask = 255.255.255.0
Gateway = 192.168.1.1
DNS = 192.168.1.1"
```

So:

```text
DHCP Server
     │
     │ DHCP Offer
     ↓
  Laptop
```

---

# 7. Step 3 — DHCP Request 📤

The client chooses an offer and sends a DHCP Request.

Conceptually:

> **"I'd like to use the offered 192.168.1.25."**

```text
Laptop
  │
  │ DHCP Request
  ↓
DHCP Server
```

This also tells other DHCP servers whose offers weren't selected that their offers are not being used.

---

# 8. Step 4 — DHCP ACK ✅

The server confirms the allocation:

```text
DHCP Server
     │
     │ DHCP ACK
     ↓
Laptop
```

Now your device has its configuration:

```text
IP Address     = 192.168.1.25
Subnet Mask    = 255.255.255.0
Gateway        = 192.168.1.1
DNS Server     = 192.168.1.1
Lease Time     = ...
```

Your device can now participate normally in the network.

---

# 9. Complete DORA Flow

```text
        DHCP CLIENT                    DHCP SERVER

             │
             │  DHCP DISCOVER 📢
             │──────────────────────→│
             │                       │
             │  DHCP OFFER 📩        │
             │←──────────────────────│
             │                       │
             │  DHCP REQUEST 📤      │
             │──────────────────────→│
             │                       │
             │  DHCP ACK ✅          │
             │←──────────────────────│
             │
          IP configured
```

🔥 **DORA** is absolutely worth memorizing.

---

# 10. What Information Does DHCP Give?

The IP address is only one part.

DHCP can provide configuration such as:

```text
IP address
Subnet mask / prefix
Default gateway
DNS servers
Lease duration
```

For example:

```text
Your MacBook

IP:
192.168.1.25

Subnet:
255.255.255.0

Gateway:
192.168.1.1

DNS:
192.168.1.1
```

Now connect this with what we've already learned:

```text
DHCP
 ↓
IP configuration
 ↓
Default Gateway
 ↓
DNS
 ↓
Internet communication
```

---

# 11. DHCP Server in a Home Network 🏠

In a typical home network, your **router usually acts as the DHCP server**.

Example:

```text
                 Internet
                     │
                🏠 Router
             ┌───────┴───────┐
             │ DHCP Server   │
             │ Gateway       │
             │ NAT/PAT       │
             └───────┬───────┘
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
       MacBook     Phone        TV
```

The router might assign:

```text
MacBook → 192.168.1.25
Phone   → 192.168.1.26
TV      → 192.168.1.27
```

---

# 12. DHCP Does Not Provide Internet Access by Itself

This is important.

DHCP gives your device **configuration**.

It doesn't itself route your packets to Google.

The roles are different:

```text
DHCP
→ "Here's your network configuration."

Default Gateway
→ "Send non-local traffic to me."

DNS
→ "Here's the IP for this domain."

Router/NAT
→ "I'll forward/translate traffic toward the Internet."
```

These pieces work together.

---

# 🧠 Real-life example

You open your MacBook and connect to your home Wi-Fi.

```text
1. MacBook joins Wi-Fi
        ↓
2. MacBook has no usable local IP yet
        ↓
3. DHCP Discover
        ↓
4. Router offers an IP
        ↓
5. MacBook requests it
        ↓
6. Router sends DHCP ACK
        ↓
7. MacBook gets:
       IP
       Subnet
       Gateway
       DNS
        ↓
8. Now normal network communication can begin
```

---

# 🔥 Must Remember

### DHCP

> **Dynamic Host Configuration Protocol automatically provides network configuration to clients.**

### DORA

```text
D → Discover
O → Offer
R → Request
A → ACK
```

### Ports

```text
DHCP Server → UDP 67
DHCP Client → UDP 68
```

### Typical home router

Often acts as:

```text
DHCP Server
+
Default Gateway
+
NAT/PAT Device
+
Wi-Fi Access Point
```

---

## Module 12 Progress

✅ Sliding Window  
✅ Socket Programming Concepts 🔌  
✅ Client-Server Model  
✅ ARP 🔎  
✅ NAT 🔄  
✅ PAT 🔀  
✅ **DHCP 📡**  
⬜ Lease Process  
⬜ Default Gateway  
⬜ Home Router  
⬜ Private Networks  

**Next → DHCP Lease Process ⏳** — we'll understand what happens **after DORA**, how long your IP remains assigned, and how your device renews the lease without doing DORA from scratch every time.