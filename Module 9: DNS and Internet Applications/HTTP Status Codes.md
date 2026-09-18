# 🚦 HTTP Status Codes

Status codes are **3-digit numbers in an HTTP response** that tell the client what happened to its request.

Example:

```http
HTTP/1.1 200 OK
```

Here:

```text
200 → Status Code
OK  → Meaning
```

Think of it as the server giving you a **result label**.

---

# 1. The 5 Status Code Families

The **first digit** tells you the category:

```text
1xx → Informational
2xx → Success
3xx → Redirection
4xx → Client-side error
5xx → Server-side error
```

The most important ones for everyday development are **2xx, 3xx, 4xx, and 5xx**.

---

# 2. 1xx — Informational ℹ️

These mean:

> "I received your request / we're continuing."

Example:

```text
100 Continue
```

They're less commonly handled directly in normal frontend/backend development.

For now, just remember:

**1xx = Information**

---

# 3. 2xx — Success ✅

The request was successfully processed.

### `200 OK`

The most common success response.

```http
HTTP/1.1 200 OK
```

Meaning:

> "Everything worked."

Example:

```text
GET /users
        ↓
200 OK
        ↓
User data
```

---

### `201 Created`

Something was successfully created.

Common with `POST`.

```http
POST /users
        ↓
201 Created
```

For example, you create a new account.

---

### `204 No Content`

Request succeeded, but there's no response body.

Common example:

```text
DELETE /users/42
        ↓
204 No Content
```

Meaning:

> "Done. There's nothing else to return."

---

# 4. 3xx — Redirection 🔄

These tell the client:

> "You need to look somewhere else / use another response path."

### `301 Moved Permanently`

The resource has permanently moved.

Example:

```text
oldsite.com
     ↓
301
     ↓
newsite.com
```

Browsers and search engines can use this information when handling the redirect.

---

### `302 Found`

A temporary redirect.

For example:

```text
/page-a
   ↓
302
   ↓
/page-b
```

There are other redirect codes like `303`, `307`, and `308`, but don't overload yourself yet.

---

# 5. 4xx — Client Error ❌

This is where many beginners get confused.

**4xx means the server received the request, but there is a problem with the request or the client's authorization.**

It doesn't necessarily mean "the browser is broken."

---

## `400 Bad Request`

The server can't properly process the request because the request is invalid/malformed.

Example:

```http
POST /users

{
  "age":
```

Invalid JSON → potentially:

```text
400 Bad Request
```

Think:

> **"Your request isn't valid."**

---

## `401 Unauthorized` 🔐

The request lacks valid authentication credentials.

Example:

```text
GET /api/profile
       ↓
No valid authentication
       ↓
401 Unauthorized
```

Think:

> **"You need to authenticate."**

Important nuance: despite the name, `401` is about **authentication**, not simply "you don't have permission."

---

## `403 Forbidden` 🚫

The server understood who you are (or otherwise understood the request), but refuses to allow access.

Example:

```text
Logged in as normal user
        ↓
Try accessing admin resource
        ↓
403 Forbidden
```

Think:

> **"I understand the request, but you're not allowed to do this."**

### 401 vs 403

Easy mental model:

```text
401 → "Who are you?"
403 → "I know who you are, but no."
```

---

## `404 Not Found` 🔍

The requested resource isn't available at that URI.

Example:

```http
GET /products/999999
```

Server:

```text
404 Not Found
```

Think:

> **"I can't find that resource."**

This is probably the status code you've seen most often on the web.

---

## `405 Method Not Allowed`

The resource exists, but that HTTP method isn't allowed for it.

Example:

```text
POST /products
```

If that endpoint only supports GET:

```text
405 Method Not Allowed
```

---

# 6. 5xx — Server Error 💥

This means the request may have been valid, but **the server failed while handling it**.

---

## `500 Internal Server Error`

The server encountered an unexpected problem.

Example:

```text
Browser
   ↓
GET /api/users
   ↓
Backend crashes/errors
   ↓
500 Internal Server Error
```

Think:

> **"Something went wrong on the server."**

---

## `502 Bad Gateway`

Usually means a server acting as a **gateway/proxy** received an invalid response from an upstream server.

Example:

```text
Browser
   ↓
Nginx / API Gateway
   ↓
Backend server 💥
```

Gateway can't get a valid response:

```text
502 Bad Gateway
```

---

## `503 Service Unavailable`

The server is currently unable to handle the request, often because of overload or temporary maintenance.

Think:

> **"The service isn't available right now."**

---

# 🧠 The most important status codes

If you're learning backend, remember these first:

```text
200 → OK
201 → Created
204 → No Content

301 → Moved Permanently
302 → Found

400 → Bad Request
401 → Unauthorized / authentication required
403 → Forbidden
404 → Not Found
405 → Method Not Allowed

500 → Internal Server Error
502 → Bad Gateway
503 → Service Unavailable
```

---

# 🔥 The easiest way to remember

```text
1xx → "Here's some information."

2xx → "Success! ✅"

3xx → "Go somewhere else. 🔄"

4xx → "There's a problem with the request/client side. ❌"

5xx → "The server failed. 💥"
```

---

# 4xx vs 5xx — VERY IMPORTANT

Suppose you send:

```http
GET /api/users/abc
```

and the API expects a numeric ID.

The server might return:

```text
400 Bad Request
```

because the request is invalid.

But suppose you send:

```http
GET /api/users/42
```

and the backend crashes because of a bug.

Then:

```text
500 Internal Server Error
```

So:

```text
4xx → Request/problem involving the client side
5xx → Server failed to fulfill an otherwise processable request
```

This distinction is **very important in debugging APIs**.

---

# 🧩 One complete example

Your React frontend:

```http
POST /api/login HTTP/1.1
Content-Type: application/json

{
  "email": "dev@example.com",
  "password": "..."
}
```

Possible responses:

### Successful login

```text
200 OK
```

### Account successfully created

```text
201 Created
```

### Invalid JSON/input

```text
400 Bad Request
```

### Missing/invalid authentication

```text
401 Unauthorized
```

### Authenticated but not permitted

```text
403 Forbidden
```

### Endpoint doesn't exist

```text
404 Not Found
```

### Backend crashed

```text
500 Internal Server Error
```

---

## 🎯 Final mental model

Whenever you see:

```http
HTTP/1.1 XYZ ...
```

look at the **first digit** first:

```text
       4
       ↑
   category
```

```text
2xx → Success
3xx → Redirect
4xx → Client/request problem
5xx → Server problem
```

Then look at the **specific code** to understand exactly what happened.

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
✅ HTTP Response 📥  
✅ **HTTP Status Codes 🚦**  
⬜ Cookies 🍪  
⬜ Sessions 🔐

**Next → Cookies 🍪** — this is where HTTP's **stateless** nature becomes really interesting, and you'll see how a website can remember things like your login, preferences, and shopping cart.