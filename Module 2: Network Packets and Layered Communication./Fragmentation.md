# 9️⃣ IP Fragmentation 📦✂️

## 🎯 **The Problem**

What happens when an IP packet is **larger than the link's MTU**?

```
IP Packet Size: 4000 bytes
Link MTU: 1500 bytes
                ↓
        MISMATCH!
        What happens?
```

For **IPv4**, the packet can be **split into smaller pieces** (fragments).  
For **IPv6**, fragmentation isn't allowed at routers (must handle it end-to-end).

---

## 📦 **How IPv4 Fragmentation Works**

### Original Packet
```
┌─────────────────────────────────┐
│      IP Packet: 4000 bytes      │
│  ┌──────────────────────────┐   │
│  │ IP Header (20 bytes)     │   │
│  ├──────────────────────────┤   │
│  │ Data: 3980 bytes         │   │
│  └──────────────────────────┘   │
└─────────────────────────────────┘
         ↓ EXCEEDED MTU (1500)
      FRAGMENTED
         ↓
```

### Fragmented Version
```
Fragment 1 (sent in Frame 1)
┌─────────────────────────────────┐
│ IP Header (with fragment flags) │
│ Data: 1480 bytes                │
│ ├─ MoreFragments = 1 (more coming)
│ ├─ Fragment Offset = 0           │
└─────────────────────────────────┘
         (Total: 1500 bytes = MTU)

Fragment 2 (sent in Frame 2)
┌─────────────────────────────────┐
│ IP Header (with fragment flags) │
│ Data: 1480 bytes                │
│ ├─ MoreFragments = 1 (more coming)
│ ├─ Fragment Offset = 1480       │
└─────────────────────────────────┘

Fragment 3 (sent in Frame 3)
┌─────────────────────────────────┐
│ IP Header (with fragment flags) │
│ Data: 1040 bytes (remaining)    │
│ ├─ MoreFragments = 0 (last one) │
│ ├─ Fragment Offset = 2960       │
└─────────────────────────────────┘
         (Total: 1060 bytes < MTU)
```

---

## 🔍 **IPv4 Fragmentation Fields**

Each fragment includes special information in the IP header:

### 1. **Identification (ID)**
```
16-bit field
Unique identifier for the original packet
All fragments of same packet share same ID

Example:
Original Packet ID = 0x1A2B
Fragment 1: ID = 0x1A2B
Fragment 2: ID = 0x1A2B
Fragment 3: ID = 0x1A2B

Receiver knows: "These 3 fragments belong together"
```

### 2. **Fragment Offset**
```
13-bit field
Tells receiver WHERE this fragment fits in original packet

Example (assuming 1480-byte payloads):
Fragment 1: Offset = 0        (starts at byte 0)
Fragment 2: Offset = 185      (starts at byte 1480)
Fragment 3: Offset = 370      (starts at byte 2960)

Receiver rebuilds in correct order:
0-1480 | 1480-2960 | 2960-4000
```

### 3. **More Fragments (MF) Flag**
```
1-bit flag
Set to 1 if more fragments are coming
Set to 0 for last fragment

Example:
Fragment 1: MF = 1  (more coming)
Fragment 2: MF = 1  (more coming)
Fragment 3: MF = 0  (this is the last)

Receiver knows when fragmentation is complete
```

### 4. **Don't Fragment (DF) Flag**
```
1-bit flag
Set by sender: "Please don't fragment this packet"

If set and packet exceeds MTU:
    → Router sends ICMP error
    → Sender learns PMTU and retries

Used for Path MTU Discovery
```

---

## 🔄 **Complete Fragmentation Example**

```
Step 1: Sender creates 3500-byte packet
┌──────────────────────────────────────┐
│ Src: 192.168.1.5                     │
│ Dst: 192.168.1.10                    │
│ ID: 1000                             │
│ DF: 0 (allow fragmentation)          │
│ Size: 3500 bytes                     │
│ Data: [AAAA...] (3480 bytes)        │
└──────────────────────────────────────┘

Step 2: Packet reaches Link 1 (MTU=1500)
Can't fit! Router fragments:

Fragment A:
┌──────────────────────────────────────┐
│ Src: 192.168.1.5                     │
│ Dst: 192.168.1.10                    │
│ ID: 1000                             │
│ Offset: 0                            │
│ MF: 1 (more coming)                  │
│ Size: 1500 bytes                     │
│ Data: [AAAA] (1480 bytes)           │
└──────────────────────────────────────┘

Fragment B:
┌──────────────────────────────────────┐
│ Src: 192.168.1.5                     │
│ Dst: 192.168.1.10                    │
│ ID: 1000                             │
│ Offset: 185 (1480/8)                 │
│ MF: 1 (more coming)                  │
│ Size: 1500 bytes                     │
│ Data: [AAAA] (1480 bytes)           │
└──────────────────────────────────────┘

Fragment C:
┌──────────────────────────────────────┐
│ Src: 192.168.1.5                     │
│ Dst: 192.168.1.10                    │
│ ID: 1000                             │
│ Offset: 370 (2960/8)                 │
│ MF: 0 (last fragment)                │
│ Size: 560 bytes                      │
│ Data: [AA] (540 bytes)               │
└──────────────────────────────────────┘

Step 3: Receiver gets all 3 fragments
Uses ID (1000) to group them together
Uses Offset to reassemble in order
Produces original 3500-byte packet
```

---

## ⚠️ **Why Fragmentation Is Bad**

### 1. Performance Cost
```
One packet → Multiple packets
  → More routing decisions
  → More processing
  → More transmission time
  → More potential loss
```

### 2. Reliability Reduction
```
Original: 1 packet
Fragmented: 3 packets

If ANY fragment is lost:
  → Entire original packet fails
  → Must retransmit ALL fragments
  → Wasted bandwidth

Loss probability:
Single packet: 1% loss chance
3 fragments: 1 - (0.99)³ = 2.97% loss chance
```

### 3. Overhead
```
Original Packet (1500 bytes):
  IP Header: 20 bytes
  Ratio: 20/1500 = 1.33% overhead

3 Fragments (3 × 20 byte headers):
  Total Headers: 60 bytes
  Ratio: 60/3500 = 1.71% overhead

More overhead means less useful data
```

---

## 🔑 **IPv4 vs IPv6 Fragmentation**

| Aspect | IPv4 | IPv6 |
|--------|------|------|
| **Router Fragmentation** | ✅ YES | ❌ NO |
| **Sender Fragmentation** | ❌ NO | ✅ YES |
| **Handles oversized packets** | Router splits them | Sender must not send them |
| **Path MTU Discovery** | Optional | REQUIRED |
| **Efficiency** | Variable | Better (must discover PMTU first) |

### The IPv6 Philosophy
IPv6 **shifts responsibility to the sender**:
- Sender uses Path MTU Discovery
- Sender learns optimal packet size
- Sender never sends oversized packets
- Result: No fragmentation at routers = better efficiency

---

## 🌐 **Path MTU Discovery (PMTU)**

Real-world networks have different links with different MTUs:

```
Laptop      Router A    Router B    Router C    Server
  ↓           ↓          ↓           ↓           ↓
MTU=1500   MTU=1500   MTU=1200   MTU=1500    MTU=1500
```

The **Path MTU** is the minimum:

```
Path MTU = MIN(1500, 1500, 1200, 1500, 1500) = 1200 bytes
```

### How Discovery Works (IPv4)
```
1. Sender creates 1500-byte packet
2. Sets DF (Don't Fragment) flag = 1
3. Sends through network

4. Router B can't send (MTU=1200)
5. Router B sends ICMP "Fragmentation Needed" error
6. Error includes: "My MTU is 1200"

7. Sender receives error
8. Learns Path MTU = 1200
9. Resends with 1200-byte packets (or smaller)
10. No more fragmentation!
```

### Why DF Flag Matters
```
DF = 0: Allows fragmentation (bad for discovering PMTU)
DF = 1: Prevents fragmentation (good for PMTU discovery)

Modern TCP uses DF=1 by default
With Path MTU Discovery active
```

---

## 🔥 **Modern Best Practice**

```
❌ Old approach: Hope router fragments if needed
  → Fragmentation happens randomly
  → Unpredictable performance

✅ Modern approach: Discover path MTU first
  → Sender learns optimal packet size
  → No fragmentation needed
  → Predictable, efficient performance
```

### How Modern TCP Works
```
1. TCP connection starts
2. TCP sends DF=1 flag
3. Network discovers Path MTU
4. Sender uses optimal packet size (MSS)
5. No fragmentation ever happens
6. Smooth, efficient transmission
```

---

## 📊 **Real-World Impact**

### Scenario 1: Gaming
```
Game sends 1500-byte packet
Some routers fragment it
Each fragment adds latency
Player experiences lag spike

Solution: Use smaller packets
More packets, but no fragmentation
Smoother gameplay
```

### Scenario 2: VPN
```
Normal packet: 1500 bytes
VPN encryption: +30-40 bytes overhead
Total: 1530-1540 bytes
        ↓
Exceeds 1500 MTU
        ↓
Gets fragmented
        ↓
VPN performance degrades

Solution: VPN negotiates MSS to ~1460 bytes
Accounts for VPN overhead
No fragmentation
```

### Scenario 3: Jumbo Frames
```
Data center with Jumbo Frames (9000 MTU)
Can send 9000-byte packets
Fewer packets needed
Lower overhead
Better efficiency

But only within data center
Must reduce to 1500 for Internet
```

---

## 🧠 **Remember**

**Fragmentation is what HAPPENS when packet > MTU**

```
Packet Size      Action
─────────────────────────────────
≤ MTU            ✅ Send as-is
> MTU (IPv4)     ⚠️  Fragment & reassemble
> MTU (IPv6)     ❌ Error (sender must avoid)
```

**Best practice:**
1. **IPv4:** Use Path MTU Discovery (DF flag)
2. **IPv6:** MUST use Path MTU Discovery
3. **Never send oversized packets**
4. **Avoid fragmentation at all costs**

**Result:** Faster, more reliable networking!