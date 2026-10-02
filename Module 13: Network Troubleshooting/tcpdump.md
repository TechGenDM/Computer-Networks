# 🐙 `tcpdump` — Packet Capture

Now we're getting into **real packet-level troubleshooting**.

So far:

```text
ping       → reachability
traceroute → path
nslookup   → DNS
dig        → detailed DNS
netstat    → connections
ss         → sockets
```

Now:

> **`tcpdump` lets you capture and inspect packets that are actually passing through a network interface.**

This is a big step up.

---

# 1. What is `tcpdump`?

`tcpdump` is a **command-line packet analyzer**.

It captures packets from a network interface and displays information about them.

Think:

```text
Application
    ↓
TCP / UDP
    ↓
IP
    ↓
Ethernet / Wi-Fi
    ↓
🐙 tcpdump observes packets
```

It doesn't normally sit in the middle and modify traffic.

It **captures/observes** traffic visible to the interface and applies filters to show what you care about.

---

# 2. Basic Command

On macOS/Linux:

```bash
sudo tcpdump
```

You may see output like:

```text
08:21:31.123456 IP 192.168.1.10.53021 > 142.250.x.x.443:
Flags [S], seq 123456, win 65535, length 0
```

That one line already contains useful networking information.

---

# 3. Which Interface Should We Capture?

A machine may have multiple interfaces:

```text
Wi-Fi
Ethernet
Loopback
VPN
```

First, find available interfaces.

On macOS/Linux:

```bash
tcpdump -D
```

You might see something like:

```text
1.en0
2.lo0
3.awdl0
...
```

On a Mac, `en0` is commonly the Wi-Fi interface, though interface names can vary.

Then:

```bash
sudo tcpdump -i en0
```

means:

> **Capture packets on interface `en0`.**

---

# 4. Why `sudo`?

Packet capture often requires elevated privileges because you're asking the operating system to provide low-level network traffic to the capture process.

So you'll commonly use:

```bash
sudo tcpdump ...
```

---

# 5. Understanding a Packet

Consider:

```text
IP 192.168.1.10.53021 > 142.250.x.x.443:
Flags [S], seq 123456, win 65535, length 0
```

Let's break it down.

### Source

```text
192.168.1.10.53021
```

Your machine:

```text
IP   = 192.168.1.10
Port = 53021
```

### Destination

```text
142.250.x.x.443
```

Remote endpoint:

```text
IP   = 142.250.x.x
Port = 443
```

So:

```text
192.168.1.10:53021
        ↓
142.250.x.x:443
```

This directly connects to the **IP + port** concepts you learned earlier.

---

# 6. `Flags [S]` 🔗

The:

```text
[S]
```

means:

> **SYN**

So this packet is likely the first packet of a TCP three-way handshake.

You might then capture:

```text
[S]
[S.]
[.]
```

Conceptually:

```text
Client → SYN
Server → SYN + ACK
Client → ACK
```

🔥 This is a beautiful example of seeing your earlier TCP theory in an actual packet capture.

---

# 7. Capture Only a Few Packets

You usually don't want to stare at an infinite stream.

Use:

```bash
sudo tcpdump -i en0 -c 10
```

`-c 10` means:

> Stop after capturing 10 packets.

So:

```text
-i → interface
-c → packet count
```

---

# 8. Don't Perform DNS Lookups

A very useful option:

```bash
sudo tcpdump -i en0 -n
```

`-n` prevents tcpdump from trying to resolve addresses into hostnames.

This is useful because:

```text
✅ Faster
✅ Cleaner output
✅ Avoids extra DNS lookups
```

A common command:

```bash
sudo tcpdump -i en0 -n
```

---

# 9. Capture HTTP/HTTPS Traffic by Port

Suppose you want to inspect HTTPS traffic:

```bash
sudo tcpdump -i en0 -n port 443
```

This means:

> Capture traffic involving port 443.

For HTTP:

```bash
sudo tcpdump -i en0 -n port 80
```

---

# 10. Capture TCP Only

```bash
sudo tcpdump -i en0 -n tcp
```

Only TCP packets.

UDP:

```bash
sudo tcpdump -i en0 -n udp
```

ICMP:

```bash
sudo tcpdump -i en0 -n icmp
```

This is particularly useful for watching:

```bash
ping 8.8.8.8
```

while another terminal runs:

```bash
sudo tcpdump -i en0 -n icmp
```

You can actually see the ICMP request/reply packets.

---

# 11. Capture Traffic to/from a Host

For example:

```bash
sudo tcpdump -i en0 -n host 8.8.8.8
```

This means:

> Show packets to or from `8.8.8.8`.

You can make the filter more specific.

For example:

```bash
sudo tcpdump -i en0 -n src host 192.168.1.10
```

Only packets whose source is that host.

Or:

```bash
sudo tcpdump -i en0 -n dst host 8.8.8.8
```

Only packets whose destination is that host.

---

# 12. Capture DNS Traffic

DNS commonly uses port 53:

```bash
sudo tcpdump -i en0 -n port 53
```

Then in another terminal:

```bash
nslookup example.com
```

You may observe DNS packets being generated.

This connects:

```text
nslookup
   ↓
DNS
   ↓
UDP/TCP
   ↓
tcpdump captures packets
```

---

# 13. Capture a TCP Handshake

This is a great learning exercise.

Run:

```bash
sudo tcpdump -i en0 -n 'tcp port 443'
```

Then establish a new HTTPS connection.

You may observe packets resembling:

```text
Client → Server   [S]
Server → Client   [S.]
Client → Server   [.]
```

These correspond to:

```text
SYN
SYN-ACK
ACK
```

Then application traffic begins.

With HTTPS, that traffic is encrypted.

---

# 14. Why Can't `tcpdump` Show My HTTPS Password?

Very important.

If you capture:

```text
HTTPS → TCP → IP
```

you can still see metadata such as:

```text
Source IP
Destination IP
Source port
Destination port
TCP flags
Packet sizes
Timing
```

But the HTTP payload is protected by **TLS encryption**.

So you generally won't see:

```text
password=123456
```

inside ordinary HTTPS packet output.

Instead, you'll see encrypted TLS records.

This is a fundamental reason HTTPS provides confidentiality.

---

# 15. Saving a Packet Capture 📁

You don't have to inspect everything immediately.

You can save packets to a `.pcap` file:

```bash
sudo tcpdump -i en0 -w capture.pcap
```

Now packets are written to:

```text
capture.pcap
```

This file can later be opened in:

> **Wireshark**

That's exactly why the next topics fit together so well:

```text
tcpdump
   ↓
capture packets
   ↓
.pcap
   ↓
Wireshark
   ↓
deep packet analysis
```

---

# 16. Reading a Simple Example

Imagine tcpdump shows:

```text
IP 192.168.1.10.53021 > 142.250.x.x.443:
Flags [S], seq 1000, win 65535, length 0
```

Interpretation:

```text
Protocol → TCP over IPv4
Source   → 192.168.1.10:53021
Dest     → 142.250.x.x:443
Flag     → SYN
Seq      → 1000
Data     → 0 bytes
```

That's a TCP connection attempt.

Then:

```text
IP 142.250.x.x.443 > 192.168.1.10.53021:
Flags [S.], seq 5000, ack 1001
```

That's:

```text
SYN + ACK
```

Then:

```text
IP 192.168.1.10.53021 > 142.250.x.x.443:
Flags [.], ack 5001
```

That's:

```text
ACK
```

You've just observed the **three-way handshake in actual traffic**. 🔥

---

# 17. Filters Are the Superpower

Without filtering:

```bash
sudo tcpdump -i en0
```

you could get a huge amount of traffic:

```text
DNS
TCP
UDP
ARP
IPv6
mDNS
QUIC
...
```

Instead:

```bash
sudo tcpdump -i en0 -n port 443
```

you focus on HTTPS-related traffic.

Or:

```bash
sudo tcpdump -i en0 -n icmp
```

focus on ping traffic.

Or:

```bash
sudo tcpdump -i en0 -n host 192.168.1.1
```

focus on your router.

Filtering is one of the most important skills when using tcpdump.

---

# 18. `tcpdump` vs Wireshark

This is worth understanding now.

### `tcpdump`

```text
Command line
Fast
Lightweight
Great for servers
Excellent filtering
```

### Wireshark

```text
Graphical
Deep packet decoding
Protocol analysis
Packet-by-packet inspection
```

Think:

```text
tcpdump  → capture/filter quickly
Wireshark → investigate deeply
```

Neither is "better" for every situation.

---

# 🧠 Best Mental Model

Think of `tcpdump` as putting a **network camera** on an interface:

```text
                  Network Traffic
                        ↓
                ┌───────────────┐
                │ Network       │
                │ Interface     │
                └───────┬───────┘
                        │
                    📹 tcpdump
                        │
                        ↓
                Packet information
```

You're watching what is actually crossing that interface.

---

# 🔥 Must Remember

### Basic capture

```bash
sudo tcpdump -i en0
```

### Numeric output

```bash
sudo tcpdump -i en0 -n
```

### Capture 10 packets

```bash
sudo tcpdump -i en0 -c 10
```

### TCP

```bash
sudo tcpdump -i en0 -n tcp
```

### UDP

```bash
sudo tcpdump -i en0 -n udp
```

### ICMP / ping

```bash
sudo tcpdump -i en0 -n icmp
```

### HTTPS

```bash
sudo tcpdump -i en0 -n port 443
```

### DNS

```bash
sudo tcpdump -i en0 -n port 53
```

### Save to `.pcap`

```bash
sudo tcpdump -i en0 -w capture.pcap
```

---

## 🎯 Key concept

> **`tcpdump` captures packets at a network interface and lets you filter and inspect their headers, protocols, addresses, ports, flags, and other metadata.**

And this is the really important progression you've reached:

```text
Theory
  ↓
TCP handshake
  ↓
Actual packets
  ↓
tcpdump
  ↓
Packet capture
  ↓
Wireshark
  ↓
Deep analysis
```

---

## Module 13 Progress

✅ `ping` 🏓  
✅ `traceroute` 🗺️  
✅ `nslookup` 🔎  
✅ `dig` 🧪  
✅ `netstat` 📊  
✅ `ss` 🔌  
✅ **`tcpdump` 🐙**  
⬜ Wireshark  
⬜ Reading Packet Captures  
⬜ Common Networking Problems

**Next → Wireshark 🦈** — we'll move from the terminal to a graphical packet analyzer and learn how to inspect individual **Ethernet → IP → TCP → TLS/HTTP** layers inside a captured packet.