# 🔐 Sessions

This is the final topic of **Module 9**, and it connects directly with the Cookies topic.

The easiest way to understand a session is:

> **A session is server-side state that lets a website remember a client across multiple HTTP requests.**

---

## 1. Why do we need sessions?

HTTP is **stateless**.

Imagine you log in:

```http
POST /login
```

The server verifies:

```text
Email ✅
Password ✅
```

Then you request:

```http
GET /profile
```

HTTP itself doesn't automatically tell the server:

> "This is the same user who just logged in."

So we need a way to associate multiple requests with the same user.

That's what a traditional **server-side session** does.

---

# 2. How a Session Works

Suppose you log in.

### Step 1 — Login

```http
POST /login

{
  "email": "dev@example.com",
  "password": "..."
}
```

### Step 2 — Server creates a session

The server creates something like:

```text
Session ID: abc123
User: Devasish
Logged in: true
```

The server stores this session information.

### Step 3 — Server sends the ID to browser

```http
HTTP/1.1 200 OK
Set-Cookie: session_id=abc123
```

Browser stores:

```text
🍪 session_id=abc123
```

---

# 3. Next Request

You now visit:

```text
/profile
```

Browser automatically sends:

```http
GET /profile HTTP/1.1
Cookie: session_id=abc123
```

Server receives:

```text
session_id = abc123
```

and looks up:

```text
abc123
   ↓
User: Devasish
Logged in: true
```

Now the server knows which session the request belongs to.

---

# 4. The Complete Flow 🧠

```text
                 LOGIN

Browser
   │
   │ POST /login
   ↓
Server
   │
   │ Verify credentials
   ↓
Create session
   │
   │ session_id = abc123
   ↓
Browser
   │
   │ Set-Cookie: session_id=abc123
   ↓
🍪 Browser stores cookie


                 NEXT REQUEST

Browser
   │
   │ GET /profile
   │ Cookie: session_id=abc123
   ↓
Server
   │
   │ Find session abc123
   ↓
User identified
   │
   ↓
Return profile
```

This is the **cookie + session relationship** you should understand.

---

# 5. Cookie vs Session

This is extremely important.

| Cookie 🍪 | Session 🔐 |
|---|---|
| Stored by browser | Traditionally stored on server |
| Sent with requests | Represents server-side state |
| Contains cookie data | Contains session information |
| Example: `session_id=abc123` | Example: `abc123 → User 42` |

Think:

```text
COOKIE
   ↓
"I have ID abc123."

SESSION
   ↓
"abc123 belongs to this user."
```

So they work together.

---

# 6. Why not simply put everything in the Cookie?

You could technically put information into cookies, but sensitive session information generally shouldn't simply be exposed there.

For example, you don't want:

```text
🍪
username=Devasish
password=123456
accountBalance=500000
```

Instead, a traditional session approach can keep the actual state on the server:

```text
Server:

abc123
  ↓
User ID: 42
Role: user
Logged in: true
```

The browser only needs:

```text
🍪 session_id=abc123
```

---

# 7. What happens when you log out?

Suppose you click **Logout**.

The server can invalidate the session:

```text
abc123 → ❌ invalid
```

The browser's cookie can also be cleared/expired.

Then:

```http
GET /profile
Cookie: session_id=abc123
```

The server no longer accepts that session as authenticated.

---

# 8. Where are Sessions Stored?

In a traditional server-side session system, sessions can be stored in:

```text
Memory
Database
Redis
Other server-side session stores
```

For example:

```text
Browser
   │
   │ session_id=abc123
   ↓
Backend
   │
   ↓
Redis
   │
   ↓
abc123 → User #42
```

Redis is commonly used because session lookups need to be fast.

---

# 9. Session Expiration

Sessions usually shouldn't live forever.

For example:

```text
Session created
      ↓
2 hours later
      ↓
Session expires
      ↓
User must authenticate again
```

Expiration can depend on things such as:

- Idle timeout
- Absolute lifetime
- Explicit logout
- Security policies

---

# 10. Session ≠ Authentication

Another subtle but important point:

**Authentication** answers:

> "Who are you?"

**Session** provides a mechanism for maintaining state across requests.

For example:

```text
Login credentials
       ↓
Authentication
       ↓
Server creates authenticated session
       ↓
Session ID stored in cookie
       ↓
Future requests associated with session
```

So don't treat these as identical concepts.

---

# 11. Session-Based Authentication vs Token-Based Authentication

You'll encounter both in backend development.

### Traditional session-based approach

```text
Browser
  ↓
🍪 Session ID
  ↓
Server
  ↓
Session Store
```

Server maintains the session state.

### Token-based approach

For example:

```text
Browser
  ↓
Token
  ↓
Server verifies token
```

The server can validate the token rather than looking up a traditional server-side session for every request.

JWTs are one example of a token format you'll encounter.

**Don't worry about JWT details yet.** That's a separate authentication topic.

---

# 12. Real-world example

Imagine you're shopping online.

You add:

```text
MacBook
```

to your cart.

Then you navigate to:

```text
/cart
```

The website needs to associate your requests with your shopping state.

A traditional session-based design might look like:

```text
🍪 session_id=xyz789
             ↓
          Server
             ↓
Session xyz789
             ↓
User #42
             ↓
Cart
 ├── MacBook
 └── Mouse
```

So even though HTTP requests are individually stateless, the application can maintain continuity.

---

# 🧠 The complete Module 9 picture

You've now learned the entire basic web flow:

```text
                USER TYPES URL

                      ↓

                    DNS
                      ↓
              Find server IP
                      ↓
                TCP / QUIC
                      ↓
                TLS (HTTPS)
                      ↓
              HTTP Request 📤
                      ↓
                   SERVER
                      ↓
              HTTP Response 📥
                      ↓
              Status Code 🚦
                      ↓
              Cookies 🍪
                      ↓
             Sessions 🔐
```

And the key relationship:

```text
             HTTP is stateless
                    ↓
             Need continuity
                    ↓
              Session created
                    ↓
          Session ID → Cookie 🍪
                    ↓
       Cookie sent with requests
                    ↓
       Server finds the session
                    ↓
          User stays recognized
```

---

# 🔥 Module 9 — Complete

| Topic | Status |
|---|---|
| Why DNS? | ✅ |
| DNS Hierarchy | ✅ |
| Recursive Resolver | ✅ |
| Root Server | ✅ |
| TLD Server | ✅ |
| Caching | ✅ |
| URL Anatomy | ✅ |
| HTTP Basics | ✅ |
| HTTP Request | ✅ |
| HTTP Response | ✅ |
| Status Codes | ✅ |
| Cookies | ✅ |
| **Sessions** | ✅ |

### 🎯 The 5 things I'd make sure you can explain in an interview

1. **Why is HTTP called stateless?**
2. **What is a cookie?**
3. **What is a session?**
4. **How do cookies and sessions work together?**
5. **What happens from login → authenticated request?**

If you can draw this from memory:

```text
Login
 ↓
Server authenticates
 ↓
Session created
 ↓
Session ID
 ↓
Set-Cookie
 ↓
Browser stores 🍪
 ↓
Future request + Cookie
 ↓
Server finds Session
 ↓
User recognized
```

—you've got the core idea. 💪