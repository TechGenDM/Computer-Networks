# 🏠 Home Router

Now let's put almost everything from this module together.

That little box in your home that gives you Wi-Fi is actually doing **many networking jobs at once**.

> **A home router is usually a combination of a router, switch, Wi-Fi access point, DHCP server, NAT/PAT device, and often a firewall.**

---

# 1. What does a Home Router actually do?

A typical home network looks like:

```text
                    🌍 INTERNET
                        │
                     ISP
                        │
                 ┌──────┴──────┐
                 │ HOME ROUTER │
                 └──────┬──────┘
                        │
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
       MacBook        Phone          TV
    192.168.1.10   192.168.1.11   192.168.1.12
```

The router may perform all of these functions:

```text
1. Routing
2. NAT/PAT
3. DHCP
4. Wi-Fi
5. Ethernet switching
6. Firewalling
```

---

# 2. Routing 🚦

This is its fundamental networking job.

Suppose your MacBook wants to reach:

```text
8.8.8.8
```

Your MacBook sees that it isn't local:

```text
192.168.1.10/24
        ↓
8.8.8.8 is outside local subnet
        ↓
Send to default gateway
        ↓
192.168.1.1
```

The router then looks at the destination IP and decides where to forward the packet.

```text
MacBook
   ↓
Router
   ↓
ISP
   ↓
Internet
```

---

# 3. DHCP 📡

Your router often acts as the **DHCP server**.

When your phone joins Wi-Fi, the router can give it:

```text
IP address
Subnet mask
Default gateway
DNS server
Lease time
```

For example:

```text
Phone
IP:      192.168.1.11
Gateway: 192.168.1.1
DNS:     192.168.1.1
```

So you don't manually configure every device.

---

# 4. NAT/PAT 🔄

Your home devices normally use private IPv4 addresses:

```text
192.168.1.10
192.168.1.11
192.168.1.12
```

The router can translate their Internet-bound traffic to an external address.

For example:

```text
192.168.1.10:53021
        ↓
      PAT
        ↓
203.0.113.50:62001
```

Then many devices can share the same external IPv4 address using different translated ports.

---

# 5. Wi-Fi 📶

The router often also contains a **wireless access point**.

Your laptop connects:

```text
MacBook
   )))))
Wi-Fi
   ↓
Router
```

The wireless access point handles the Wi-Fi radio communication.

Important distinction:

> **Wi-Fi access point ≠ router**, even though home devices commonly combine them into one box.

---

# 6. Ethernet Switch 🔌

Most home routers also have several Ethernet ports.

For example:

```text
Router
 ├── LAN Port 1 → PC
 ├── LAN Port 2 → TV
 ├── LAN Port 3 → Console
 └── LAN Port 4 → NAS
```

The built-in **switch** forwards Ethernet frames within the local network using MAC addresses.

So your home router may actually contain:

```text
Router
+
Switch
+
Wi-Fi AP
```

---

# 7. Firewall 🛡️

Home routers commonly also perform firewall functions.

For example, they can use rules/state to restrict unsolicited inbound traffic from the Internet.

Conceptually:

```text
Internet
   │
   │ incoming traffic
   ↓
Firewall
   │
   ├── allowed ✅
   └── blocked ❌
```

But remember our earlier distinction:

```text
NAT/PAT ≠ Firewall
```

They are different functions that often happen on the same device.

---

# 8. What happens when you open a website?

Now let's combine **everything**.

Suppose your MacBook is:

```text
192.168.1.10
```

and you open:

```text
https://example.com
```

### Step 1 — DHCP

Your MacBook already received:

```text
IP       → 192.168.1.10
Gateway  → 192.168.1.1
DNS      → 192.168.1.1
```

---

### Step 2 — DNS

MacBook asks the DNS resolver:

```text
"What is example.com's IP?"
```

It gets an IP address.

---

### Step 3 — Routing decision

MacBook determines:

```text
Destination isn't local
        ↓
Use default gateway
```

---

### Step 4 — ARP

It needs the router's MAC address.

```text
Who has 192.168.1.1?
        ↓
Router MAC
```

---

### Step 5 — Send frame to router

The frame has:

```text
Destination MAC → Router
```

while the IP packet has:

```text
Destination IP → Website server
```

---

### Step 6 — Router performs NAT/PAT

For example:

```text
192.168.1.10:53021
        ↓
203.0.113.50:62001
```

---

### Step 7 — Router forwards toward ISP

```text
Home Router
    ↓
ISP
    ↓
Internet
    ↓
Web Server
```

---

### Step 8 — Response comes back

The router uses its NAT/PAT state to associate the returning traffic with your MacBook.

```text
Public connection
      ↓
PAT mapping
      ↓
192.168.1.10:53021
      ↓
MacBook
```

Then your browser receives the HTTP response.

---

# 9. So what is the router's LAN IP?

A common home configuration is:

```text
Router LAN IP:
192.168.1.1
```

Devices on the LAN may have:

```text
192.168.1.10
192.168.1.11
192.168.1.12
...
```

The router's LAN address is often the:

> **Default gateway**

for those devices.

But the exact address doesn't have to be `192.168.1.1`; other private subnets are common.

---

# 10. WAN vs LAN Side

Your router has two conceptual sides.

```text
          WAN SIDE              LAN SIDE

        Internet
           │
           │
       [ Router ]
           │
           │
      Home Network
```

### WAN

Connects toward your ISP/upstream network.

### LAN

Your private home network.

For example:

```text
WAN:
203.x.x.x

LAN:
192.168.1.1
```

The router connects these two networks.

---

# 11. One device doing many jobs

This is the big realization:

```text
                 HOME ROUTER
                      │
      ┌───────────────┼────────────────┐
      ↓               ↓                ↓
   Routing          DHCP             NAT/PAT
      │               │                │
      ↓               ↓                ↓
   Forwarding     Give IPs        Internet sharing

      ┌───────────────┼────────────────┐
      ↓               ↓                ↓
   Wi-Fi           Switch           Firewall
```

That's why calling the box simply a **"Wi-Fi router"** hides a lot of functionality.

---

# 🧠 Best mental model

Think of your home router as the **gateway and traffic manager for your house**:

```text
             🏠 YOUR HOME
                  │
        ┌─────────┴─────────┐
        │     HOME ROUTER   │
        │                   │
        │ DHCP → IPs        │
        │ Routing → paths   │
        │ PAT → sharing IP  │
        │ Wi-Fi → wireless  │
        │ Switch → LAN      │
        │ Firewall → rules  │
        └─────────┬─────────┘
                  │
                ISP
                  │
               INTERNET
```

---

# 🔥 Must Remember

A typical home router can act as:

```text
✅ Default Gateway
✅ Router
✅ DHCP Server
✅ NAT/PAT Device
✅ Wi-Fi Access Point
✅ Ethernet Switch
✅ Firewall
```

And the most important distinction:

> **The home router is a device that connects your private LAN to an upstream network, while often combining several networking functions into one appliance.**

One more real-world nuance: depending on your ISP setup, the device you call a "router" may sit behind a separate modem/ONT, and some ISPs use **carrier-grade NAT (CGNAT)**, so the address shown on the router's WAN side isn't necessarily a globally routable public IPv4 address.

---

## Module 12 Progress

✅ Sliding Window  
✅ Socket Programming Concepts 🔌  
✅ Client-Server Model  
✅ ARP 🔎  
✅ NAT 🔄  
✅ PAT 🔀  
✅ DHCP 📡  
✅ Lease Process ⏳  
✅ Default Gateway 🚪  
✅ **Home Router 🏠**  
⬜ Private Networks

**Next → Private Networks 🔒** — we'll finish the module by bringing together **private IP ranges, LANs, NAT/PAT, and why addresses like `192.168.1.10` can be reused in millions of homes.**