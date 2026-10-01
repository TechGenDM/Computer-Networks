# 📥 HTTP Response

Now we flip the direction.

We just learned:

```text
Client ───── HTTP Request ─────→ Server
Client ←──── HTTP Response ───── Server
```

An **HTTP response** is what the server sends back after receiving and processing an HTTP request.

---

## 1. Anatomy of an HTTP Response

A typical response looks like:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 27

{"message":"Hello World"}
```

Think of it as:

```text
HTTP Response
│
├── Status Line
├── Headers
├── Blank Line
└── Optional Body
```

Very similar to an HTTP request, but instead of a **request line**, we have a **status line**.

---

# 2. Status Line

First line:

```http
HTTP/1.1 200 OK
```

It contains:

```text
HTTP/1.1
   ↓
HTTP version

200
   ↓
Status code

OK
   ↓
Reason phrase
```

The most important part is the **status code**.

We'll study status codes separately next.

---

# 3. Status Code

Example:

```http
200 OK
```

means:

> "Your request was successful."

Some common ones:

```text
200 → OK
201 → Created
204 → No Content

301 → Moved Permanently
302 → Found

400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found

500 → Internal Server Error
502 → Bad Gateway
503 → Service Unavailable
```

Don't try to memorize all of these yet—we'll cover **Status Codes** properly next.

---

# 4. Response Headers 📋

After the status line come headers:

```http
Content-Type: application/json
Content-Length: 27
Cache-Control: max-age=3600
```

These are **metadata about the response**.

For example:

```http
Content-Type: application/json
```

means:

> "The data I'm sending you is JSON."

---

## 5. Content-Type

This is extremely important.

Suppose the server returns HTML:

```http
Content-Type: text/html
```

JSON:

```http
Content-Type: application/json
```

CSS:

```http
Content-Type: text/css
```

JavaScript:

```http
Content-Type: text/javascript
```

Image:

```http
Content-Type: image/png
```

It tells the client **how to interpret the response body**.

---

# 6. Content-Length

Example:

```http
Content-Length: 27
```

This tells the client the size of the response body in bytes in the relevant HTTP message framing context.

For example:

```http
Content-Length: 27

{"message":"Hello World"}
```

The client knows how much response data to expect.

---

# 7. Response Body

Finally, we can have the actual data:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{"message":"Hello World"}
```

The body is:

```json
{"message":"Hello World"}
```

This is the **actual content/data** being returned.

---

# 8. Real Website Example 🌐

You visit:

```text
https://example.com/products
```

Browser sends:

```http
GET /products HTTP/1.1
Host: example.com
Accept: text/html
```

Server might respond:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1250

<html>
  <body>
    <h1>Products</h1>
  </body>
</html>
```

So:

```text
Browser
   │
   │ GET /products
   ↓
Server
   │
   │ 200 OK + HTML
   ↓
Browser
```

The browser then uses the HTML to render the webpage.

---

# 9. API Example

This becomes even clearer with APIs.

Frontend sends:

```http
GET /api/users/42 HTTP/1.1
Host: example.com
Accept: application/json
```

Backend responds:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 42,
  "name": "Devasish"
}
```

Your React/Next.js frontend can then take that JSON and use it.

This is basically what happens constantly in modern web applications.

---

# 10. What if something goes wrong?

Suppose you request:

```http
GET /products/9999 HTTP/1.1
```

but product `9999` doesn't exist.

The server might respond:

```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "error": "Product not found"
}
```

Notice something important:

**The response can contain both an error status and a body explaining the error.**

---

# 11. Request vs Response

This is a very important interview concept.

| | Request 📤 | Response 📥 |
|---|---|---|
| Direction | Client → Server | Server → Client |
| First line | Request line | Status line |
| Contains | Method + target | Status code |
| Headers | Yes | Yes |
| Body | Optional | Optional |
| Example | `GET /users` | `200 OK` |

Mental model:

```text
REQUEST
"What do you want?"

       ↓

SERVER

       ↓

RESPONSE
"Here's what happened + here's the data."
```

---

# 🧠 Complete picture

```text
                 HTTP

Client                               Server
  │                                    │
  │  GET /products HTTP/1.1            │
  │  Host: example.com                 │
  │  Accept: text/html                 │
  │                                    │
  │ ───────── REQUEST ───────────────→ │
  │                                    │
  │                                    │ Process
  │                                    │ request
  │                                    │
  │ ←──────── RESPONSE ────────────── │
  │  HTTP/1.1 200 OK                   │
  │  Content-Type: text/html           │
  │                                    │
  │  <html>...</html>                  │
  │                                    │
```

### The key structure:

**Request:**

```text
Request Line
↓
Headers
↓
Body
```

**Response:**

```text
Status Line
↓
Headers
↓
Body
```

And the big difference:

> **Request tells the server what the client wants. Response tells the client what happened and may contain the requested data.**

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
✅ HTTP Request 📤  
✅ **HTTP Response 📥**  
⬜ Status Codes  
⬜ Cookies  
⬜ Sessions

**Next → HTTP Status Codes** 🚦 — we'll understand the `2xx`, `3xx`, `4xx`, and `5xx` families and why `404`, `401`, `403`, and `500` are different.