# 🌐 Module 9 — Topic 8: HTTP Basics

Now we've reached one of the **most important protocols in web development and networking**:

# HTTP — HyperText Transfer Protocol

You've already learned:

```text
URL
 ↓
DNS
 ↓
IP address
```

Now we need to understand:

> **Once the browser knows where the server is, how does it communicate with it?**

That's where HTTP comes in.

---

# 1. What is HTTP?

**HTTP = HyperText Transfer Protocol**

It's an **application-layer protocol** used for communication between clients and servers.

The basic model is:

```text
Client                         Server
  │                              │
  │────── HTTP Request ─────────>│
  │                              │
  │<───── HTTP Response ─────────│
  │                              │
```

Usually:

```text
Browser = Client
Web Server = Server
```

So when you open a website:

```text
Browser
   ↓
HTTP Request
   ↓
Server
   ↓
HTTP Response
   ↓
Browser
```

---

# 2. HTTP follows a request-response model

This is the **most important HTTP concept**.

The client asks:

> "Give me this resource."

The server responds:

> "Here it is."

For example:

```text
Browser → GET /products
```

Server:

```text
Server → 200 OK + webpage
```

That's the **request-response cycle**.

---

# 3. HTTP is an application-layer protocol

Remember our networking layers:

```text
Application
    ↓
Transport
    ↓
Network
    ↓
Link
```

HTTP operates at:

> **Application Layer**

It doesn't itself decide:

- which router to use
- how Ethernet frames work
- how IP packets are routed

Those are handled by lower layers.

Conceptually:

```text
HTTP
 ↓
TCP / QUIC
 ↓
IP
 ↓
Ethernet / Wi-Fi
```

---

# 4. HTTP vs HTTPS

You've seen both:

```text
http://
https://
```

### HTTP

The HTTP messages themselves aren't protected by TLS.

### HTTPS

HTTPS is essentially:

> **HTTP carried over a secure TLS connection.**

Conceptually:

```text
HTTP
 ↓
TLS
 ↓
TCP
 ↓
IP
```

For modern HTTP/3, the stack differs:

```text
HTTP/3
 ↓
QUIC
 ↓
UDP
 ↓
IP
```

For now, don't worry about HTTP/3—we'll keep our main mental model around traditional HTTP over TCP.

---

# 5. HTTP is stateless

This is a **very important concept**.

HTTP itself is generally considered **stateless**.

That means:

> **Each request is treated independently; HTTP does not inherently remember previous requests.**

Imagine:

```text
Request 1:
"Give me /login"

Request 2:
"Give me /profile"

Request 3:
"Give me /orders"
```

HTTP itself doesn't automatically say:

> "Oh, this is the same person who made Request 1."

Something else has to provide that continuity.

That's where **cookies and sessions** come in later.

---

# 6. Example: Opening a website

You type:

```text
https://example.com/products
```

We've already learned the first part:

### Step 1 — DNS

```text
example.com
     ↓
DNS
     ↓
IP address
```

### Step 2 — Connection

The browser connects to the server.

For HTTPS, TLS is also involved.

### Step 3 — HTTP

The browser sends a request:

```http
GET /products HTTP/1.1
Host: example.com
```

The server processes it.

### Step 4 — Response

The server might return:

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

followed by the HTML content.

---

# 7. HTTP Methods

HTTP provides different **methods** to describe what the client wants to do.

The most important ones are:

| Method | Typical purpose |
|---|---|
| GET | Retrieve data |
| POST | Submit/create data |
| PUT | Replace/update a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |

For example:

### GET

```http
GET /products
```

> "Give me the products."

### POST

```http
POST /users
```

> "Create a user using the data I'm sending."

### DELETE

```http
DELETE /users/42
```

> "Delete user 42."

We'll study the actual request structure in the **next topic**.

---

# 8. HTTP Headers

HTTP messages contain **headers**.

Headers provide additional information.

Example:

```http
GET /products HTTP/1.1
Host: example.com
Accept: text/html
User-Agent: ...
```

Think of headers as **metadata about the request or response**.

Some common headers:

```text
Host
Content-Type
Content-Length
Authorization
Cookie
Accept
Cache-Control
```

We'll explore these more deeply when we study HTTP Requests and Responses.

---

# 9. HTTP Body

A request or response can also contain a **body**.

For example, when creating a user:

```http
POST /users HTTP/1.1
Content-Type: application/json

{
  "name": "Rahul",
  "age": 20
}
```

The JSON is the **request body**.

Similarly, the server can send a response body:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 42,
  "name": "Rahul"
}
```

So conceptually:

```text
HTTP Message
 ├── Headers
 └── Body
```

The body is optional depending on the message.

---

# 10. HTTP doesn't mean "only HTML"

This is a common misconception.

HTTP can transfer many types of resources:

```text
HTML
CSS
JavaScript
JSON
Images
Videos
Fonts
PDFs
...
```

For example:

```http
GET /api/users
```

might return:

```json
[
  {"id": 1, "name": "Rahul"},
  {"id": 2, "name": "Aman"}
]
```

So HTTP is a general **application-level request/response protocol**, not merely an HTML-transfer protocol.

---

# 11. HTTP and APIs

This is especially relevant to you as a developer.

When your React/Next.js frontend calls a backend:

```text
React App
    ↓
HTTP request
    ↓
FastAPI / Express / Flask
    ↓
HTTP response
    ↓
React App
```

For example:

```http
GET /api/products
```

Response:

```json
{
  "products": [...]
}
```

This is how a huge amount of modern web application communication works.

---

# 🧠 The complete mental model

Suppose you visit:

```text
https://example.com/products
```

Think:

```text
                 Browser
                    │
                    │ DNS
                    ↓
                IP address
                    │
                    │ Connection
                    ↓
                 Server
                    │
                    │ HTTP Request
                    ↓
              ┌─────────────┐
              │ Web Server  │
              └──────┬──────┘
                     │
                     │ HTTP Response
                     ↓
                  Browser
```

And the HTTP layer itself:

```text
HTTP Request
     ↓
Method + URL/path + Headers + Body
     ↓
Server
     ↓
HTTP Response
     ↓
Status + Headers + Body
```

We'll break those pieces down next.

---

# 🔥 HTTP Cheat Sheet

### HTTP

> Application-layer protocol for communication between clients and servers.

### Core model

```text
Client
  ↓
Request
  ↓
Server
  ↓
Response
  ↓
Client
```

### Important methods

```text
GET
POST
PUT
PATCH
DELETE
```

### HTTP message

```text
Headers
+
optional Body
```

### Important property

> **HTTP is stateless by itself.**

### HTTPS

> **HTTP + TLS security**

---

## Module 9 progress

✅ Why DNS?  
✅ DNS Hierarchy  
✅ Recursive Resolver  
✅ Root Server  
✅ TLD Server  
✅ Caching  
✅ URL Anatomy  
✅ **HTTP Basics**  
⬜ HTTP Request  
⬜ HTTP Response  
⬜ Status Codes  
⬜ Cookies  
⬜ Sessions

**Next → HTTP Request 📤**

We'll take an actual HTTP request and dissect **every line**:

```http
GET /products HTTP/1.1
Host: example.com
User-Agent: ...
Accept: text/html
Cookie: ...
```

You'll see exactly what your browser is sending to the server.