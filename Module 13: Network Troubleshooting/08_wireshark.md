<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 13: Network Troubleshooting* • **Topic 08 of 10** (Global #097)

| [⬅️ Previous: 🐙 `tcpdump` — Packet Capture](./07_tcpdump.md) | 📑 [**Module Overview**](./README.md) | [Next: 🔬 Reading Packet Captures ➡️](./09_Reading_Packet_Captures.md) |
| :--- | :---: | ---: |

---

</div>

# 🦈 Wireshark — Packet Analysis

Now we move from **`tcpdump` (command line)** to **Wireshark (graphical packet analyzer)**.

This is where networking becomes very visual.

> **Wireshark captures and deeply analyzes network packets, allowing you to inspect protocols layer-by-layer.**

---

# 1. What is Wireshark?

Wireshark is a **network protocol analyzer**.

It can show packets like:

```text
Ethernet
   ↓
IPv4 / IPv6
   ↓
TCP / UDP / ICMP
   ↓
DNS / HTTP / TLS / QUIC
   ↓
Application data
```

Instead of just seeing:

```text
TCP 192.168.1.10:53021 → server:443
```

you can click the packet and inspect its individual protocol fields.

---

# 2. What does a captured packet look like?

Imagine you capture a TCP packet.

Wireshark might show:

```text
Frame
└── Ethernet II
    └── Internet Protocol Version 4
        └── Transmission Control Protocol
            └── TLS
```

Think of this as **encapsulation being reversed for inspection**.

Remember what you learned earlier:

```text
Application Data
     ↓
TCP
     ↓
IP
     ↓
Ethernet
```

Wireshark lets you inspect that structure from the outside inward.

---

# 3. The Three Main Wireshark Areas

The interface generally has three important sections.

### ① Packet List

Shows captured packets:

```text
No.   Time    Source       Destination    Protocol   Info
1     0.000   192.168.1.10 192.168.1.1     ARP        Who has...
2     0.120   192.168.1.10 93.x.x.x       TCP        SYN
3     0.135   93.x.x.x     192.168.1.10    TCP        SYN, ACK
```

---

### ② Packet Details

Click a packet and you'll see something like:

```text
Frame
Ethernet II
Internet Protocol Version 4
Transmission Control Protocol
```

You can expand each section.

---

### ③ Packet Bytes

At the bottom, Wireshark shows the raw bytes:

```text
00 1a 2b 3c ...
```

This is the actual binary data being analyzed.

---

# 4. Let's inspect a TCP packet 🔍

Suppose you capture:

```text
Client → Server
TCP SYN
```

Wireshark may show:

```text
Transmission Control Protocol
    Source Port: 53021
    Destination Port: 443
    Sequence Number: 1000
    Flags: SYN
    Window Size: ...
    Checksum: ...
```

Now you can directly see what we've learned theoretically.

```text
Source Port
Destination Port
Sequence Number
Flags
Window
Checksum
```

---

# 5. TCP Three-Way Handshake in Wireshark

This is one of the best things to practice.

Filter:

```text
tcp.flags.syn == 1
```

or use a specific connection.

You might observe:

```text
1  Client → Server  SYN
2  Server → Client  SYN, ACK
3  Client → Server  ACK
```

Exactly what you learned:

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
```

🔥 You're now seeing the theory in real packets.

---

# 6. Inspecting IP

Expand:

```text
Internet Protocol Version 4
```

You may see:

```text
Source Address:      192.168.1.10
Destination Address: 142.250.x.x
Time to Live:        64
Protocol:            TCP
Header Length:       20 bytes
```

This connects directly to:

```text
IP addressing
TTL
TCP
```

that you've already studied.

---

# 7. Inspecting Ethernet

Expand:

```text
Ethernet II
```

You may see:

```text
Destination: CC:CC:CC:CC:CC:CC
Source:      AA:AA:AA:AA:AA:AA
Type:        IPv4
```

Now you can visually observe:

```text
MAC → local-link delivery
IP  → network-layer addressing
```

This is exactly the distinction we discussed when learning ARP and routers.

---

# 8. Inspecting ARP

Capture ARP traffic and you'll see things such as:

```text
Address Resolution Protocol
    Sender MAC address
    Sender IP address
    Target MAC address
    Target IP address
```

An ARP request might essentially say:

```text
Who has 192.168.1.1?
```

and Wireshark lets you inspect the actual fields.

---

# 9. Inspecting DNS 🧪

Apply:

```text
dns
```

as a display filter.

You'll see DNS queries and responses.

For example:

```text
Standard query
Name: example.com
Type: A
```

and later:

```text
Standard query response
Name: example.com
Address: ...
```

Now the DNS concepts you learned—A records, answers, queries, etc.—become visible in actual traffic.

---

# 10. Inspecting HTTP

If you're dealing with unencrypted HTTP traffic:

```text
http
```

Wireshark can dissect things such as:

```text
GET /products HTTP/1.1
Host: example.com
```

and:

```text
HTTP/1.1 200 OK
Content-Type: text/html
```

So you can literally see:

```text
HTTP Request 📤
      ↓
HTTP Response 📥
```

in captured packets.

---

# 11. Why HTTPS Looks Different 🔒

If you're accessing:

```text
https://example.com
```

you'll generally see TLS-encrypted traffic rather than readable HTTP payloads.

Wireshark might show:

```text
TLS
    Client Hello
    Server Hello
    ...
    Application Data
```

You can still inspect important metadata such as:

```text
IP addresses
ports
packet sizes
timing
TCP flags
TLS records
```

But the encrypted HTTP contents aren't simply exposed as readable text.

This is one of the clearest demonstrations of what **TLS encryption** accomplishes.

---

# 12. Display Filters — Wireshark's Superpower 🔥

Wireshark can capture huge amounts of traffic.

Filters let you focus.

### TCP

```text
tcp
```

### UDP

```text
udp
```

### DNS

```text
dns
```

### HTTP

```text
http
```

### ICMP

```text
icmp
```

### Specific IP

```text
ip.addr == 192.168.1.10
```

### Specific TCP port

```text
tcp.port == 443
```

### Traffic to a host

```text
ip.dst == 8.8.8.8
```

### SYN packets

```text
tcp.flags.syn == 1
```

These are **display filters**—they filter what Wireshark shows after/beside capture.

---

# 13. Capture Filter vs Display Filter

This is an important Wireshark distinction.

### Capture filter

Controls **what gets captured**.

For example:

```text
port 53
```

Meaning:

> Capture traffic involving DNS port 53.

### Display filter

Controls **what gets displayed** from captured packets.

For example:

```text
dns
```

Meaning:

> Show DNS packets.

Mental model:

```text
Capture filter
     ↓
What enters capture

Display filter
     ↓
What I see from capture
```

---

# 14. Follow a TCP Stream

One of Wireshark's incredibly useful features is:

> **Follow TCP Stream**

This lets you examine the data exchanged over a TCP connection when the payload is available and not encrypted.

Conceptually:

```text
Client → Server
Server → Client
Client → Server
...
```

Wireshark can reconstruct the conversation for you.

With HTTPS, the payload will generally remain encrypted unless you have the necessary TLS decryption information/configuration.

---

# 15. Packet Details vs Raw Bytes

Suppose Wireshark shows:

```text
Transmission Control Protocol
    Source Port: 53021
    Destination Port: 443
    Sequence Number: ...
    Flags: ACK
```

At the bottom, you can inspect the corresponding hexadecimal bytes.

This is useful because:

```text
Protocol analysis
        +
Raw bytes
        ↓
Understand exactly what is on the wire
```

You probably won't inspect raw bytes every day, but it's valuable when learning protocols deeply.

---

# 16. Wireshark vs tcpdump

You've now learned both.

| | `tcpdump` | Wireshark |
|---|---|---|
| Interface | Command line | GUI |
| Capture packets | ✅ | ✅ |
| Filtering | ✅ | ✅ |
| Protocol dissection | Good | ✅✅ |
| Visual analysis | ❌ | ✅ |
| Deep packet inspection | Good | ✅ |
| Server troubleshooting | Excellent | Excellent |

Mental model:

```text
tcpdump
→ "Capture/filter this quickly."

Wireshark
→ "Let's investigate exactly what happened."
```

They complement each other.

---

# 17. Practical Example — Debugging a Website

Suppose your browser can't access a website.

You can investigate:

### DNS

```text
dns
```

Did DNS resolve the domain?

### TCP

```text
tcp
```

Did the TCP connection establish?

Look for:

```text
SYN
SYN-ACK
ACK
```

### TLS

```text
tls
```

Did the TLS handshake progress?

### HTTP

```text
http
```

For plaintext HTTP, did you get:

```text
GET
200 OK
```

With HTTPS, inspect TLS rather than expecting readable HTTP payloads.

This gives you a troubleshooting chain:

```text
DNS
 ↓
TCP
 ↓
TLS
 ↓
HTTP/application
```

🔥 That's much more powerful than simply saying:

> "The website isn't working."

---

# 18. A Full Packet Stack

One of the most useful things to remember from Wireshark is:

```text
┌───────────────────────────┐
│ Application               │
│ DNS / HTTP / TLS / ...    │
├───────────────────────────┤
│ TCP / UDP                 │
├───────────────────────────┤
│ IP                        │
├───────────────────────────┤
│ Ethernet / Wi-Fi          │
└───────────────────────────┘
```

Wireshark can show you this hierarchy **inside one captured packet**.

---

# 🧠 Best Mental Model

Think of Wireshark as a **microscope for network traffic** 🔬.

`tcpdump` tells you:

```text
"Packet from A → B, TCP port 443."
```

Wireshark lets you ask:

```text
"What is inside the Ethernet header?"
"What is the IP TTL?"
"What TCP flags are set?"
"What is the sequence number?"
"Is this DNS?"
"Is this a SYN?"
"What's the TLS message?"
```

---

# 🔥 Must Remember

> **Wireshark is a graphical network protocol analyzer that captures and dissects packets layer-by-layer.**

Core skills:

```text
Capture traffic
     ↓
Find packets
     ↓
Apply display filters
     ↓
Expand protocol layers
     ↓
Inspect fields
     ↓
Understand the communication
```

Useful filters:

```text
tcp
udp
dns
http
icmp
ip.addr == 192.168.1.10
tcp.port == 443
tcp.flags.syn == 1
```

And remember:

```text
Capture Filter → controls what gets captured
Display Filter → controls what gets displayed
```

---

## Module 13 Progress

✅ `ping` 🏓  
✅ `traceroute` 🗺️  
✅ `nslookup` 🔎  
✅ `dig` 🧪  
✅ `netstat` 📊  
✅ `ss` 🔌  
✅ `tcpdump` 🐙  
✅ **Wireshark 🦈**

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What Wireshark display filter isolates only packets involved in a specific TCP connection port (e.g. port 443)?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> `tcp.port == 443`.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🐙 `tcpdump` — Packet Capture](./07_tcpdump.md) | [**Module 13: Network Troubleshooting**](./README.md) | [Next: 🔬 Reading Packet Captures ➡️](./09_Reading_Packet_Captures.md) |

<div align="center">
  <br/>
  <code>[████████░░] 86% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
