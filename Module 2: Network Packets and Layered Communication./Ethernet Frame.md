# 7️⃣ Ethernet Frame 🔗

## 🎯 **What Is An Ethernet Frame?**

**Ethernet Frame = the data unit used by Ethernet (Layer 2) to deliver data across a local network link.**

When you send data over your Wi-Fi or Ethernet cable, it's packaged in an **Ethernet frame**.

Think of it like a **shipping box for local delivery**:
- The frame carries an IP packet inside it
- It has a local delivery address (MAC address) on the outside
- It travels across ONE local link only

---

## 📦 **Ethernet Frame Structure**

```
┌──────────┬──────────┬────────┬──────────────┬──────────┐
│  Preamble│  Frame   │Dest MAC│  Src MAC     │ Type/Len │
│  (7B)    │ Delim(1B)│ (6B)   │   (6B)       │  (2B)    │
├──────────┴──────────┴────────┴──────────────┴──────────┤
│                                                          │
│                PAYLOAD (46-1500 bytes)                  │
│              [IP Packet + TCP + Data]                   │
│                                                          │
├─────────────────────────────────────────────────────────┤
│  FCS/CRC (Frame Check Sequence) - 4 bytes               │
└─────────────────────────────────────────────────────────┘

Minimum Frame: 64 bytes
Maximum Frame: 1518 bytes (standard Ethernet)
```

---

## 🔍 **Detailed Field Breakdown**

### 1️⃣ **Preamble (7 bytes)**
```
Purpose: Synchronization signal
Content: 10101010 10101010 10101010 10101010 10101010 10101010 10101010
Use: Tells receiver "a frame is coming, sync up"
Not counted in 1518 bytes
```

### 2️⃣ **Frame Delimiter (1 byte)**
```
Purpose: Marks the end of the preamble
Content: 10101011
Use: Signals start of actual frame data
```

### 3️⃣ **Destination MAC Address (6 bytes)**
```
Format: AA:BB:CC:DD:EE:FF
Where to deliver this frame?

Examples:
- 00:1A:2B:3C:4D:5E = specific device
- FF:FF:FF:FF:FF:FF = broadcast (all devices)
- 01:00:5E:XX:XX:XX = multicast (group of devices)

Important: Changes at each hop!
```

### 4️⃣ **Source MAC Address (6 bytes)**
```
Format: AA:BB:CC:DD:EE:FF
Who sent this frame?

Example:
- Your laptop MAC: A4:C3:61:78:92:D1

The receiver knows who sent the frame
so it can send a reply
```

### 5️⃣ **Type/Length Field (2 bytes)**
```
Purpose: Identify payload type

Common Values:
- 0x0800 = IPv4 packet
- 0x0806 = ARP (Address Resolution Protocol)
- 0x86DD = IPv6 packet
- 0x8100 = VLAN tagged

Example:
If Type = 0x0800, receiver knows the payload contains an IPv4 packet
```

### 6️⃣ **Payload (46-1500 bytes)**
```
The actual data being carried

Typically contains:
- IP Header (20 bytes)
- TCP/UDP Header (20 bytes)
- Application Data (variable)

Minimum 46 bytes (for small messages, padding added)
Maximum 1500 bytes (standard Ethernet MTU)
```

### 7️⃣ **FCS - Frame Check Sequence (4 bytes)**
```
Purpose: Error detection
Method: CRC-32 (Cyclic Redundancy Check)

How it works:
1. Sender calculates checksum of entire frame
2. Adds checksum at the end
3. Receiver recalculates checksum
4. If it matches, frame is OK
5. If it doesn't match, frame is corrupted → discard

Protects: Everything from Dest MAC to end of Payload
```

---

## 📊 **Complete Frame Example**

```
Real-world example: Sending "Hello" from 192.168.1.5 to 192.168.1.10

┌─────────────────────────────────────────────────────────┐
│ Preamble: 10101010... (sync signal)                     │
├─────────────────────────────────────────────────────────┤
│ Delimiter: 10101011                                     │
├─────────────────────────────────────────────────────────┤
│ Dest MAC: 00:1A:2B:3C:4D:5E  (receiver's MAC)           │
│ Src MAC:  A4:C3:61:78:92:D1  (sender's MAC)             │
│ Type: 0x0800  (IPv4)                                    │
├─────────────────────────────────────────────────────────┤
│ Payload (IP Packet with TCP Segment):                   │
│ ┌────────────────────────────────────────────────────┐  │
│ │ IP Header (20B):                                   │  │
│ │   Source IP: 192.168.1.5                          │  │
│ │   Dest IP: 192.168.1.10                           │  │
│ │   TTL: 64                                         │  │
│ │   Protocol: 6 (TCP)                               │  │
│ ├────────────────────────────────────────────────────┤  │
│ │ TCP Header (20B):                                  │  │
│ │   Source Port: 54321                              │  │
│ │   Dest Port: 80                                   │  │
│ │   Sequence: 1000                                  │  │
│ │   Flags: PSH, ACK                                 │  │
│ ├────────────────────────────────────────────────────┤  │
│ │ Application Data: "Hello" (5 bytes)               │  │
│ └────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────┤
│ FCS: 0x12345678  (checksum for error detection)        │
└─────────────────────────────────────────────────────────┘

Total Frame Size: ~90 bytes
```

---

## 🎯 **Key Concepts**

### MAC Addresses Are Local Only

```
Laptop (192.168.1.5)      Router          YouTube Server
   MAC: A1:B1:C1         (Gateway)        (8.8.8.8)
                          MAC: AA:BB      MAC: XX:YY

Step 1: Laptop to Router
┌────────────────────────┐
│ Dest MAC: AA:BB        │
│ Src MAC: A1:B1:C1      │
│ IP Dest: 8.8.8.8       │
│ IP Src: 192.168.1.5    │
└────────────────────────┘

Step 2: Router extracts frame, makes routing decision
and creates NEW frame for next link

Step 3: Router to ISP Gateway
┌────────────────────────┐
│ Dest MAC: YY:ZZ        │
│ Src MAC: AA:BB         │
│ IP Dest: 8.8.8.8       │  ← Same IP dest
│ IP Src: 192.168.1.5    │  ← Same IP src
└────────────────────────┘

The IP addresses NEVER change
The MAC addresses change at EVERY hop
```

### Broadcast Frames

When a device doesn't know the destination MAC:

```
Device A wants to talk to Device B
But doesn't know B's MAC address

Solution: Send BROADCAST frame
┌────────────────────────────────────────┐
│ Dest MAC: FF:FF:FF:FF:FF:FF (broadcast)│
│ Src MAC: A1:B1:C1                      │
│ Content: "Who has IP 192.168.1.10?"    │
└────────────────────────────────────────┘

All devices receive it
Device B recognizes its IP and replies
"I have that IP, my MAC is: BB:BB:BB"
```

This is the **ARP (Address Resolution Protocol)** in action.

---

## ⚡ **Frame Size Limits**

```
Maximum Payload: 1500 bytes (MTU - Maximum Transmission Unit)
Minimum Payload: 46 bytes (padding added if smaller)
Frame Overhead: 14 bytes header + 4 bytes trailer = 18 bytes
Maximum Total Frame: 1518 bytes
```

If an IP packet is larger than 1500 bytes:
- It must be **fragmented** into multiple frames
- OR the packet can't be delivered on that link

---

## 🔑 **Critical Insight: Two Different Addresses**

| Aspect | MAC Address | IP Address |
|--------|-------------|-----------|
| **Layer** | Layer 2 (Link) | Layer 3 (Network) |
| **Scope** | Local (this link only) | End-to-End |
| **Changes?** | YES, at every router hop | NO, from source to destination |
| **Used by** | Switches, local devices | Routers, Internet core |
| **Format** | 48 bits (AA:BB:CC:DD:EE:FF) | 32 bits (IPv4: 192.168.1.1) |
| **Purpose** | Local delivery | Global routing |

---

## 🧠 **How The Ethernet Frame Works**

```
1. Application creates data
        ↓
2. TCP wraps it (adds ports)
        ↓
3. IP wraps it (adds IP addresses)
        ↓
4. Ethernet wraps it (adds MAC addresses)
        ↓
5. Sender transmits the frame
        ↓
6. If destination is local:
   Receiver gets it directly (MAC matches)
        ↓
7. If destination is remote:
   Router intercepts it
   Extracts the IP packet
   Puts it in a NEW Ethernet frame
   (with NEW destination MAC)
   Forwards to next router
        ↓
8. This repeats until IP packet reaches destination
   At which point application receives the data
```

---

## 🚀 **Frame in Motion: Real Example**

```
You access YouTube from your laptop

1. Browser: "GET /home"
        ↓
2. TCP: [TCP Hdr | GET /home]
        ↓
3. IP: [IP Hdr | TCP | GET /home]
        ↓
4. Ethernet: [Eth Hdr | IP | TCP | GET /home | FCS]
        ↓
5. Sent to Router (local)
   ┌──────────────────────────────────────┐
   │ Dest MAC: Router's MAC               │
   │ Src MAC: Your Laptop's MAC           │
   │ IP Dest: 142.251.32.46 (YouTube)    │
   │ IP Src: 192.168.1.100 (You)         │
   └──────────────────────────────────────┘
        ↓
6. Router processes it, forwards to ISP
   ┌──────────────────────────────────────┐
   │ Dest MAC: ISP Gateway's MAC   ← NEW  │
   │ Src MAC: Router's MAC         ← NEW  │
   │ IP Dest: 142.251.32.46  (Same)       │
   │ IP Src: 192.168.1.100   (Same)       │
   └──────────────────────────────────────┘
        ↓
7. ISP gateway forwards toward backbone
   ┌──────────────────────────────────────┐
   │ Dest MAC: Core Router's MAC   ← NEW  │
   │ Src MAC: ISP Gateway's MAC    ← NEW  │
   │ IP Dest: 142.251.32.46  (Same)       │
   │ IP Src: 192.168.1.100   (Same)       │
   └──────────────────────────────────────┘
        ↓
8. Eventually reaches YouTube server
   Extracts the data
   Server sends response
   Process repeats in reverse
```

---

## 🎓 **Remember**

> **Ethernet Frame = Layer 2's way of moving data across ONE local link**

It says:
- "Where locally?" (Destination MAC)
- "From whom?" (Source MAC)
- "What's inside?" (Type field)
- "Is it OK?" (FCS checksum)

The frame changes at each router, but the IP packet inside stays the same.

**This is the foundation of how the Internet routing works!**