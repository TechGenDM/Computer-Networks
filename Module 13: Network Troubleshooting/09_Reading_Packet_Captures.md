<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 13: Network Troubleshooting* • **Topic 09 of 10** (Global #098)

| [⬅️ Previous: 🦈 Wireshark — Packet Analysis](./08_wireshark.md) | 📑 [**Module Overview**](./README.md) | [Next: 🛠️ Common Networking Problems ➡️](./10_Common_Networking_Problems.md) |
| :--- | :---: | ---: |

---

</div>

# 🔬 Reading Packet Captures

This is where all the networking concepts you've learned start coming together.

Reading a packet capture isn't about memorizing every field. It's about learning to answer:

> **What happened? Where did it happen? And why?**

A good network engineer looks at a capture and reconstructs the communication.

---

# 1. Start from the Outside → Inside

When you click a packet in Wireshark, you'll usually see something like:

```text
Frame
└── Ethernet II
    └── IPv4
        └── TCP
            └── TLS
```

Think of it as:

```text
Ethernet
   ↓
IP
   ↓
TCP/UDP
   ↓
Application protocol
```

Each layer answers a different question.

| Layer | Main question |
|---|---|
| Ethernet | Which local device? |
| IP | Which source/destination host? |
| TCP/UDP | Which application endpoint? |
| Application | What is the actual communication? |

---

# 2. Let's Read One Packet

Suppose Wireshark shows:

```text
Ethernet II
    Src: AA:AA:AA:AA:AA:AA
    Dst: CC:CC:CC:CC:CC:CC

Internet Protocol Version 4
    Src: 192.168.1.10
    Dst: 142.250.x.x
    TTL: 64
    Protocol: TCP

Transmission Control Protocol
    Src Port: 53021
    Dst Port: 443
    Flags: SYN
    Seq: 1000
```

Don't read all of it at once.

Read it layer by layer.

---

# 3. Ethernet Layer 🔌

```text
Src MAC: AA:AA:AA:AA:AA:AA
Dst MAC: CC:CC:CC:CC:CC:CC
```

This tells you the **local-link addresses**.

If the destination is outside your LAN, `CC:CC...` may be the MAC address of your **default gateway**, not the remote server.

So you might interpret this as:

```text
Laptop
  ↓
Local Ethernet/Wi-Fi
  ↓
Router
```

---

# 4. IP Layer 🌍

Now look at:

```text
Source IP:
192.168.1.10

Destination IP:
142.250.x.x
```

This tells you:

```text
Who sent the packet?
        ↓
192.168.1.10

Where is the packet ultimately addressed?
        ↓
142.250.x.x
```

Notice the important difference:

```text
MAC destination → local next hop
IP destination  → network-layer destination
```

---

# 5. TTL

You might see:

```text
TTL: 64
```

Remember:

> **Routers decrement IP TTL by 1 when forwarding an IPv4 packet.**

So if a packet originated with TTL 64 and you capture it after several router hops, the observed TTL may be lower.

For example:

```text
Initial TTL ≈ 64
       ↓
Router 1 → 63
Router 2 → 62
Router 3 → 61
```

This is one reason TTL appears in tools like `ping` and packet captures.

---

# 6. Transport Layer — TCP

Now:

```text
Source Port:
53021

Destination Port:
443
```

This tells you:

```text
Your application endpoint
        ↓
192.168.1.10:53021

Remote service endpoint
        ↓
142.250.x.x:443
```

And:

```text
Flags: SYN
```

means this may be the beginning of a TCP connection.

---

# 7. Reconstruct the TCP Handshake

A single packet tells only part of the story.

To understand the connection, look for the sequence:

```text
1. SYN
2. SYN + ACK
3. ACK
```

For example:

```text
No.   Source → Destination       Flags
101   Client → Server            SYN
102   Server → Client            SYN, ACK
103   Client → Server            ACK
```

Now you can conclude:

> A TCP connection was successfully established.

That's much more useful than simply knowing that packet #101 had a SYN flag.

---

# 8. Follow the Sequence Numbers 🔢

Suppose:

```text
Packet 101:
SYN
Seq = 1000

Packet 102:
SYN-ACK
Seq = 5000
Ack = 1001

Packet 103:
ACK
Ack = 5001
```

You can reconstruct the handshake:

```text
Client ISN = 1000
Server ISN = 5000
```

and:

```text
Client ACKs server's SYN:
5000 + 1 = 5001
```

This connects directly with the TCP concepts you already learned.

---

# 9. Then Look at the Data

After the handshake:

```text
SYN
SYN-ACK
ACK
```

you may see:

```text
PSH, ACK
ACK
PSH, ACK
ACK
...
```

The exact flags depend on the traffic and TCP implementation.

The important idea is:

```text
Handshake
   ↓
Data transfer
```

---

# 10. Reading an HTTPS Connection 🔒

Suppose you open:

```text
https://example.com
```

A simplified capture might look like:

```text
DNS
 ↓
TCP SYN
 ↓
TCP SYN-ACK
 ↓
TCP ACK
 ↓
TLS Client Hello
 ↓
TLS Server Hello
 ↓
TLS handshake messages
 ↓
TLS Application Data
```

This gives you a **story** of what happened.

Notice:

```text
DNS
 ↓
TCP
 ↓
TLS
```

Each step belongs to a different protocol layer/purpose.

---

# 11. What About HTTP?

If the traffic is plaintext HTTP, you might literally see:

```text
GET /products HTTP/1.1
Host: example.com
```

followed by:

```text
HTTP/1.1 200 OK
Content-Type: text/html
```

Then you can reconstruct:

```text
Request
   ↓
Server response
   ↓
Status code
   ↓
Data
```

With HTTPS, HTTP is encrypted inside TLS, so Wireshark normally won't show the HTTP contents directly unless the traffic is appropriately decrypted.

---

# 12. Reading DNS Packets 🧪

Suppose you see:

```text
DNS Query
example.com
Type: A
```

Then:

```text
DNS Response
example.com
A
93.184.216.34
```

You can reconstruct:

```text
Client:
"What is example.com's IPv4 address?"

DNS:
"93.184.216.34"
```

Then you may see a TCP or QUIC connection to that address.

So your capture can reveal:

```text
DNS resolution
      ↓
Destination IP
      ↓
Transport connection
      ↓
Application communication
```

---

# 13. Reading ARP

You might see:

```text
ARP Request
Who has 192.168.1.1?
```

then:

```text
ARP Reply
192.168.1.1 is at CC:CC:CC:CC:CC:CC
```

Now you know:

```text
192.168.1.1
     ↓
CC:CC:CC:CC:CC:CC
```

This is exactly the ARP process you learned earlier.

---

# 14. Reading UDP

Suppose you see:

```text
UDP
Src Port: 53000
Dst Port: 53
```

You can infer:

```text
Client
   ↓
UDP
   ↓
DNS service
```

Unlike TCP, you won't see:

```text
SYN
SYN-ACK
ACK
```

because UDP doesn't establish a TCP-style connection.

---

# 15. Spotting Packet Loss or Retransmissions 🚨

This is where packet captures become very useful for troubleshooting.

Wireshark may flag something like:

```text
TCP Retransmission
```

Conceptually:

```text
Sender → Segment #10
         ❌ lost

Sender → Segment #10 again
```

That can indicate packet loss or another condition causing retransmission.

But be careful:

> A retransmission tells you TCP retransmitted data; it doesn't by itself prove exactly why the first transmission wasn't acknowledged.

---

# 16. Duplicate ACKs

Suppose you see:

```text
ACK 5000
ACK 5000
ACK 5000
```

while later data arrives.

This can be evidence that the receiver is repeatedly acknowledging the same contiguous byte boundary, potentially because something earlier is missing.

That can lead to:

```text
Fast Retransmit
```

This connects directly to the TCP reliability topic.

---

# 17. Reading TCP Close

At the end of a normal TCP session you may see:

```text
FIN
ACK
FIN
ACK
```

or a FIN/ACK combination depending on the implementation.

So from a capture you can reconstruct:

```text
Connection establishment
        ↓
Data transfer
        ↓
Connection termination
```

This is much more powerful than memorizing the handshake diagrams separately.

---

# 18. A Packet Capture Is a Story 📖

Suppose your capture contains:

```text
1  ARP Request
2  ARP Reply

3  DNS Query
4  DNS Response

5  TCP SYN
6  TCP SYN-ACK
7  TCP ACK

8  TLS Client Hello
9  TLS Server Hello
10 TLS Application Data
11 TLS Application Data

12 TCP FIN
13 TCP ACK
14 TCP FIN
15 TCP ACK
```

You can translate the entire thing into plain English:

```text
1. Device found its local gateway/MAC.
2. DNS resolved the domain.
3. TCP connection was established.
4. TLS security negotiation occurred.
5. Encrypted application data was exchanged.
6. TCP connection was gracefully closed.
```

🔥 **This is the skill of reading packet captures.**

---

# 19. How to Troubleshoot Using a Capture

Suppose:

> "My website isn't opening."

Don't randomly inspect thousands of packets.

Ask sequential questions.

### Question 1 — Did DNS work?

Look for:

```text
dns
```

If there's no response, investigate DNS.

### Question 2 — Did TCP connect?

Look for:

```text
SYN
SYN-ACK
ACK
```

If you get:

```text
SYN
SYN
SYN
...
```

with no successful handshake, connectivity or filtering may be involved.

### Question 3 — Did TLS work?

Look for:

```text
tls
```

### Question 4 — Did the application exchange data?

For plaintext HTTP:

```text
http
```

For HTTPS, inspect TLS/application-data behavior rather than expecting readable HTTP.

This gives you:

```text
DNS
 ↓
TCP
 ↓
TLS
 ↓
Application
```

---

# 20. Wireshark Expert Trick: Follow Conversations

When you identify a packet belonging to a connection, don't necessarily inspect packets randomly.

Use Wireshark's conversation/stream features to focus on that flow.

For TCP, **Follow TCP Stream** is particularly useful.

You can then reason about:

```text
Client ↔ Server
```

as one conversation rather than isolated packets.

---

# 21. Important Wireshark Fields to Learn

You don't need every field.

Focus on these first:

### Ethernet

```text
Source MAC
Destination MAC
EtherType
```

### IP

```text
Source IP
Destination IP
TTL / Hop Limit
Protocol
```

### TCP

```text
Source Port
Destination Port
Sequence Number
Acknowledgment Number
Flags
Window
```

### UDP

```text
Source Port
Destination Port
Length
Checksum
```

### DNS

```text
Query
Response
Name
Type
TTL
```

This is enough to understand a huge amount of traffic.

---

# 🧠 The 5-question method

Whenever you open a packet capture, ask:

```text
1. WHO?
   Source IP/MAC

2. WHERE?
   Destination IP/MAC

3. WHICH APPLICATION?
   Source/Destination port

4. WHAT PROTOCOL?
   TCP / UDP / ICMP / DNS / TLS / HTTP...

5. WHAT HAPPENED?
   SYN? ACK? Retransmission? DNS answer? Error?
```

Then look at neighboring packets to reconstruct the conversation.

---

# 🔥 Must Remember

Don't read a `.pcap` as a collection of isolated packets.

Read it as a **timeline**:

```text
ARP
 ↓
DNS
 ↓
TCP Handshake
 ↓
TLS / HTTP
 ↓
Data
 ↓
TCP Close
```

And use layers:

```text
Ethernet
   ↓
IP
   ↓
TCP / UDP
   ↓
Application
```

### The most important skill:

> **Use multiple packets together to reconstruct what happened, rather than trying to understand the entire network from one packet.**

---

## Module 13 Progress

✅ `ping` 🏓  
✅ `traceroute` 🗺️  
✅ `nslookup` 🔎  
✅ `dig` 🧪  
✅ `netstat` 📊  
✅ `ss` 🔌  
✅ `tcpdump` 🐙  
✅ Wireshark 🦈  
✅ **Reading Packet Captures 🔬**  
⬜ Common Networking Problems

We're at the **final topic of Module 13**:

**→ Common Networking Problems 🛠️**

There we'll take problems like **"Wi-Fi connected but no Internet," "DNS isn't working," "connection refused," "high latency," and "website loads slowly"** and build a systematic troubleshooting process for each one.

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *How do you identify a TCP retransmission in a packet capture?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> You see a segment with identical Sequence Numbers and payload data sent again after an elapsed time, often preceded by duplicate ACKs.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🦈 Wireshark — Packet Analysis](./08_wireshark.md) | [**Module 13: Network Troubleshooting**](./README.md) | [Next: 🛠️ Common Networking Problems ➡️](./10_Common_Networking_Problems.md) |

<div align="center">
  <br/>
  <code>[████████░░] 87% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
