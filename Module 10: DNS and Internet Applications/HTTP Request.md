# 📤 Module 9 — Topic 9: HTTP Request

Now let's look at **exactly what your browser sends to a web server**.

We know the basic cycle:

```text
Browser
   │
   │ HTTP Request 📤
   ↓
Server
   │
   │ HTTP Response 📥
   ↓
Browser
```

An HTTP request is basically the browser saying:

> **"I want this resource, and here is some information about my request."**

---

# 1. Anatomy of an HTTP Request

A simplified HTTP request looks like this:

```http
GET /products?id=42 HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

And sometimes it has a body:

```http
POST /users HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 35

{
  "name": "Devasish",
  "age": 19
}
```

So think:

```text
HTTP Request
│
├── Request Line
├── Headers
│
├── Blank Line
│
└── Optional Body
```

---

# 2. Request Line

The first line:

```http
GET /products?id=42 HTTP/1.1
```

contains three important pieces:

```text
GET
 ↓
Method

/products?id=42
 ↓
Request target

HTTP/1.1
 ↓
HTTP version
```

---

# 3. Method

The method tells the server **what the client wants to do**.

### GET

```http
GET /products
```

> "Give me the products."

### POST

```http
POST /users
```

> "I'm sending data to create/process something."

### PUT

```http
PUT /users/42
```

> "Replace/update this resource."

### PATCH

```http
PATCH /users/42
```

> "Partially update this resource."

### DELETE

```http
DELETE /users/42
```

> "Delete this resource."

---

# 4. Request Target

For example:

```http
GET /products?id=42 HTTP/1.1
    ^^^^^^^^^^^^^^^
```

This tells the server which resource/path is being requested.

The server receives:

```text
/products
```

with:

```text
id=42
```

as the query parameter.

Remember our URL anatomy:

```text
https://example.com/products?id=42
                    └─────────────┘
                    request target
```

The full URL isn't necessarily repeated in the request line.

---

# 5. HTTP Version

Example:

```http
HTTP/1.1
```

This identifies the HTTP version being used.

You'll commonly encounter:

```text
HTTP/1.1
HTTP/2
HTTP/3
```

Don't worry about the detailed differences yet.

For learning the request structure, `HTTP/1.1` is easiest to visualize.

---

# 6. Headers 📋

After the request line come headers.

Example:

```http
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
Accept-Language: en-US
```

Headers provide **metadata** about the request.

Think of them as:

> **"Extra information the client wants the server to know."**

---

# 7. The Host Header

One of the most important headers:

```http
Host: example.com
```

It tells the server which host the request is intended for.

This is particularly important because a single server/IP can host multiple websites.

For example:

```text
             203.0.113.10
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
    siteA.com  siteB.com  siteC.com
```

The request's `Host` header helps the server determine which site/application should handle the request.

---

# 8. User-Agent

Example:

```http
User-Agent: Mozilla/5.0 ...
```

This identifies information about the client software making the request.

For example:

```text
Browser
Operating system
Browser engine/client information
```

Servers may use this information for compatibility, analytics, logging, etc.

---

# 9. Accept

Example:

```http
Accept: text/html
```

This tells the server what content types the client can accept/prefer.

For example, an API client might send:

```http
Accept: application/json
```

Meaning:

> "I'd like JSON."

A browser might accept multiple types.

---

# 10. Content-Type

This is especially important when the request has a body.

Example:

```http
Content-Type: application/json
```

It tells the server:

> **"The data I'm sending is JSON."**

Then:

```http
{
  "name": "Devasish",
  "age": 19
}
```

The server knows how to interpret the body.

---

# 11. Request Body

Not every request needs a body.

For example:

```http
GET /products HTTP/1.1
Host: example.com
```

typically doesn't need a request body.

But a POST request commonly carries data:

```http
POST /users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "name": "Devasish",
  "age": 19
}
```

Here:

```text
Headers
   ↓
Blank line
   ↓
Body
```

---

# 12. Let's dissect a real-looking request 🔎

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token>

{
  "name": "Devasish",
  "email": "dev@example.com"
}
```

### Line 1

```http
POST /api/users HTTP/1.1
```

Means:

```text
POST
 ↓
Method

/api/users
 ↓
Target

HTTP/1.1
 ↓
Version
```

### Headers

```http
Host: example.com
```

Which host?

```http
Content-Type: application/json
```

What format is the body?

```http
Accept: application/json
```

What response format does the client want?

```http
Authorization: ...
```

Credentials/authorization information.

### Body

```json
{
  "name": "Devasish",
  "email": "dev@example.com"
}
```

Data being sent.

---

# 13. What happens after you press Enter?

Suppose you visit:

```text
https://example.com/products
```

Your browser eventually sends an HTTP request resembling:

```http
GET /products HTTP/1.1
Host: example.com
Accept: text/html
```

The server receives it:

```text
Internet
   ↓
Server
   ↓
Read HTTP Request
   ↓
Which method? GET
   ↓
Which resource? /products
   ↓
Process request
   ↓
Generate HTTP Response
```

And that's our **next topic**.

---

# 14. HTTP Request vs HTTP Response

Keep this distinction crystal clear.

### Request 📤

```text
Client → Server
```

The client asks for something or sends data.

### Response 📥

```text
Server → Client
```

The server tells the client what happened and may return data.

---

# 🧠 Complete mental model

```text
             HTTP REQUEST 📤

┌─────────────────────────────────┐
│ GET /products HTTP/1.1          │ ← Request Line
│ Host: example.com               │
│ Accept: text/html               │ ← Headers
│ User-Agent: ...                 │
│                                 │
│                                 │
│ [Optional Request Body]         │
└─────────────────────────────────┘
                  │
                  ↓
               SERVER
                  │
                  ↓
             HTTP RESPONSE 📥
```

---

# 🔥 What you should remember

An HTTP request consists conceptually of:

```text
Request Line
     ↓
Headers
     ↓
Blank Line
     ↓
Optional Body
```

### Request line:

```text
METHOD + TARGET + HTTP VERSION
```

### Headers:

> Metadata about the request.

### Body:

> Data sent with the request, when applicable.

### Most important methods:

```text
GET
POST
PUT
PATCH
DELETE
```

---

## Module 9 progress

✅ Why DNS?  
✅ DNS Hierarchy  
✅ Recursive Resolver  
✅ Root Server  
✅ TLD Server  
✅ Caching  
✅ URL Anatomy  
✅ HTTP Basics  
✅ **HTTP Request 📤**  
⬜ HTTP Response 📥  
⬜ Status Codes  
⬜ Cookies  
⬜ Sessions

**Next → HTTP Response 📥**

We'll take the server's response apart:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 42

{"message":"Hello World"}
```

and understand **status line + headers + body**, including exactly how the server tells your browser whether the request succeeded or failed.