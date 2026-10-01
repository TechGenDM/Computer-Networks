# 🍪 Cookies

Cookies are one of those HTTP concepts that look simple but are **extremely important for web development**.

Remember something from HTTP Basics:

> **HTTP is stateless.**

That means the server doesn't automatically remember previous requests.

Cookies help provide a way for the browser and server to **maintain information across requests**.

---

## 1. The Problem: HTTP is Stateless

Imagine you log into Instagram.

First request:

```http
POST /login
```

Server checks your username/password:

```text
✅ Correct!
```

Then you visit:

```http
GET /profile
```

How does the server know that this is **you**?

HTTP itself doesn't automatically remember:

```text
"Hey, this is the same person who logged in 10 seconds ago."
```

Each HTTP request is independent.

We need a mechanism to maintain state.

That's where **cookies + sessions** come in.

---

# 2. What is a Cookie?

A cookie is a **small piece of data stored by the browser and associated with a website's domain**.

Example:

```text
session_id=abc123
```

The server can ask the browser to store it.

Later, the browser can send it back with requests.

Mental model:

```text
Server
   ↓
"Browser, remember this."
   ↓
🍪 Cookie
   ↓
Browser stores it
   ↓
Browser sends it with relevant requests
```

---

# 3. How a Cookie Gets Created

Suppose you log in.

The server responds with a `Set-Cookie` header:

```http
HTTP/1.1 200 OK
Set-Cookie: session_id=abc123
```

The browser receives this and stores:

```text
🍪 session_id = abc123
```

---

# 4. Browser Sends the Cookie Back

Now you request:

```http
GET /profile
```

The browser may automatically include:

```http
Cookie: session_id=abc123
```

So:

```text
First:

Browser ── login ──→ Server
Browser ← Set-Cookie ─ Server
       🍪 stores cookie


Later:

Browser ── Cookie ──→ Server
Browser ←─ Response ─ Server
```

This is the core idea.

---

# 5. `Set-Cookie` vs `Cookie`

This distinction is **very important**.

### Server → Browser

```http
Set-Cookie: session_id=abc123
```

Means:

> "Store this cookie."

### Browser → Server

```http
Cookie: session_id=abc123
```

Means:

> "Here is the cookie you previously gave me."

So:

```text
Set-Cookie → server sets it
Cookie     → browser sends it
```

🔥 Remember this for interviews.

---

# 6. Example: Login

### Step 1 — Login request

```http
POST /login HTTP/1.1
Content-Type: application/json

{
  "username": "dev",
  "password": "..."
}
```

### Step 2 — Server responds

```http
HTTP/1.1 200 OK
Set-Cookie: session_id=abc123
```

Browser stores:

```text
🍪 session_id=abc123
```

### Step 3 — User visits profile

```http
GET /profile HTTP/1.1
Cookie: session_id=abc123
```

Server sees:

```text
session_id = abc123
```

and can use that identifier to determine the associated logged-in session.

---

# 7. Cookies Don't Necessarily Store the Actual User Data

This is an important distinction.

You might have:

```text
🍪 session_id=abc123
```

The cookie doesn't have to contain:

```text
username = Devasish
password = ...
account_balance = ...
```

Instead, the cookie can contain an **identifier**.

The server maintains the actual session information:

```text
Server:

abc123
  ↓
User: Devasish
Logged in: yes
```

We'll connect this directly to **Sessions** in the next topic.

---

# 8. Important Cookie Attributes

Cookies can have additional attributes controlling how they behave.

Example:

```http
Set-Cookie: session_id=abc123; Secure; HttpOnly; SameSite=Lax
```

Let's understand the important ones.

### `Secure` 🔒

```text
Secure
```

Means the browser should send the cookie only over a secure connection, normally HTTPS.

---

### `HttpOnly`

```text
HttpOnly
```

Prevents JavaScript running in the page from accessing the cookie through `document.cookie`.

This is useful for cookies containing sensitive session identifiers because it reduces exposure to some client-side attacks such as cookie theft through XSS.

It does **not** make the cookie magically immune to all attacks.

---

### `SameSite`

Controls when cookies are sent in **cross-site contexts**.

Common values:

```text
Strict
Lax
None
```

You don't need to memorize all the edge cases yet.

For now:

> `SameSite` helps control cross-site cookie sending and is an important part of CSRF defenses.

---

### `Expires` / `Max-Age`

Controls how long the cookie persists.

Example:

```http
Set-Cookie: theme=dark; Max-Age=3600
```

Roughly:

```text
Max-Age = 3600 seconds
         = 1 hour
```

---

# 9. Session Cookie vs Persistent Cookie

### Session cookie

Generally exists only for the browser session and doesn't specify a persistent expiration.

### Persistent cookie

Has an expiration through:

```text
Expires
```

or:

```text
Max-Age
```

Example:

```http
Set-Cookie: theme=dark; Max-Age=86400
```

The browser can retain it for the specified lifetime.

---

# 10. Cookies and Authentication 🔐

One of the most common uses of cookies is authentication.

```text
Login
  ↓
Server verifies credentials
  ↓
Server creates session
  ↓
Server gives browser session cookie
  ↓
Browser stores 🍪
  ↓
Browser sends 🍪 with future requests
  ↓
Server identifies session
  ↓
User remains logged in
```

That's why you can close one page, open another, and still be logged in.

---

# 11. Cookies Are NOT the Same as Sessions

This is where the next topic becomes important.

### Cookie

Stored on the **client/browser**.

```text
Browser
  ↓
🍪 cookie
```

### Session

Represents **server-side state** associated with a user/session identifier in a traditional server-side session design.

```text
Server
  ↓
Session data
```

They often work together:

```text
🍪 session_id=abc123
           ↓
Server
           ↓
Session abc123
           ↓
User's session data
```

But modern applications can also use other authentication designs, such as tokens in cookies, so **cookie ≠ session**.

---

# 🧠 The entire concept

```text
                 LOGIN

Browser ───────────────→ Server
         POST /login

Browser ←─────────────── Server
         Set-Cookie:
         session_id=abc123

            🍪
       Browser stores it


                 NEXT REQUEST

Browser ───────────────→ Server
         Cookie:
         session_id=abc123

                         ↓
                   Server identifies
                     the session

Browser ←─────────────── Server
         Response
```

---

## 🔥 Must-remember points

```text
Cookie = small data stored by browser

Set-Cookie = server tells browser to store/update cookie

Cookie = browser sends cookie back to server

Cookies help maintain state across HTTP requests

HttpOnly = JavaScript cannot read that cookie via document.cookie

Secure = send over HTTPS

SameSite = controls cross-site cookie sending

Cookie ≠ Session
```

### One-line mental model:

> **🍪 Cookie = the browser's little ID card/token that it can carry between requests.**

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
✅ Status Codes 🚦  
✅ **Cookies 🍪**  
⬜ Sessions 🔐

**Next → Sessions 🔐** — we'll connect everything together: **login → session creation → session ID → cookie → authenticated requests**.