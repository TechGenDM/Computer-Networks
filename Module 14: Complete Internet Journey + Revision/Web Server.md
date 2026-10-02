# 🖥️ Web Server — In the Complete Internet Journey

We're almost at the end of the entire journey.

Our request has already passed through:

```text
Browser Cache
   ↓
DNS
   ↓
ARP
   ↓
DHCP (if needed)
   ↓
TCP Handshake
   ↓
TLS Handshake
   ↓
HTTP Request
   ↓
Router Forwarding
   ↓
NAT
   ↓
Load Balancer
   ↓
Web Server 🖥️ ← HERE
```

Now the request has finally reached the **server-side infrastructure** that will handle it.

---

# 1. What is a Web Server?

A **web server** is software that receives HTTP requests and sends HTTP responses.

Examples of web-server software include:

```text
Nginx
Apache HTTP Server
Caddy
IIS
```

But there's an important distinction:

> A web server can mean the **software**, while people sometimes casually use "web server" to mean the **machine running that software**.

---

# 2. What Does the Web Server Receive?

Suppose you requested:

```text
https://example.com/products?id=42
```

After the networking and TLS layers do their jobs, the server-side application eventually gets an HTTP request such as:

```http
GET /products?id=42 HTTP/1.1
Host: example.com
Cookie: session_id=abc123
```

Now the server has to figure out:

> **"What should I do with this request?"**

---

# 3. Web Server vs Backend Application

This distinction is **very important**.

A web server doesn't necessarily contain all your business logic.

A common architecture looks like:

```text
Client
   ↓
Load Balancer
   ↓
Web Server / Reverse Proxy
   ↓
Backend Application
   ↓
Database
```

For example:

```text
Nginx
  ↓
Node.js / Express
  ↓
PostgreSQL
```

or:

```text
Nginx
  ↓
Python / FastAPI
  ↓
PostgreSQL
```

So:

```text
Web Server
→ handles HTTP/network-facing work

Backend Application
→ handles application/business logic
```

In some simpler deployments, one application can perform both roles.

---

# 4. Static Content vs Dynamic Content

This is one of the most useful concepts.

## Static content

The server can directly return files/resources:

```text
HTML
CSS
JavaScript
Images
Fonts
```

For example:

```text
GET /logo.png
```

The web server can simply return:

```text
logo.png
```

---

## Dynamic content

The request may require application logic.

Example:

```text
GET /api/products/42
```

The backend might:

```text
Receive request
      ↓
Validate request
      ↓
Check authentication
      ↓
Query database
      ↓
Process result
      ↓
Generate JSON
```

Then:

```json
{
  "id": 42,
  "name": "MacBook"
}
```

is returned.

---

# 5. Web Server as Reverse Proxy

Modern deployments commonly use a web server such as Nginx as a **reverse proxy**.

Example:

```text
Internet
   ↓
Nginx
   ↓
Node.js application
```

Nginx might:

```text
/api/*      → Backend
/static/*   → Static files
```

It can also handle things such as:

```text
TLS termination
Request routing
Connection handling
Compression
Caching
Access logging
```

The exact responsibilities depend on the architecture.

---

# 6. What Happens to Our Request?

Let's use:

```http
GET /products?id=42
```

The web server receives it.

It may determine:

```text
/products
```

is a request for a dynamically generated page.

So it forwards the request to the application:

```text
Web Server
    ↓
Backend Application
```

The backend might then query:

```text
PostgreSQL
```

For example:

```sql
SELECT * FROM products WHERE id = 42;
```

The database returns the data:

```text
Product #42
```

The backend builds a response.

---

# 7. Full Server-Side Flow 🔥

```text
HTTP Request
     ↓
Web Server
     ↓
Route request
     ↓
Backend Application
     ↓
Business Logic
     ↓
Database / Cache / Other Services
     ↓
Result
     ↓
Backend generates response
     ↓
Web Server sends response
```

For a simple static file:

```text
HTTP Request
     ↓
Web Server
     ↓
Find file
     ↓
HTTP Response
```

So not every request needs a database.

---

# 8. Example: E-commerce Website 🛒

You request:

```text
GET /products/42
```

Server side:

```text
                    Request
                       ↓
                Load Balancer
                       ↓
                  Web Server
                       ↓
                 Backend API
                       ↓
                  PostgreSQL
                       ↓
               Product information
                       ↓
                  Backend
                       ↓
              HTTP Response
```

The backend might return:

```json
{
  "id": 42,
  "name": "MacBook Air",
  "price": 99999
}
```

The response then starts its journey back toward you.

---

# 9. Where Does TLS Decryption Happen?

With HTTPS, someone has to terminate TLS before the application can read the HTTP request.

Depending on the architecture, this might happen at:

```text
Client
   ↓
Load Balancer
   ↓
TLS termination
   ↓
Web Server / Backend
```

or:

```text
Client
   ↓
Load Balancer
   ↓
Web Server
   ↓
TLS termination
   ↓
Backend
```

The exact architecture varies.

The important idea is:

> **TLS encryption protects the HTTP data in transit; eventually an authorized server-side component must decrypt it to process the HTTP request.**

---

# 10. What Does the Database Do?

A database isn't normally directly handling your browser's HTTP request.

Instead:

```text
Browser
   ↓
Web Server
   ↓
Backend
   ↓
Database
```

The backend translates application needs into database operations.

For example:

```text
GET /products/42
       ↓
Backend
       ↓
SQL query
       ↓
PostgreSQL
       ↓
Result
       ↓
JSON response
```

This separation is important for security, architecture, and application design.

---

# 11. Web Server vs Database Server

Don't confuse these.

### Web server

Handles web/network requests:

```text
HTTP
HTTPS
```

### Database server

Stores and retrieves application data:

```text
PostgreSQL
MySQL
MongoDB
...
```

They can run on the same physical machine in a small deployment, but logically they perform different roles.

---

# 12. What Happens When the Server Is Busy?

Suppose 1,000,000 users make requests.

You may have:

```text
                 Load Balancer
                /      |      \
               ↓       ↓       ↓
             Web 1   Web 2   Web 3
               ↓       ↓       ↓
             Backend Backend Backend
                \       |       /
                 Database
```

This is why the **load balancer** we learned about earlier is important.

The web-server layer can scale horizontally:

```text
1 server
   ↓
3 servers
   ↓
50 servers
```

and the load balancer distributes traffic among them.

---

# 13. Server Generates an HTTP Response

Eventually, the application has the result.

For example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 42,
  "name": "MacBook Air"
}
```

Now the journey reverses:

```text
Web Server
   ↓
HTTP Response
   ↓
TLS encryption
   ↓
TCP
   ↓
IP
   ↓
Network
   ↓
Your Router
   ↓
MacBook
```

And that's our **next topic: HTTP Response**.

---

# 🧠 Best Mental Model

Think of the server side as a restaurant:

```text
Load Balancer
     ↓
Receptionist
     ↓
Web Server
     ↓
Takes the request
     ↓
Backend
     ↓
Chef prepares the order
     ↓
Database
     ↓
Storage room / ingredients
     ↓
Backend creates result
     ↓
Web Server returns response
```

The web server is the **front door/interface**, while the backend performs the application's deeper work.

---

# 🔥 Must Remember

> **A web server is software that accepts web requests and returns web responses, often sitting in front of backend application services.**

Typical architecture:

```text
Client
  ↓
Load Balancer
  ↓
Web Server / Reverse Proxy
  ↓
Backend Application
  ↓
Database / Cache
  ↓
Backend
  ↓
Web Server
```

### Important distinction:

```text
Web Server
→ HTTP-facing infrastructure

Backend
→ Application/business logic

Database
→ Data storage/retrieval
```

And:

> **Not every request needs a backend or database. Static resources can often be served directly by the web server.**

---

## 🎯 Module 14 Progress

✅ Browser Cache  
✅ DNS Lookup  
✅ ARP  
✅ DHCP (if needed)  
✅ TCP Handshake 🤝  
✅ TLS Handshake 🔐  
✅ HTTP Request 📤  
✅ Router Forwarding 🚦  
✅ NAT 🔄  
✅ Load Balancer ⚖️  
✅ **Web Server 🖥️**  
⬜ HTTP Response 📥  
⬜ TCP Close 🔚

**Next → HTTP Response 📥** — we'll follow the server's response all the way back to your browser, including **status code, headers, body, TLS encryption, TCP, routers, and NAT**.