# 4️⃣ Headers 📋

## 🎯 **Core Concept**

**Header = control/metadata information that tells the network how to deliver, route, and process the data.**

Think of a postal letter:

```
┌─────────────────────────┐
│  [Address & Postage]    │  ← HEADER (metadata)
│  ───────────────────    │
│  [Letter Content]       │  ← PAYLOAD (actual data)
│  ───────────────────    │
│  [Sender Details]       │
└─────────────────────────┘
```

The **address** tells the postal system how to deliver it.  
The **letter** is what the recipient actually wants to read.

Networking works the same way.

---

## 📦 **Header Structure**

Every packet/frame/segment has this basic structure:

```
┌──────────────────────────────────────┐
│          HEADER                      │
│  ┌────────────────────────────────┐  │
│  │ Control/Metadata Information   │  │
│  │ ├─ Addresses                   │  │
│  │ ├─ Protocols                   │  │
│  │ ├─ Sequence Numbers            │  │
│  │ └─ Error Checking              │  │
│  └────────────────────────────────┘  │
├──────────────────────────────────────┤
│           PAYLOAD                    │
│  (Actual data being carried)         │
└──────────────────────────────────────┘
```

---

## 🧩 **Headers at Different Layers**

Each layer adds its own header with specific information:

### Application Layer Header (HTTP)
```
GET /home HTTP/1.1
Host: youtube.com
User-Agent: Chrome
...
```
**Purpose:** Tell the web server what resource you want

---

### Transport Layer Header (TCP)
```
Source Port: 54321
Destination Port: 443
Sequence Number: 1234567
Acknowledgment: 9876543
Flags: [SYN, ACK]
Window Size: 65535
Checksum: 0x1A2B3C4D
Urgent Pointer: 0
```
**Purpose:** Reliable, ordered delivery between processes

---

### Network Layer Header (IPv4)
```
Version: 4
Header Length: 20 bytes
Type of Service: 0
Total Length: 1500 bytes
Identification: 0x1A2B
Flags: [Don't Fragment, More Fragments]
Fragment Offset: 0
TTL: 64
Protocol: 6 (TCP)
Checksum: 0x5A4B3C2D
Source IP: 192.168.1.100
Destination IP: 142.251.33.14
Options: (if any)
```
**Purpose:** Route packets across networks

---

### Link Layer Header (Ethernet)
```
Destination MAC: 00:1A:2B:3C:4D:5E
Source MAC: AA:BB:CC:DD:EE:FF
Type/Length: 0x0800 (IPv4)
Payload: [IP Packet]
Frame Check Sequence: 0x12345678
```
**Purpose:** Deliver frames across local network links

---

## 💡 **What Information Does Each Header Contain?**

### MAC Address Header (Link Layer)
```
Question: "Which device on THIS local network?"
Answers:  Destination MAC, Source MAC
Purpose:  Local network delivery only
Scope:    Changes at every hop!
```

### IP Header (Network Layer)
```
Question: "Where in the WORLD does this go?"
Answers:  Destination IP, Source IP
Purpose:  Route across the internet
Scope:    Stays the same from source to destination
```

### Port Header (Transport Layer)
```
Question: "Which APPLICATION/PROCESS?"
Answers:  Destination Port, Source Port
Purpose:  Identify which service (HTTP=443, SSH=22, DNS=53)
Scope:    Critical for multiplexing many apps
```

---

## 🔍 **Real Example: HTTP to Google**

When you type `google.com` into your browser:

```
Application Header:
  GET / HTTP/1.1
  Host: google.com
  Accept: text/html

Transport Header:
  Port 443 (HTTPS/TLS)
  Sequence: 1000

Network Header:
  Destination IP: 142.251.32.46 (Google's server)
  Source IP: 192.168.1.100 (Your laptop)
  TTL: 64

Link Header:
  Destination MAC: FF:FF:FF:FF:FF:FF (broadcast to find router)
  Source MAC: AA:BB:CC:DD:EE:FF (your laptop's MAC)
```

**Each header answers a specific question at that layer.**

---

## ⚙️ **TCP Header Fields in Detail**

```
TCP Header Format (20+ bytes):

┌──────────────────────────────────────┐
│   Source Port (16 bits) = 54321      │
├──────────────────────────────────────┤
│   Dest Port (16 bits) = 443          │
├──────────────────────────────────────┤
│   Sequence Number (32 bits)          │
│   = 1234567890                       │
├──────────────────────────────────────┤
│   Acknowledgment Number (32 bits)    │
│   = 9876543210                       │
├──────────────────────────────────────┤
│ Length | Reserved | Flags | Window   │
│   U | A | P | R | S | F              │
│   R | C | S | S | Y | I              │
│   G | K | H | T | N | N              │
├──────────────────────────────────────┤
│   Checksum (16 bits)                 │
├──────────────────────────────────────┤
│   Urgent Pointer (16 bits)           │
│   [Options, Padding if present]      │
└──────────────────────────────────────┘

Flags Meaning:
SYN = Synchronize (start connection)
ACK = Acknowledgment (confirm receipt)
FIN = Finish (end connection)
RST = Reset (abort connection)
PSH = Push (send data immediately)
URG = Urgent (urgent data follows)
```

---

## 🏗️ **IP Header Fields in Detail**

```
IPv4 Header Format (20 bytes minimum):

┌──────────────────────────────────────┐
│ Ver | HL | DSCP | ECN | Length       │
│  4  | 5  | 0    | 0   | 1500         │
├──────────────────────────────────────┤
│ Identification | Flags | Fragment    │
│  0x1A2B        | DF|MF | Offset      │
├──────────────────────────────────────┤
│ TTL (64) | Protocol (6=TCP) | Chk    │
├──────────────────────────────────────┤
│  Source IP: 192.168.1.100            │
├──────────────────────────────────────┤
│  Destination IP: 142.251.32.46       │
├──────────────────────────────────────┤
│  Options (if HL > 5)                 │
└──────────────────────────────────────┘

Key Fields Explained:
- TTL: Hop limit (decrement at each router)
- Protocol: 6=TCP, 17=UDP, 1=ICMP
- Checksum: Error detection for header only
- DF (Don't Fragment): Never split this packet
- MF (More Fragments): More fragments coming
- Fragment Offset: Position in original packet
```

---

## 🎯 **Why Headers Are NOT Wasteful**

Headers seem like "extra data," but they're **absolutely critical**:

```
✅ Routing: IP header tells routers WHERE to send it
✅ Port Mapping: TCP header tells OS which APP to give it to
✅ Error Detection: Checksums catch corruption
✅ Sequence Control: Numbers ensure ordered delivery
✅ Flow Control: Window size prevents overwhelming receiver
✅ Connection State: Flags manage connection lifecycle
✅ Priority: DSCP field prioritizes important traffic
✅ Looping Prevention: TTL prevents infinite routing loops
```

**Without headers, the network would be chaos!**

---

## 🧠 **Header vs Payload - The Key Distinction**

| Aspect | Header | Payload |
|--------|--------|---------|
| **Contains** | Metadata & Instructions | Actual user data |
| **Processed By** | Network devices (routers, switches) | End applications |
| **Changes?** | Some fields change at each hop | Never changes |
| **Size** | Typically 20-60 bytes | Can be very large |
| **Essential?** | YES - must be present | YES - purpose of packet |
| **Example** | TCP Port = 443 | "GET /home" |

---

## 🔑 **Remember**

> **Header = How to handle/deliver the data**  
> **Payload = The data itself**

Think of every data transmission as a letter:
- The **envelope** is the header (routing info)
- The **letter inside** is the payload (the message)

Without the envelope, your letter gets lost.  
Without the letter, there's nothing to deliver.

**Both are essential.**

---

## 📚 **Next Up**

We'll see how payloads can be:
- Simple data (text, images)
- Nested (another packet inside)
- Fragmented (split across multiple frames)

That's what makes networking fascinating!