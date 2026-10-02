<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 13: Network Troubleshooting* • **Topic 10 of 10** (Global #099)

| [⬅️ Previous: 🔬 Reading Packet Captures](./09_Reading_Packet_Captures.md) | 📑 [**Module Overview**](./README.md) | [Next Module: 🧭 Topic 1 — Browser Cache ➡️](../Module%2014%3A%20Complete%20Internet%20Journey%20%2B%20Revision/01_Browser_Cache.md) |
| :--- | :---: | ---: |

---

</div>

# 🛠️ Common Networking Problems

This is the **final topic of Module 13**, and honestly, this is where all the concepts you've learned become useful in real life.

A good network troubleshooter doesn't randomly try things.

They ask:

> **"At which layer is the communication failing?"**

---

# 1. The Golden Troubleshooting Method 🧠

When something isn't working, troubleshoot from **near → far**:

```text
Your application
      ↓
Transport
      ↓
IP connectivity
      ↓
Local network
      ↓
Gateway
      ↓
Internet
      ↓
DNS / remote service
```

A practical sequence:

```text
1. Check interface/Wi-Fi
2. Check IP configuration
3. Check gateway
4. Check remote IP
5. Check DNS
6. Check port/service
7. Inspect packets if necessary
```

This prevents guessing.

---

# 2. Problem: "Wi-Fi is connected, but Internet doesn't work" 📶❌

This is extremely common.

Your laptop may show:

```text
✅ Connected to Wi-Fi
```

but:

```text
❌ Websites don't open
```

Don't assume the Internet itself is down.

Test progressively.

### Step 1

Check your IP configuration.

You should have something like:

```text
IP      → 192.168.1.10
Gateway → 192.168.1.1
DNS     → 192.168.1.1
```

If you have no valid IP address, DHCP may be the problem.

### Step 2

Ping the gateway:

```bash
ping 192.168.1.1
```

If it fails:

```text
Laptop
   ❌
Router
```

Potential issues:

```text
Wi-Fi
Ethernet
DHCP/configuration
Local network
Router
```

### Step 3

Ping a remote IP:

```bash
ping 8.8.8.8
```

If this works:

```text
Local network ✅
Internet IP connectivity ✅
```

but:

```bash
ping google.com
```

fails, DNS becomes a strong suspect.

---

# 3. Problem: DNS isn't working 🌐❌

Symptom:

```text
ping 8.8.8.8
✅

ping google.com
❌
```

This is a classic clue.

The network can reach an IP address, but hostname resolution may be failing.

Test:

```bash
nslookup google.com
```

or:

```bash
dig google.com
```

You can also test another resolver:

```bash
nslookup google.com 8.8.8.8
```

If your normal resolver fails but another resolver works, the problem may be with your configured/local DNS resolver path.

Mental model:

```text
IP works ✅
DNS fails ❌
      ↓
Check DNS
```

---

# 4. Problem: `Connection refused` 🚫

Suppose you open:

```text
localhost:8000
```

and get:

```text
Connection refused
```

This usually means the connection reached the host, but **nothing is accepting the connection on that port**, or the host actively rejected it.

Check:

```bash
ss -tlnp
```

or on macOS:

```bash
netstat -an | grep 8000
```

If you don't see a listener:

```text
No service listening on 8000
```

Possible causes:

```text
Backend isn't running
Wrong port
Application crashed
Server bound to another address
```

---

# 5. Problem: Port is blocked / connection times out ⏱️

Suppose:

```text
Connection timed out
```

This can indicate that packets or replies aren't getting through.

Possible causes include:

```text
Firewall
Routing issue
Security group
Network filtering
Server unavailable
```

Tools that help:

```text
ping
traceroute
ss
tcpdump
Wireshark
```

A timeout is different from a refusal.

### Refused

```text
"Something actively rejected the connection."
```

### Timeout

```text
"I didn't get the expected response in time."
```

---

# 6. Problem: High Latency 🐢

Suppose:

```bash
ping example.com
```

shows:

```text
time = 250 ms
```

when you normally expect much less.

First determine **where the latency appears**.

Use:

```bash
traceroute example.com
```

You might see:

```text
1   2 ms
2   5 ms
3   8 ms
4  120 ms
5  125 ms
```

This provides a clue that the path changes significantly around that point.

But don't automatically blame the first hop that shows a high number—routers can deprioritize or rate-limit traceroute responses. Look at whether the increased delay **continues for subsequent responsive hops**.

---

# 7. Problem: Packet Loss 📉

Suppose:

```bash
ping example.com
```

shows:

```text
20 packets transmitted
18 received
10% packet loss
```

Possible causes:

```text
Weak Wi-Fi
Congestion
Faulty network equipment
Wireless interference
Routing problems
Remote filtering
```

Again, don't immediately conclude:

> "The Internet is broken."

Test closer to you first.

```text
Laptop
  ↓
Gateway
  ↓
ISP
  ↓
Internet
```

If the gateway itself shows packet loss, focus on the local network.

If the gateway is clean but remote destinations lose packets, investigate farther out.

---

# 8. Problem: Website Doesn't Load, but Ping Works

Suppose:

```bash
ping example.com
```

✅

but the website doesn't load.

That's completely possible.

Why?

Because:

```text
ping → ICMP
website → HTTPS/TCP/TLS
```

The server could allow ICMP while:

```text
TCP port 443
```

is unavailable or the application/TLS layer has a problem.

So:

> **Ping success does not prove that a website or service is working.**

This is a very important troubleshooting lesson.

---

# 9. Problem: DNS Works, but TCP Doesn't

Imagine:

```text
dig example.com
✅

TCP connection
❌
```

Then:

```text
DNS ✅
IP resolution ✅
Transport connection ❌
```

Possible causes:

```text
Firewall
Port closed
Server not listening
Routing problem
Service outage
```

Now inspect TCP with:

```text
tcpdump
```

or:

```text
Wireshark
```

Look for:

```text
SYN
SYN-ACK
ACK
```

If you see:

```text
SYN
SYN
SYN
...
```

with no successful response, the TCP connection isn't completing.

---

# 10. Problem: Slow Website 🐌

A slow website could have problems at many layers.

Possibilities:

```text
DNS resolution slow
↓
TCP connection slow
↓
TLS handshake slow
↓
Server response slow
↓
Large content transfer
↓
Client rendering slow
```

This is why packet captures are powerful.

You can break the problem into:

```text
DNS
 ↓
TCP
 ↓
TLS
 ↓
HTTP/application
```

and identify where time is being spent.

---

# 11. Problem: Wrong IP Configuration

Suppose your laptop gets:

```text
IP:
169.254.x.x
```

This is a major clue on many IPv4 hosts.

It commonly indicates that the device assigned itself a **link-local IPv4 address** because it did not successfully obtain a DHCP address.

So investigate:

```text
DHCP
 ↓
Router
 ↓
Local network
```

rather than immediately troubleshooting Internet routing.

---

# 12. Problem: Wrong Default Gateway

Suppose:

```text
IP      = 192.168.1.10
Gateway = incorrect/unreachable
```

You might have:

```text
✅ Some local connectivity
❌ Internet access
```

Check the routing table.

On macOS/Linux, tools such as:

```bash
netstat -rn
```

can help.

On Linux, you can also use:

```bash
ip route
```

You want to see an appropriate default route, conceptually:

```text
default via 192.168.1.1
```

---

# 13. Problem: ARP Issue 🔎

Suppose you're trying to reach your gateway:

```bash
ping 192.168.1.1
```

but it doesn't work.

If your IP configuration looks correct, an ARP problem could be relevant.

You can inspect the local ARP/neighbor information using OS-specific commands and capture traffic with Wireshark/tcpdump.

You may see:

```text
ARP Request:
Who has 192.168.1.1?

        ↓

No ARP Reply
```

Then your machine can't obtain the gateway's MAC address through ARP.

---

# 14. Problem: TCP Retransmissions

In Wireshark you might see:

```text
TCP Retransmission
```

This means TCP retransmitted data.

Potential underlying causes include:

```text
Packet loss
Congestion
Wireless problems
Network path problems
Delayed/missing acknowledgements
```

Don't confuse:

```text
Retransmission
```

with:

```text
Root cause identified
```

It's evidence that TCP had to retransmit, not proof of exactly why.

---

# 15. A Complete Troubleshooting Example 🔥

Suppose your browser says:

> **"This site can't be reached."**

Don't panic.

Work through:

### ① Is your network interface connected?

```text
Wi-Fi / Ethernet
```

### ② Do you have an IP?

```text
192.168.1.x
```

### ③ Can you reach the gateway?

```bash
ping 192.168.1.1
```

### ④ Can you reach a remote IP?

```bash
ping 8.8.8.8
```

### ⑤ Does DNS work?

```bash
nslookup example.com
```

### ⑥ Can you establish the service connection?

For HTTPS:

```text
TCP → port 443
```

### ⑦ Can you inspect the packets?

```text
tcpdump / Wireshark
```

Now you're troubleshooting systematically rather than guessing.

---

# 16. The Troubleshooting Decision Tree 🌳

Keep this one.

```text
             Website not working
                     │
                     ↓
            Do I have an IP?
               /          \
             NO            YES
             ↓              ↓
           DHCP       Ping Gateway?
                         /       \
                       NO         YES
                       ↓           ↓
                   Local issue   Ping remote IP?
                                  /       \
                                NO         YES
                                ↓           ↓
                         Routing/path    DNS works?
                           problem        /    \
                                        NO      YES
                                        ↓        ↓
                                      DNS      Check
                                      issue    TCP/443
                                                 ↓
                                        Handshake works?
                                           /       \
                                         NO         YES
                                         ↓           ↓
                                      TCP/path    TLS/
                                      /firewall   application
```

This is the kind of thinking you want to develop.

---

# 🧠 The Troubleshooting Toolkit

You've now learned what each tool is for:

| Tool | Main purpose |
|---|---|
| `ping` | Reachability + RTT |
| `traceroute` | Path/hops |
| `nslookup` | DNS lookup |
| `dig` | Detailed DNS investigation |
| `netstat` | Connections/routing/statistics |
| `ss` | Socket/connection inspection |
| `tcpdump` | Command-line packet capture |
| Wireshark | Deep packet analysis |

The key is **using them together**.

---

# 🔥 The Most Important Mental Model

When something fails, locate the **first layer where it fails**.

```text
Application
    ↓
DNS
    ↓
TCP/UDP
    ↓
IP routing
    ↓
Gateway
    ↓
Local network
```

For example:

```text
Gateway ✅
Remote IP ✅
DNS ❌
```

→ likely DNS-related.

Or:

```text
DNS ✅
SYN sent
SYN-ACK never arrives
```

→ investigate connectivity, filtering, server reachability, or the return path.

Or:

```text
TCP ✅
TLS ❌
```

→ investigate TLS/security/certificate/server configuration rather than your basic IP connectivity.

---

# 🎯 Module 13 — COMPLETE! 🏆

✅ `ping`  
✅ `traceroute`  
✅ `nslookup`  
✅ `dig`  
✅ `netstat`  
✅ `ss`  
✅ `tcpdump`  
✅ Wireshark  
✅ Reading Packet Captures  
✅ **Common Networking Problems**

You've now learned not only **how networking works**, but also how to **observe and troubleshoot it**.

Your overall networking mental model is becoming:

```text
                         APPLICATION
                              │
                    HTTP / DNS / etc.
                              │
                         TCP / UDP
                              │
                           PORT
                              │
                             IP
                              │
                         ARP / NDP
                              │
                    Ethernet / Wi-Fi
                              │
                           SWITCH
                              │
                          ROUTER
                         /        \
                      LAN         WAN
                                   │
                                  ISP
                                   │
                               INTERNET
```

And when something breaks:

```text
Observe → Isolate → Test → Capture → Identify
```

That is the practical networking mindset. 💪

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What is an MTU black hole problem, and how does it manifest for end users?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> When a router drops packets that exceed MTU (with DF bit set) but its ICMP 'Fragmentation Needed' message is blocked by a firewall. Small packets (ping, handshakes) succeed, but large HTTP downloads hang indefinitely.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🔬 Reading Packet Captures](./09_Reading_Packet_Captures.md) | [**Module 13: Network Troubleshooting**](./README.md) | [Next Module: 🧭 Topic 1 — Browser Cache ➡️](../Module%2014%3A%20Complete%20Internet%20Journey%20%2B%20Revision/01_Browser_Cache.md) |

<div align="center">
  <br/>
  <code>[████████░░] 88% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
