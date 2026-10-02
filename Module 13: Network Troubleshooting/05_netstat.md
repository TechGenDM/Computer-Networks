<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 13: Network Troubleshooting* • **Topic 05 of 10** (Global #094)

| [⬅️ Previous: 🧪 `dig` — DNS Investigation Tool](./04_dig.md) | 📑 [**Module Overview**](./README.md) | [Next: 🔌 `ss` — Socket Statistics ➡️](./06_ss.md) |
| :--- | :---: | ---: |

---

</div>

# 📊 `netstat`

Now we're moving from **DNS/path troubleshooting** into inspecting what's happening **inside your own machine**.

The simplest definition:

> **`netstat` is a command-line tool that displays network connections, listening sockets, routing information, and network statistics.**

Think of it as:

```text
ping      → Can I reach it?
traceroute → Where does traffic go?
nslookup  → What does DNS say?
dig       → Show me detailed DNS information
netstat   → What network connections/ports exist on my machine?
```

---

# 1. Basic command

Try:

```bash
netstat
```

But you'll usually want options.

A very useful one:

```bash
netstat -an
```

This shows network connections/listening sockets without trying to resolve names.

You might see something like:

```text
Proto  Local Address        Foreign Address      State
tcp4   127.0.0.1.3000       *.*                  LISTEN
tcp4   192.168.1.10.53021   142.250.x.x.443      ESTABLISHED
```

---

# 2. Understanding the Output

Let's take:

```text
tcp4  192.168.1.10.53021  142.250.x.x.443  ESTABLISHED
```

### `tcp4`

TCP over IPv4.

### `192.168.1.10.53021`

Local endpoint:

```text
IP   = 192.168.1.10
Port = 53021
```

### `142.250.x.x.443`

Remote endpoint:

```text
IP   = 142.250.x.x
Port = 443
```

### `ESTABLISHED`

The TCP connection is currently established.

---

# 3. `LISTEN` 👂

Suppose you run your backend:

```text
localhost:8000
```

You might see:

```text
tcp4  127.0.0.1.8000  *.*  LISTEN
```

This means a TCP socket is **waiting for incoming connections** on port `8000`.

Remember our socket programming flow:

```text
socket()
 ↓
bind()
 ↓
listen()
 ↓
accept()
```

`netstat` lets you inspect the result from outside the program.

---

# 4. Finding Listening Ports

A very useful troubleshooting command:

```bash
netstat -an | grep LISTEN
```

This can show services currently listening for TCP connections.

For example:

```text
127.0.0.1.3000     *.*     LISTEN
127.0.0.1.8000     *.*     LISTEN
*.22               *.*     LISTEN
```

You can then reason:

```text
3000 → some development server
8000 → some backend
22   → SSH service
```

The exact services depend on what's running on the machine.

---

# 5. `ESTABLISHED`

Example:

```text
tcp4 192.168.1.10.53021 142.250.x.x.443 ESTABLISHED
```

This means:

```text
Your machine
   │
   │ TCP connection
   ↓
Remote server
```

So `netstat` can help you see active connections.

---

# 6. Other TCP States

You'll encounter states such as:

```text
LISTEN
ESTABLISHED
SYN_SENT
SYN_RECEIVED
TIME_WAIT
CLOSE_WAIT
FIN_WAIT_1
FIN_WAIT_2
```

These correspond to the **TCP state machine** you have already started learning.

For example:

### `SYN_SENT`

Your machine has sent a SYN and is waiting for the server's response.

```text
Client → SYN → Server
        ↓
    SYN_SENT
```

### `ESTABLISHED`

The TCP connection is ready for normal data transfer.

### `TIME_WAIT`

The endpoint is waiting for the appropriate period after closing a TCP connection.

This is where your earlier **three-way handshake and four-way close** knowledge becomes useful.

---

# 7. Routing Table 🛣️

`netstat` can also display routing information.

On macOS/Linux:

```bash
netstat -rn
```

You may see something conceptually like:

```text
Destination        Gateway          Flags
default            192.168.1.1
192.168.1           link#...
127                 127.0.0.1
```

The important entry:

```text
default → 192.168.1.1
```

means:

> Traffic without a more-specific route can be sent toward the default gateway.

This directly connects to what we studied earlier.

---

# 8. Why `-n`?

The `n` means:

> **Don't resolve addresses/numbers into names.**

So:

```bash
netstat -an
```

usually gives numeric addresses and ports.

This is useful because:

```text
✅ Faster
✅ Easier to read during troubleshooting
✅ Avoids additional DNS lookups
```

---

# 9. Network Statistics

`netstat` can also display protocol/network statistics.

For example:

```bash
netstat -s
```

Depending on the operating system, this can show statistics for protocols such as:

```text
TCP
UDP
IP
ICMP
```

The exact output differs between macOS, Linux, and BSD variants.

---

# 10. Practical Debugging Example 🛠️

Imagine your Node.js backend should be running on:

```text
localhost:8000
```

But your frontend says:

```text
Connection refused
```

Instead of guessing, check:

```bash
netstat -an | grep 8000
```

### If you see:

```text
127.0.0.1.8000   *.*   LISTEN
```

Something is listening on port 8000.

### If nothing appears:

There may simply be **no process listening on port 8000**.

Possible causes:

```text
Backend isn't running
Backend crashed
Backend uses another port
Server bound to another address
```

That's much more useful than blindly restarting things.

---

# 11. `netstat` and Sockets

Remember:

```text
Socket
 ↓
IP + Port
 ↓
Network connection
```

`netstat` gives you visibility into those endpoints.

For example:

```text
Local                  Foreign
192.168.1.10:53021  →  142.250.x.x:443
```

You can immediately see:

```text
Local port = 53021
Remote port = 443
Protocol = TCP
State = ESTABLISHED
```

---

# 12. `netstat` vs `ss`

Your module also contains:

```text
netstat
ss
```

They solve similar troubleshooting problems.

### `netstat`

Older, widely known tool.

```text
Connections
Listening ports
Routing information
Statistics
```

### `ss`

Modern Linux-focused socket inspection tool.

```text
Socket statistics
Active connections
Listening sockets
TCP states
```

For example on Linux:

```bash
ss -tuln
```

is commonly used to show listening TCP/UDP sockets.

On modern Linux systems, `ss` is generally preferred over `netstat`.

**macOS is different:** `netstat` is available, while `ss` is generally not the standard built-in equivalent there.

---

# 🧠 Best Mental Model

Think of `netstat` as a **network dashboard for your computer**:

```text
              netstat
                 │
      ┌──────────┼──────────┐
      ↓          ↓          ↓
 Connections  Listening   Routing
             Ports        Table
      ↓          ↓          ↓
  Who am I    What is     Where do
 connected    waiting?    packets go?
   to?
```

---

# 🔥 Must Remember

### Most useful commands

```bash
netstat -an
```

View network connections/listening sockets numerically.

```bash
netstat -an | grep LISTEN
```

Find listening TCP sockets.

```bash
netstat -rn
```

View the routing table.

```bash
netstat -s
```

View protocol/network statistics.

### Important states:

```text
LISTEN      → waiting for connections
ESTABLISHED → active TCP connection
SYN_SENT    → SYN sent, waiting
TIME_WAIT   → connection recently closed
```

### One-line definition:

> **`netstat` lets you inspect network connections, listening ports, routing information, and network statistics on your machine.**

---

## Module 13 Progress

✅ `ping` 🏓  
✅ `traceroute` 🗺️  
✅ `nslookup` 🔎  
✅ `dig` 🧪  
✅ **`netstat` 📊**  
⬜ `ss`  
⬜ `tcpdump`  
⬜ Wireshark

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What does `netstat -tuln` display on a Linux machine?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> It lists all **T**CP and **U**DP sockets that are **L**istening, showing **N**umerical IP addresses and port numbers without slow DNS lookups.
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🧪 `dig` — DNS Investigation Tool](./04_dig.md) | [**Module 13: Network Troubleshooting**](./README.md) | [Next: 🔌 `ss` — Socket Statistics ➡️](./06_ss.md) |

<div align="center">
  <br/>
  <code>[████████░░] 83% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
