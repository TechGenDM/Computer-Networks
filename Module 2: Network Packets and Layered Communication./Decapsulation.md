# 3️⃣ Decapsulation 📦➡️

## 🎯 **Core Concept**

**Decapsulation = The receiver processes the packet layer-by-layer, UNWRAPPING and interpreting headers as it moves UP through the layers.**

We learned **encapsulation**: sender wraps data as it goes **DOWN**.

Now the receiver does the **opposite**: unwraps data as it goes **UP**.

---

## 🌐 **Complete Example: Laptop to Server**

### The Journey DOWN (Encapsulation)

Your laptop wants to send data:

```
Laptop ─ encapsulates ─→ bits ─ physical transmission ─→ Server
```

Step by step DOWN:

```
Application Layer: "GET /youtube"
        ↓ wrap
Transport Layer: [TCP Header | GET /youtube]
        ↓ wrap
Network Layer: [IP Header | TCP Header | GET /youtube]
        ↓ wrap
Link Layer: [Eth Header | IP Header | TCP Header | GET /youtube | Trailer]
        ↓ convert
Physical Layer: 101101010010...
        ↓ transmit over wire
```

---

### The Journey UP (Decapsulation)

Those bits arrive at the server.

The server now **unwraps** them:

```
Physical Layer
Receives: 101101010010...
        ↓
Link Layer (Data Link)
Receives: Ethernet Frame
Checks: Destination MAC, Frame integrity
Removes: Ethernet Header & Trailer
Passes UP: IP Packet
        ↓
Network Layer
Receives: IP Packet
Checks: Destination IP
Interprets: TTL, Protocol type
Removes: IP Header
Passes UP: TCP Segment
        ↓
Transport Layer
Receives: TCP Segment
Checks: Destination Port, Sequence numbers
Interprets: Connection state
Removes: TCP Header
Passes UP: Application Data
        ↓
Application Layer
Receives: "GET /youtube"
Processes the HTTP request
```

---

## 🧩 **Visual Decapsulation Process**

```
┌──────────────────────────────────────┐
│    Ethernet Frame (on the wire)      │
│ [Eth | IP | TCP | GET /youtube | ...]│
└──────────────────────────────────────┘
            ↓ remove Eth
┌──────────────────────────────────────┐
│      IP Packet (IP unwraps)          │
│    [IP | TCP | GET /youtube]         │
└──────────────────────────────────────┘
            ↓ remove IP
┌──────────────────────────────────────┐
│   TCP Segment (Transport unwraps)    │
│      [TCP | GET /youtube]            │
└──────────────────────────────────────┘
            ↓ remove TCP
┌──────────────────────────────────────┐
│  Application Data (finally!)         │
│         GET /youtube                 │
└──────────────────────────────────────┘
            ↓
┌──────────────────────────────────────┐
│   Web Server Processes the Request   │
│     Returns: HTML page content       │
└──────────────────────────────────────┘
```

---

## 🔍 **What Happens At Each Layer?**

### Layer 1: Physical Layer ⚡

```
Receives: Raw bits/electrical signals
┌────────────────────┐
│  101101010010...   │
└────────────────────┘

Action: Converts signals into a link-layer frame
Passes: Frame to Data Link Layer
```

---

### Layer 2: Data Link Layer (Ethernet) 🔗

```
Receives: Ethernet Frame
┌──────────────────────────────────────┐
│ Dest MAC | Src MAC | Type | Data ... │
└──────────────────────────────────────┘

Checks:
├─ Is this frame for me? (Destination MAC)
├─ Is the checksum valid? (Frame integrity)

Removes: Ethernet Header & Trailer
Passes UP: IP Packet (the payload)

Result:
┌───────────────────────┐
│  IP Packet            │
│  (without Eth wrapper)│
└───────────────────────┘
```

---

### Layer 3: Network Layer (IP) 🌍

```
Receives: IP Packet
┌──────────────────────────────────────┐
│ IP Header | TCP Header + Data ...    │
│ Src IP | Dst IP | TTL | Protocol ... │
└──────────────────────────────────────┘

Determines:
├─ Source IP address: Where did this come from?
├─ Destination IP: Is it for me or forward?
├─ TTL (Time To Live): Has it expired?
├─ Protocol: Is it TCP, UDP, or ICMP?

Removes: IP Header
Passes UP: Transport Layer Data (TCP Segment)

Result:
┌────────────────────────────┐
│  TCP Segment               │
│  (without IP wrapper)      │
└────────────────────────────┘
```

---

### Layer 4: Transport Layer (TCP/UDP) 🚚

```
Receives: TCP Segment
┌──────────────────────────────────────┐
│ TCP Header | Application Data ...    │
│ Src Port | Dst Port | Seq | Ack ...  │
└──────────────────────────────────────┘

Checks:
├─ Destination Port: Which application?
├─ Sequence Numbers: Is this in order?
├─ ACK numbers: Confirm receipt
├─ Flags: SYN? FIN? RST?

Actions:
├─ If TCP: ensure ordered delivery, handle retransmissions
├─ If UDP: just pass it up (fast, no guarantees)

Removes: TCP/UDP Header
Passes UP: Application Data

Result:
┌────────────────────────┐
│  "GET /youtube"        │
│  (pure application)    │
└────────────────────────┘
```

---

### Layer 5: Application Layer 🌐

```
Receives: Application Data
"GET /youtube"

Action: Interprets the data using the application protocol (HTTP)

Understands:
├─ This is an HTTP GET request
├─ User wants the /youtube resource
├─ Parse headers and request body

Result: Web server retrieves the page and sends back:
200 OK
[HTML content]
```

---

## 🎯 **Key Differences: Encapsulation vs Decapsulation**

| Step | Direction | Action |
|------|-----------|--------|
| **Encapsulation** | DOWN (Application → Physical) | **ADD** headers |
| **Decapsulation** | UP (Physical → Application) | **REMOVE** headers |

---

## 💡 **Important Concept: Each Layer Sees Different Data**

```
Perspective from different layers (incoming packet):

Application Layer sees:
  "GET /youtube"

Transport Layer sees:
  Port: 443 | "GET /youtube"

Network Layer sees:
  IP: 192.168.1.1 → 142.251.33.14 | Ports | "GET /youtube"

Data Link Layer sees:
  MAC: AA:BB:CC:DD:EE:FF → 11:22:33:44:55:66 | IPs | Ports | "GET /youtube"

Physical Layer sees:
  101101010010101...
```

**Each layer only cares about its own header!**

---

## 🧠 **Why Decapsulation is Essential**

Without decapsulation, the receiver couldn't:

✅ Know **where** the packet came from (IP address)  
✅ Know **which application** should process it (port)  
✅ Verify **if the packet was corrupted** (checksums)  
✅ Know **the correct order** of packets (sequence numbers)  
✅ Handle **retransmissions** if needed  

**Decapsulation = extracting meaningful information at each layer.**

---

## 🔑 **The Complete Picture**

```
SENDER                              RECEIVER
────────────────────────────────────────────────

Application Data                 Application Layer
    ↓ encapsulate                    ↑ decapsulate
Transport Layer                  Transport Layer
    ↓ encapsulate                    ↑ decapsulate
Network Layer                    Network Layer
    ↓ encapsulate                    ↑ decapsulate
Link Layer                       Link Layer
    ↓ encapsulate                    ↑ decapsulate
Physical Layer ──────→ BITS ──────→ Physical Layer
    (transmit)          (wire)        (receive)
```

**Communication is a dance of wrapping and unwrapping!**

---

## 🎓 **Remember**

> **Encapsulation = Sender wraps UP data (DOWN the stack)**  
> **Decapsulation = Receiver unwraps data (UP the stack)**

Together, they make modern networking possible!

For TCP, this can include:

- Source port
- Destination port
- Sequence number
- Acknowledgment information

Then the actual application data is passed upward.

---

### 5. Application Layer

Finally, the application receives the actual data.

For example:

```text
HTTP
GET /home
```

Your application can now process it.

---

# 🧠 The complete picture

```text
SENDER                         RECEIVER

Application                    Application
    ↓                              ↑
Transport                       Transport
    ↓                              ↑
Network                         Network
    ↓                              ↑
Data Link                      Data Link
    ↓                              ↑
Physical ───────────────────► Physical
       Encapsulation       Decapsulation
```

Or simply:

```text
SENDER                           RECEIVER

Data                             Data
 ↓                                ↑
TCP + Data                      TCP removed
 ↓                                ↑
IP + TCP + Data                IP removed
 ↓                                ↑
Ethernet + IP + TCP + Data    Ethernet processed
 ↓                                ↑
Bits ───────────────────────────► Bits
```

---

## 🔥 Important: Routers are different

A router doesn't normally decapsulate your entire packet all the way to the application.

For example:

```text
Laptop
   ↓
Ethernet Frame
   ↓
Router
```

The router processes the **link-layer information**, examines the **IP packet**, decides the next hop, and then sends the packet onward using a new link-layer frame.

Conceptually:

```text
Frame A
[Ethernet A | IP | TCP | Data]
             ↓
           Router
             ↓
Frame B
[Ethernet B | IP | TCP | Data]
```

Notice:

**Ethernet information changes from hop to hop.**

But the **IP packet generally travels end-to-end** (with some fields potentially changing, such as TTL/hop limit).

This distinction becomes **very important** when we study Frames vs Packets.

---

### 🎯 Remember

**Encapsulation:**

> Data → Headers added → Bits

**Decapsulation:**

> Bits → Headers processed/removed → Data

Next: **Headers & Payload** — this will make the packet structures much easier to understand.