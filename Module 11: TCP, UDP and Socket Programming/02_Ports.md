<div align="center">

### 🌐 Computer Networks Learning Journey
#### *Module 11: TCP, UDP and Socket Programming* • **Topic 02 of 08** (Global #072)

| [⬅️ Previous: 🧱 Topic 1 — Transport Layer](./01_Transport_Layer.md) | 📑 [**Module Overview**](./README.md) | [Next: 🛡️ TCP — Transmission Control Protocol ➡️](./03_TCP.md) |
| :--- | :---: | ---: |

---

</div>

# 🔌 Ports

Now we come to one of the most important Transport Layer concepts.

You already know:

> **IP address identifies the machine.**

But a machine can run **many network applications simultaneously**.

So we need another identifier:

> **Port number identifies the application/service endpoint.**

---

# 1. Why do we need ports?

Suppose your laptop has:

```text
Chrome       → Website
Spotify      → Music
VS Code      → GitHub
Discord      → Messages
```

They may all use the same IP address:

```text
192.168.1.10
```

When data arrives, the operating system needs to know:

> "Which application should receive this data?"

That's where the port comes in.

```text
IP Address  → Which machine?
Port        → Which application/service?
```

Example:

```text
192.168.1.10:5000
            ↑
          Port
```

---

# 2. What is a Port?

A **port number** is a **16-bit number** used by the Transport Layer to identify an application/service endpoint.

Because it's 16 bits:

```text
2^16 = 65,536
```

So port numbers range from:

```text
0 → 65535
```

---

# 3. IP + Port

Together, an IP address and port identify a network endpoint.

Example:

```text
142.250.72.14:443
```

means:

```text
IP   → 142.250.72.14
Port → 443
```

Think:

```text
IP   = building address
Port = apartment/service number
```

---

# 4. Common Ports

Some services commonly use particular ports.

| Port | Common use |
|---:|---|
| **20/21** | FTP |
| **22** | SSH |
| **25** | SMTP |
| **53** | DNS |
| **80** | HTTP |
| **443** | HTTPS |
| **3306** | MySQL |
| **5432** | PostgreSQL |

For example:

```text
https://example.com
```

normally means:

```text
example.com:443
```

because HTTPS commonly uses port **443**.

---

# 5. Port Ranges

The traditional ranges are:

```text
0–1023
   ↓
Well-known ports

1024–49151
   ↓
Registered ports

49152–65535
   ↓
Dynamic / private ports
```

The exact allocation rules are maintained by IANA, but this classification is the standard way to remember the ranges.

---

# 6. What happens when you visit a website?

Suppose you visit:

```text
https://example.com
```

The browser needs to communicate with the server's HTTPS service.

Conceptually:

```text
Browser
   ↓
example.com
   ↓
DNS → IP address
   ↓
Connect to port 443
```

So the destination becomes something like:

```text
203.0.113.10:443
```

---

# 7. Source Port and Destination Port

This is **very important**.

Suppose your computer is:

```text
192.168.1.10
```

Your browser might create a connection like:

```text
192.168.1.10:53021
          ↓
203.0.113.10:443
```

Here:

```text
53021 → Source port
443   → Destination port
```

Why `53021`?

Because the operating system can assign a temporary **ephemeral port** to your client connection.

So:

```text
Client
192.168.1.10:53021

        ↓

Server
203.0.113.10:443
```

---

# 8. How can multiple applications work simultaneously?

This is where ports become really useful.

Your laptop could have:

```text
Chrome
192.168.1.10:53021
       ↓
Server:443
```

At the same time:

```text
Spotify
192.168.1.10:53022
       ↓
Server:443
```

And:

```text
VS Code
192.168.1.10:53023
       ↓
GitHub server:443
```

Same machine.

Same local IP.

Different source ports.

The operating system uses these transport-layer identifiers to deliver incoming data to the appropriate socket/application.

---

# 9. The 4-tuple 🔥

For TCP connections, a connection can be identified by four values:

```text
Source IP
Source Port
Destination IP
Destination Port
```

Example:

```text
192.168.1.10 : 53021
203.0.113.10  : 443
```

Together:

```text
(192.168.1.10, 53021,
 203.0.113.10, 443)
```

This is called the **4-tuple**.

This is extremely useful for understanding how a machine can maintain many simultaneous connections.

---

# 10. Port ≠ Application

Be careful with this.

A port doesn't permanently belong to a specific application.

For example:

```text
443 → commonly HTTPS
```

doesn't mean:

> "Port 443 can only ever run HTTPS."

A program can listen on many ports, and services can sometimes be configured to use non-standard ports.

For example, a development server might run on:

```text
localhost:3000
```

or:

```text
localhost:8000
```

---

# 11. What does `localhost:3000` mean?

You've probably seen this while developing React/Next.js apps.

Example:

```text
http://localhost:3000
```

means:

```text
localhost → this computer
3000      → TCP port 3000
```

So your browser is essentially saying:

> "Connect to the web service running on port 3000 on this machine."

For example:

```text
Browser
   │
   │ 127.0.0.1:3000
   ↓
Next.js / Vite / Node server
```

---

# 12. Ports with TCP and UDP

Both TCP and UDP have **source and destination port fields**.

So:

```text
TCP
 ├── Source Port
 └── Destination Port
```

and:

```text
UDP
 ├── Source Port
 └── Destination Port
```

This allows the OS to deliver data to the appropriate socket.

---

# 🧠 Best mental model

Think about an apartment building:

```text
IP Address
   ↓
Building

Port
   ↓
Apartment
```

For example:

```text
142.250.72.14:443
```

means:

```text
142.250.72.14 → Which machine?
443            → Which service?
```

And on your own computer:

```text
127.0.0.1:3000
```

means:

```text
127.0.0.1 → This machine
3000      → Service listening on port 3000
```

---

# 🔥 Must remember

```text
Port = 16-bit identifier
Range = 0–65535

IP address → identifies host/network endpoint
Port       → identifies transport-layer endpoint/service

HTTP  → commonly 80
HTTPS → commonly 443
SSH   → commonly 22
DNS   → commonly 53

Client usually uses an ephemeral source port.
Server commonly listens on a well-known/service port.
```

### One-line mental model:

> **IP gets data to the right computer; the port helps get it to the right network application.**

---

## Module 11 Progress

✅ Transport Layer  
✅ **Ports 🔌**  
⬜ TCP  
⬜ UDP  
⬜ Three-way Handshake  
⬜ Four-way Close

---

## 💡 Active Recall & Self-Assessment

<details>
<summary>🎯 <b>Knowledge Check: Test Your Understanding</b> (Click to expand)</summary>

> **Question:** *What five parameters uniquely identify any active TCP connection?*
>
> <details>
> <summary>👉 <b>Click to Reveal Answer & Explanation</b></summary>
>
> **Answer:**
> The 5-tuple: 1. Source IP, 2. Source Port, 3. Destination IP, 4. Destination Port, 5. Protocol (TCP).
> </details>
</details>

---

## 🧭 Navigation & Progression

| ⬅️ Previous Topic | 📑 Module Index | Next Topic ➡️ |
| :--- | :---: | ---: |
| [⬅️ Previous: 🧱 Topic 1 — Transport Layer](./01_Transport_Layer.md) | [**Module 11: TCP, UDP and Socket Programming**](./README.md) | [Next: 🛡️ TCP — Transmission Control Protocol ➡️](./03_TCP.md) |

<div align="center">
  <br/>
  <code>[██████░░░░] 64% Total Course Complete</code>
  <br/>
  <small><b>Computer Networks Mastery Series</b> • Built with ❤️ for Developers & Students</small>
</div>
