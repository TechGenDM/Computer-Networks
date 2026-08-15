# 8️⃣ MTU — Maximum Transmission Unit 📏

## 🎯 **Core Concept**

**MTU = the maximum size of data (IP packet) that can fit into a single frame on a particular network link without being split up.**

Think of it like **a truck's weight limit**:
- The truck can carry a maximum of X kg
- If your cargo is heavier, you need multiple trucks or a bigger truck
- Networks work the same way with packets

---

## 📊 **Common MTU Values**

Different network types have different MTUs:

| Network Type | MTU | Overhead | Payload |
|--------------|-----|----------|---------|
| **Ethernet** (standard) | 1500 | 18 bytes | 1482 bytes |
| **Wi-Fi (802.11)** | 1500 | 18 bytes | 1482 bytes |
| **Loopback (localhost)** | 65535 | Minimal | Maximum |
| **PPP (dial-up)** | 576 | 20+ bytes | ~556 bytes |
| **Jumbo Frame Ethernet** | 9000 | 18 bytes | 8982 bytes |
| **Token Ring** | 4464 | Varies | Varies |
| **5G Cellular** | 1500-3000 | Varies | Varies |

**Most common: 1500 bytes (Ethernet standard)**

---

## 🔍 **How MTU Works**

### Scenario: Normal Packet

```
Application sends: "GET /youtube"
        ↓
TCP wraps it: TCP Segment (100 bytes total)
        ↓
IP wraps it: IP Packet (120 bytes total)
        ↓
Ethernet wraps it: Ethernet Frame (120 + 18 overhead = 138 bytes)
        ↓
Link MTU = 1500 bytes
        ↓
138 bytes < 1500 bytes ✅ FITS!
        ↓
Frame sent successfully
```

### Scenario: Large Packet

```
Application sends: Large file (2 MB)
        ↓
TCP wraps it: TCP Segment (1480 bytes payload)
        ↓
IP wraps it: IP Packet (1500 bytes total)
        ↓
Ethernet wraps it: Ethernet Frame (1500 + 18 = 1518 bytes)
        ↓
Link MTU = 1500 bytes
        ↓
1500 bytes = 1500 bytes ✅ FITS (exactly!)
        ↓
But the 2 MB file needs MANY packets...
```

---

## 🚚 **MTU and Fragmentation Path Concept**

```
Your Computer
  (MTU = 1500)
        ↓
ISP Gateway
  (MTU = 1500)
        ↓
Backbone Router
  (MTU = 9000 - Jumbo Frame)
        ↓
Another ISP
  (MTU = 1500)
        ↓
Destination
  (MTU = 1500)
```

The **bottleneck** is the smallest MTU = 1500 bytes.

If any link has a smaller MTU, the packet might need to be fragmented.

---

## ⚙️ **MTU Structure in Detail**

### Typical Ethernet MTU (1500 bytes)

```
┌──────────────────────────────────────────┐
│   Ethernet Frame Total: 1518 bytes       │
├──────────────────────────────────────────┤
│ Preamble & Delimiter: 8 bytes            │
├──────────────────────────────────────────┤
│ Destination MAC: 6 bytes                 │
│ Source MAC: 6 bytes                      │
│ Type/Length: 2 bytes                     │
│ ────────────────────────────────────── │
│ (HEADER TOTAL: 14 bytes)                 │
├──────────────────────────────────────────┤
│ PAYLOAD: 1500 bytes (This is the MTU)   │
│   ├─ IP Header: 20 bytes                │
│   ├─ TCP/UDP Header: 20 bytes           │
│   └─ Application Data: 1460 bytes       │
├──────────────────────────────────────────┤
│ FCS (Frame Check Sequence): 4 bytes      │
└──────────────────────────────────────────┘

Total: 1518 bytes
But MTU = 1500 (payload size only)
```

---

## 🧠 **TCP MSS (Maximum Segment Size)**

When TCP establishes a connection (3-way handshake), it negotiates the MSS:

```
TCP MSS = MTU - IP Header - TCP Header

Example:
MTU = 1500 bytes
  Minus IP Header = 20 bytes
  Minus TCP Header = 20 bytes
  = 1460 bytes (MSS)

So TCP will send data in 1460-byte chunks
```

### Why This Matters

```
If MTU = 1500 and TCP doesn't account for headers:
  → Might create 1500-byte TCP payload
  → Plus IP header (20 bytes)
  → Plus TCP header (20 bytes)
  → Total: 1540 bytes
  → Exceeds link capacity!
  → Packet gets fragmented
  → Performance degrades

So TCP deliberately limits payload to ~1460 bytes
This fits nicely in 1500-byte MTU
```

---

## 📱 **Different Network Scenarios**

### Home Network (Ethernet/Wi-Fi)
```
Your Device → Router → ISP → Internet
MTU: 1500    MTU: 1500  MTU: 1500  MTU: 1500
                    (bottleneck = 1500)
```
**Result:** Packets up to 1500 bytes travel without fragmentation

### Mobile Network
```
Your Phone → 4G Tower → Core Network → Internet
MTU: 1500   MTU: 1500    MTU: 1500     MTU: 1500
                     (but might vary!)
```
**Result:** Sometimes different, can cause issues

### Submarine Cables
```
Data Center → Submarine Cable → Data Center
MTU: 1500      MTU: 4608        MTU: 1500
                        (large MTU!)
```
**Result:** Jumbo frames on cable, then break down at land

---

## ⚠️ **MTU Discovery**

Hosts need to know the path's MTU to avoid fragmentation:

### Method 1: ICMP Path MTU Discovery (RFC 1191)
```
1. Sender starts with link MTU (1500)
2. Sends packet with "Don't Fragment" bit set
3. If router gets too-large packet:
   → Sends ICMP "Fragmentation Needed" error
   → Tells sender the MTU of next link
4. Sender reduces MTU and retries
5. Continues until packet successfully reaches destination
```

### Method 2: Jumbo Frames
```
Some networks use larger MTU (9000 bytes)
More efficient for high-speed data centers
Reduces packet overhead
But must support end-to-end
```

---

## 🔥 **Why MTU Matters in Practice**

### Scenario 1: Video Streaming
```
Stream data comes in frames
Each frame might be 2-5 MB
Must be split into MTU-sized packets
MTU=1500 → ~1300 packets per frame
MTU=9000 → ~200 packets per frame
Smaller MTU = more packets = more overhead
```

### Scenario 2: File Transfer
```
100 MB file transfer
MTU = 1500 bytes

Packets needed = 100,000,000 / 1460 ≈ 68,500 packets
Each packet has 40 bytes of header overhead
Total overhead = 68,500 × 40 = 2.74 MB wasted just on headers
```

### Scenario 3: VPN Over Internet
```
Normal packet: 1500 bytes
VPN adds encryption: +30 bytes
Total: 1530 bytes
            ↓
Exceeds link MTU (1500)
            ↓
Gets fragmented (bad for performance)
            ↓
Solution: Reduce VPN packet size or use Path MTU Discovery
```

---

## 🎯 **MTU and Fragmentation Relationship**

```
IP Packet Size  |  MTU  | What Happens
────────────────┼───────┼──────────────────────
500 bytes       | 1500  | ✅ Sent as-is
1500 bytes      | 1500  | ✅ Sent as-is  
1501 bytes      | 1500  | ⚠️ Fragmented into 2
2000 bytes      | 1500  | ⚠️ Fragmented into 2
3000 bytes      | 1500  | ⚠️ Fragmented into 2-3
```

---

## 💡 **Key Insight**

MTU is a **link property**, not a packet property.

```
Same packet can travel:
  → 1500-byte MTU link (fits perfectly)
  → 1200-byte MTU link (needs fragmentation)
  → 9000-byte MTU link (uses only small part)

The packet size doesn't change
But how it's handled depends on the link MTU
```

---

## 🧠 **Simple Rule**

> **If packet size ≤ MTU → Send in one frame**  
> **If packet size > MTU → Fragment into multiple frames**

Fragmentation causes:
- ✅ More processing
- ✅ More packets sent
- ✅ More potential for packet loss
- ✅ Slower performance

**Best practice:** Design packets to fit MTU to avoid fragmentation

---

## 🎓 **Remember**

**MTU = Maximum Transmission Unit = size limit of the link**

Key values:
- **1500 bytes** = standard Ethernet (most common)
- **9000 bytes** = Jumbo Frames (data centers)
- **65535 bytes** = Loopback (theoretical max)

**Related Concept:**
- **MSS = Maximum Segment Size** = application-level data limit
- MSS ≈ MTU minus headers ≈ 1460 bytes (typical)

**What happens when packet exceeds MTU?**  
→ **Fragmentation** (next topic)