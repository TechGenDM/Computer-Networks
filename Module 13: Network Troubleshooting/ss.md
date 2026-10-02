# 🔌 `ss` — Socket Statistics

Now we move to **`ss`**, which is primarily used on Linux to inspect **sockets and network connections**.

You can think of it as:

```text
netstat → traditional network inspection tool
ss      → modern Linux socket inspection tool
```

> **`ss` = Socket Statistics**

It is especially useful for seeing **listening ports, active TCP/UDP connections, and TCP states**.

---

# 1. The most useful command

The command you'll see everywhere is:

```bash
ss -tuln
```

Break it down:

```text
-t → TCP
-u → UDP
-l → listening
-n → numeric addresses/ports
```

So:

> **`ss -tuln` = show listening TCP/UDP sockets without DNS/service-name resolution.**

Example:

```text
Netid  State   Local Address:Port
tcp    LISTEN  0.0.0.0:22
tcp    LISTEN  127.0.0.1:3000
tcp    LISTEN  0.0.0.0:8000
udp    UNCONN  0.0.0.0:53
```

---

# 2. Understanding `LISTEN`

Suppose you see:

```text
tcp   LISTEN   127.0.0.1:8000
```

It means:

> A TCP service is listening for incoming connections on port `8000` on the loopback interface.

This connects directly to socket programming:

```text
socket()
 ↓
bind()
 ↓
listen()
```

`ss` lets you inspect that listening socket from the operating system.

---

# 3. `0.0.0.0` vs `127.0.0.1`

This is very important for backend development.

### `127.0.0.1:8000`

The service is listening on the **loopback interface**.

```text
Your machine
    ↓
127.0.0.1:8000
```

Generally, only the local machine can directly access it.

### `0.0.0.0:8000`

This means the service is listening on **all suitable local IPv4 interfaces**.

For example:

```text
Wi-Fi IP        → 192.168.1.10
Ethernet IP     → 192.168.1.20
Loopback        → 127.0.0.1

           ↓
     0.0.0.0:8000
```

The service can potentially accept connections arriving through those interfaces, subject to firewall/network configuration.

🔥 This is a very useful concept when deploying APIs.

---

# 4. Checking Only TCP

```bash
ss -t
```

Shows TCP sockets.

For listening TCP sockets:

```bash
ss -tl
```

For numeric output:

```bash
ss -tln
```

---

# 5. Checking Only UDP

```bash
ss -u
```

For listening UDP sockets:

```bash
ss -uln
```

Remember:

UDP doesn't have a TCP-style `LISTEN` state.

You may see something like:

```text
udp   UNCONN   0.0.0.0:53
```

because UDP is connectionless.

---

# 6. Viewing Established TCP Connections

Run:

```bash
ss -tn
```

You might see:

```text
State      Local Address:Port      Peer Address:Port
ESTAB      192.168.1.10:53021      142.250.x.x:443
```

This tells you:

```text
Local:
192.168.1.10:53021

Remote:
142.250.x.x:443

State:
ESTAB
```

Which matches what we learned about TCP connections and the **4-tuple**.

---

# 7. TCP State Information

`ss` is particularly useful because it exposes TCP states.

For example:

```text
LISTEN
ESTAB
TIME-WAIT
SYN-SENT
SYN-RECV
CLOSE-WAIT
FIN-WAIT-1
FIN-WAIT-2
```

These directly correspond to TCP's state machine.

For example:

```text
Client
  ↓ SYN
SYN-SENT
```

then after the connection is established:

```text
ESTAB
```

After termination, you may see:

```text
TIME-WAIT
```

So `ss` is a great way to connect the **theory of TCP states** to what's actually happening on a Linux system.

---

# 8. Find a Specific Port

Suppose your FastAPI server should run on:

```text
8000
```

You can check:

```bash
ss -tln | grep :8000
```

If you see:

```text
LISTEN 0 128 127.0.0.1:8000
```

then something is listening there.

If nothing appears:

> Nothing is currently listening on TCP port 8000.

That can immediately explain errors such as:

```text
Connection refused
```

---

# 9. See the Processes Using Sockets

On Linux, you can use:

```bash
ss -tulpn
```

The extra option:

```text
-p → show process information
```

So you might see:

```text
tcp LISTEN 0 128 127.0.0.1:8000
users:(("python",pid=1234,fd=5))
```

Now you know:

```text
Port 8000
   ↓
Python process
   ↓
PID 1234
```

This is extremely useful when debugging servers.

Note: seeing process details may require appropriate privileges depending on the system.

---

# 10. `ss` vs `netstat`

| | `netstat` | `ss` |
|---|---|---|
| Socket inspection | ✅ | ✅ |
| Listening ports | ✅ | ✅ |
| TCP states | ✅ | ✅ |
| Routing table | ✅ | More focused on sockets |
| Modern Linux usage | Older | ✅ Preferred |
| Process information | Limited/varies | ✅ |
| Speed/efficiency | Older approach | Generally better |

The important point isn't that `netstat` is "bad."

Rather:

> **On modern Linux systems, `ss` is generally the preferred tool for inspecting sockets.**

---

# 11. Practical Backend Example 🚀

Suppose you run:

```bash
uvicorn main:app --port 8000
```

Your application should listen on port `8000`.

Check:

```bash
ss -tlnp | grep :8000
```

You might see:

```text
LISTEN 0 2048 127.0.0.1:8000
```

Now suppose another computer on your LAN can't access it.

You might discover that the server is listening only on:

```text
127.0.0.1:8000
```

instead of:

```text
0.0.0.0:8000
```

That gives you a strong clue about why remote LAN access isn't working.

---

# 12. Socket Programming Connection

Remember your previous topic:

### Server

```text
socket()
   ↓
bind()
   ↓
listen()
   ↓
accept()
```

### `ss`

```text
ss
 ↓
"What sockets does the operating system currently know about?"
```

So:

```text
Your program
     ↓
creates socket
     ↓
OS tracks socket
     ↓
ss shows socket
```

---

# 🧠 Best Mental Model

Think of `ss` as looking inside your computer's **network switchboard**:

```text
                ss
                 │
      ┌──────────┼──────────┐
      ↓          ↓          ↓
 Listening    Active      TCP States
   Ports     Connections
      ↓          ↓          ↓
 "Who's       "Who am I     "What stage
 listening?"   talking to?"  is it in?"
```

---

# 🔥 Must Remember

The most useful command:

```bash
ss -tuln
```

means:

```text
TCP + UDP
+
Listening
+
Numeric
```

Useful commands:

```bash
ss -tuln
```

→ listening TCP/UDP sockets

```bash
ss -tn
```

→ TCP connections

```bash
ss -tlnp
```

→ listening TCP sockets + process information

```bash
ss -u
```

→ UDP sockets

And remember:

```text
127.0.0.1:8000
→ local-only service (generally)

0.0.0.0:8000
→ listening on all IPv4 interfaces
```

### One-line definition:

> **`ss` is a Linux command-line tool for inspecting sockets, listening ports, active connections, and TCP states.**

---

## Module 13 Progress

✅ `ping` 🏓  
✅ `traceroute` 🗺️  
✅ `nslookup` 🔎  
✅ `dig` 🧪  
✅ `netstat` 📊  
✅ **`ss` 🔌**  
⬜ `tcpdump`  
⬜ Wireshark  
⬜ Reading Packet Captures  
⬜ Common Networking Problems

**Next → `tcpdump` 🐙** — this is where troubleshooting gets much more powerful: we'll actually **capture packets traveling through an interface** and inspect what's really being sent over the network.