# 🔗 Module 9 — Topic 7: URL Anatomy

You've probably written hundreds of URLs, but now let's understand **what every part actually means**.

Take this example:

```text
https://www.example.com:443/products?id=42#reviews
```

A URL is basically a structured way of telling the browser:

> **What protocol should I use, which server should I contact, what resource do I want, and what extra information should I send/use?**

---

# 1. Break the URL apart

```text
https://www.example.com:443/products?id=42#reviews
│      │   │         │   │       │       │
│      │   │         │   │       │       └── Fragment
│      │   │         │   │       └────────── Query
│      │   │         │   └────────────────── Path
│      │   │         └────────────────────── Port
│      │   └──────────────────────────────── Host
│      └──────────────────────────────────── Subdomain
└─────────────────────────────────────────── Scheme
```

Let's understand each one.

---

# 2. Scheme / Protocol

```text
https://
^^^^^
```

This is the **scheme**.

It tells the browser what kind of protocol/access method is being used.

Common examples:

```text
http://
https://
ftp://
```

For websites, you'll most commonly encounter:

```text
HTTP
HTTPS
```

### HTTP

```text
http://example.com
```

### HTTPS

```text
https://example.com
```

HTTPS means HTTP is carried over a **secure TLS connection**.

We'll study HTTP and HTTPS more deeply in the next topic.

---

# 3. Host / Domain

Now:

```text
www.example.com
^^^^^^^^^^^^^^^
```

This identifies the destination host.

It can be represented using a domain name.

Remember DNS?

```text
www.example.com
      ↓
     DNS
      ↓
    IP address
```

So the browser ultimately needs an IP address to communicate with the server.

---

# 4. Subdomain

Look at:

```text
www.example.com
^^^
```

`www` is a label commonly used as a hostname/subdomain.

You could have:

```text
www.example.com
api.example.com
blog.example.com
mail.example.com
```

They can point to the same server, different servers, or other infrastructure depending on their DNS configuration.

For example:

```text
example.com
├── www.example.com
├── api.example.com
└── blog.example.com
```

---

# 5. Port

Our example contains:

```text
:443
^^^^
```

The port tells the operating system **which transport-layer service endpoint** on the destination host should receive the connection.

For example:

```text
HTTP  → 80
HTTPS → 443
```

So:

```text
https://example.com:443
```

means:

> Connect to `example.com` using TCP port 443 for HTTPS, in the usual case.

### Important:

You normally don't have to type `:443`.

Because HTTPS has a **default port of 443**.

Similarly:

```text
http://example.com
```

normally means port:

```text
80
```

---

# 6. Path

Now:

```text
/products
^^^^^^^^
```

The path identifies the requested resource/path on the server.

For example:

```text
example.com/
example.com/products
example.com/products/laptops
example.com/about
```

These represent different URL paths.

Think:

```text
Domain
   ↓
example.com

Resource
   ↓
/products
```

The server/application decides what those paths mean.

---

# 7. Query String

Now:

```text
?id=42
^^^^^^
```

The `?` begins the **query component**.

It contains parameters sent as part of the URL.

Example:

```text
/products?id=42
```

means the query contains:

```text
id = 42
```

Multiple parameters can be included:

```text
/products?category=laptops&sort=price
```

Which gives:

```text
category = laptops
sort     = price
```

The server/application can use these values to determine what response to return.

---

# 8. Fragment

Finally:

```text
#reviews
^^^^^^^^
```

The `#` begins the **fragment**.

A fragment identifies a particular part of a resource.

For example:

```text
https://example.com/article#comments
```

could indicate:

> "Go to the `comments` section of this page."

### Important difference:

The fragment is generally handled by the **browser/client** and is **not sent to the server as part of the HTTP request target**.

So if you visit:

```text
https://example.com/page#reviews
```

the HTTP request is conceptually for:

```text
/page
```

The browser can then use:

```text
#reviews
```

to navigate to the relevant part of the page.

🔥 This is a very useful interview fact.

---

# 9. Let's build a URL ourselves

Suppose we want:

> Securely access the API server, on port 8443, asking for user 42.

We could have:

```text
https://api.example.com:8443/users?id=42
```

Breakdown:

```text
https://
   ↓
Scheme

api
   ↓
Subdomain

example.com
   ↓
Domain

:8443
   ↓
Port

/users
   ↓
Path

?id=42
   ↓
Query
```

---

# 10. One complete example

Let's use:

```text
https://shop.example.com:443/products/laptop?id=42&sort=price#reviews
```

| Part | Value | Purpose |
|---|---|---|
| Scheme | `https` | Protocol |
| Subdomain | `shop` | Host label |
| Domain | `example.com` | Domain name |
| Port | `443` | Transport endpoint |
| Path | `/products/laptop` | Requested resource/path |
| Query | `id=42&sort=price` | Parameters |
| Fragment | `reviews` | Client-side location within resource |

---

# 11. What happens when you enter this URL?

Now connect everything we've learned.

You enter:

```text
https://www.example.com/products
```

### Step 1 — Parse URL

Browser determines:

```text
Scheme = HTTPS
Host = www.example.com
Path = /products
```

### Step 2 — DNS

Browser/device needs the IP:

```text
www.example.com
       ↓
DNS
       ↓
IP address
```

### Step 3 — Connect to server

For HTTPS, normally:

```text
Destination port = 443
```

A connection is established, involving TCP and TLS in the traditional HTTP/1.1 or HTTP/2 stack.

### Step 4 — HTTP request

The browser sends something conceptually like:

```http
GET /products HTTP/...
Host: www.example.com
```

### Step 5 — Server responds

The server sends an HTTP response:

```text
HTTP/...
200 OK
...
```

along with the requested content.

We'll study exactly what these requests and responses look like next.

---

# 🧠 The most important mental model

Think of a URL as an **address + instructions**:

```text
https://www.example.com:443/products?id=42#reviews
│       │               │   │        │       │
│       │               │   │        │       └─ Where in the page?
│       │               │   │        └──────── Parameters
│       │               │   └──────────────── Resource
│       │               └──────────────────── Port
│       └──────────────────────────────────── Server
└──────────────────────────────────────────── Protocol
```

---

# ⚠️ One subtle distinction

Don't say:

> ❌ "The URL is the domain."

They're different.

```text
URL:
https://www.example.com/products?id=42

Domain:
example.com

Host:
www.example.com
```

A **URL** can contain a scheme, host, port, path, query, and fragment.

A **domain name** is only one component of that URL.

---

# 🔥 URL Cheat Sheet

```text
https://www.example.com:443/products?id=42#reviews
│      │   │         │   │       │       │
│      │   │         │   │       │       └─ Fragment
│      │   │         │   │       └───────── Query
│      │   │         │   └───────────────── Path
│      │   │         └───────────────────── Port
│      │   └─────────────────────────────── Host
│      └─────────────────────────────────── Subdomain
└────────────────────────────────────────── Scheme
```

### Remember these 6:

> **Scheme → how**

> **Host → where**

> **Port → which service endpoint**

> **Path → what resource**

> **Query → what parameters**

> **Fragment → what location within the resource**

---

## Module 9 progress

✅ Why DNS?  
✅ DNS Hierarchy  
✅ Recursive Resolver  
✅ Root Server  
✅ TLD Server  
✅ Caching  
✅ **URL Anatomy**  
⬜ HTTP Basics  
⬜ HTTP Request  
⬜ HTTP Response  
⬜ Status Codes  
⬜ Cookies  
⬜ Sessions

**Next → HTTP Basics 🌐** — we'll finally move from *"How do I find the server?"* to *"How does my browser actually communicate with that server?"*