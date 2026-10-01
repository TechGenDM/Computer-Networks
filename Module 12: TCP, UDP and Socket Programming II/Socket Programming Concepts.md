# 🔌 Socket Programming Concepts

Now we connect the networking theory to **actual programs**.

Since you're working with Node.js, Python, FastAPI, etc., this concept is especially useful.

The simplest definition:

> **A socket is a software endpoint that an application uses to communicate over a network.**

---

# 1. What exactly is a Socket?

You've learned:

```text
IP Address → identifies the host
Port       → identifies the service/endpoint
```

A socket combines networking information with an application's communication endpoint.

For a TCP connection, we can think of an endpoint as:

```text
IP + Port + Protocol
```

Example:

```text
192.168.1.10 : 5000 : TCP
```

This tells us:

```text
Host → 192.168.1.10
Port → 5000
Protocol → TCP
```

---

# 2. Socket vs Port

Don't treat them as exactly the same.

### Port

A **number** used by TCP/UDP to identify a transport-layer endpoint.

```text
5000
```

### Socket

A **programming abstraction** through which an application sends and receives network data.

Think:

```text
Port
 ↓
Network endpoint

Socket
 ↓
Program's interface to that endpoint
```

So:

> **The port is part of the address; the socket is what the program uses to communicate.**

---

# 3. Real Example: Node.js Server

Suppose you write:

```js
import net from "net";

const server = net.createServer((socket) => {
  console.log("Client connected");
});

server.listen(5000);
```

When you run this:

```text
Node.js
   ↓
creates/listens on socket
   ↓
TCP port 5000
```

You could connect to:

```text
localhost:5000
```

So:

```text
Browser/Client
      ↓
127.0.0.1:5000
      ↓
Node.js socket
```

---

# 4. Server Socket

A server typically creates a socket and **binds it to an address/port**, then listens for incoming connections.

Conceptually:

```text
Create Socket
     ↓
Bind
     ↓
Listen
     ↓
Accept
     ↓
Communicate
```

Let's understand these.

---

# 5. `socket()`

The program creates a socket.

Conceptually:

```text
socket()
```

This gives the application a socket object/descriptor it can use for network communication.

---

# 6. `bind()`

The server associates the socket with a local IP address and port.

For example:

```text
bind(0.0.0.0, 5000)
```

Meaning, roughly:

> "Use port 5000 on the local machine/interface(s) represented by this address."

Another example:

```text
127.0.0.1:5000
```

means the service is reachable only through the local machine's loopback interface.

---

# 7. `listen()`

For TCP, the server puts the socket into a state where it can accept incoming connection attempts.

```text
listen()
```

Think:

> **"I'm ready for TCP clients to connect."**

---

# 8. `accept()`

When a TCP client connects:

```text
accept()
```

returns a **new connected socket** for that client.

This is a very important concept.

Suppose:

```text
Server
  │
  ├── Client A
  ├── Client B
  └── Client C
```

The server can continue listening on:

```text
:5000
```

while each accepted connection gets its own connected socket.

Conceptually:

```text
Listening Socket
      │
      ├── Connected Socket → Client A
      ├── Connected Socket → Client B
      └── Connected Socket → Client C
```

🔥 This distinction is extremely important.

---

# 9. Client Side

A TCP client typically does:

```text
socket()
   ↓
connect()
   ↓
send/receive
   ↓
close()
```

Example:

```text
Client
  │
  │ connect(server, 5000)
  ↓
Server
```

The `connect()` operation initiates the TCP connection establishment.

That leads to the:

```text
SYN
SYN-ACK
ACK
```

handshake we studied earlier.

---

# 10. Full TCP Socket Flow

### Server

```text
socket()
   ↓
bind()
   ↓
listen()
   ↓
accept()
   ↓
send() / receive()
   ↓
close()
```

### Client

```text
socket()
   ↓
connect()
   ↓
send() / receive()
   ↓
close()
```

This is one of the most useful diagrams to remember.

---

# 11. Example: Simple Python TCP Server

Here's a conceptual server:

```python
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server.bind(("127.0.0.1", 5000))
server.listen()

conn, address = server.accept()

data = conn.recv(1024)
print(data.decode())

conn.sendall(b"Hello from server")

conn.close()
server.close()
```

The important parts:

```python
socket.AF_INET
```

means IPv4.

```python
socket.SOCK_STREAM
```

means TCP-style byte-stream socket.

Then:

```python
bind()
listen()
accept()
```

prepare the server.

---

# 12. TCP vs UDP Socket Programming

The programming model differs because TCP and UDP differ.

### TCP

```text
Server:
socket
 ↓
bind
 ↓
listen
 ↓
accept
 ↓
send / recv
```

Client:

```text
socket
 ↓
connect
 ↓
send / recv
```

### UDP

There is no TCP-style connection establishment.

Typically:

```text
socket
 ↓
bind (server)
 ↓
sendto / recvfrom
```

A UDP application sends individual **datagrams** rather than using a connected TCP byte stream.

---

# 13. TCP Socket ≠ One Port Per Client

This is another common beginner confusion.

Suppose your server listens on:

```text
203.0.113.10:5000
```

Three clients connect:

```text
Client A → 203.0.113.10:5000
Client B → 203.0.113.10:5000
Client C → 203.0.113.10:5000
```

They can all use the same server port.

Their connections are distinguished by their connection endpoints.

For example:

```text
Client A:
192.168.1.20:51001 → 203.0.113.10:5000

Client B:
192.168.1.21:51002 → 203.0.113.10:5000

Client C:
192.168.1.22:51003 → 203.0.113.10:5000
```

Same destination port:

```text
5000
```

Different clients.

---

# 14. Socket and Your Web Development

This is where it becomes practical.

When you run:

```text
npm run dev
```

and your application says:

```text
http://localhost:3000
```

a process is listening on a local port.

Conceptually:

```text
Browser
   │
   │ TCP connection
   ↓
localhost:3000
   ↓
Your development server
```

Similarly, a backend might listen on:

```text
localhost:8000
```

and your frontend might call:

```text
http://localhost:8000/api/users
```

---

# 15. What about WebSockets?

Don't confuse **sockets** with **WebSocket**.

They're related concepts but not identical.

### Socket

General networking programming abstraction.

```text
TCP socket
UDP socket
```

### WebSocket

A higher-level protocol that provides persistent, bidirectional communication between a browser and server.

For example:

```text
Browser
   ↕
WebSocket
   ↕
Server
```

We'll keep WebSockets separate from basic socket programming.

---

# 🧠 Best mental model

Think of a socket as a **phone endpoint for a program** 📞.

```text
IP address
   ↓
Which building?

Port
   ↓
Which line?

Socket
   ↓
The actual communication endpoint
   used by the program
```

---

# 🔥 Must Remember

### TCP server

```text
socket()
↓
bind()
↓
listen()
↓
accept()
↓
send()/recv()
↓
close()
```

### TCP client

```text
socket()
↓
connect()
↓
send()/recv()
↓
close()
```

### UDP

```text
socket()
↓
sendto()/recvfrom()
```

And:

> **A listening TCP socket accepts connection requests; each accepted client connection gets a separate connected socket used for communication.**

---

## Module 12 Progress

✅ Sliding Window  
✅ **Socket Programming Concepts 🔌**  
⬜ Client-Server Model  
⬜ ARP  
⬜ NAT  
⬜ PAT  
⬜ DHCP  
⬜ Lease Process  
⬜ Default Gateway  
⬜ Home Router  
⬜ Private Networks

**Next → Client-Server Model 🖥️↔️🖥️** — we'll see exactly how the client, server, sockets, IP addresses, and ports all fit together in one complete communication flow.