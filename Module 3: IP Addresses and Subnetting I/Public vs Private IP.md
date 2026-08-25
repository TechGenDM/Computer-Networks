# 7. Public vs Private IP Addresses 🌐

Now let's understand something you'll encounter **every day** as a developer.

Not every IP address on your device is reachable directly from the Internet.

There are two broad categories we care about:

* **Private IP**
* **Public IP**

---

## 🏠 Private IP Address

A **private IP** is used inside a local/private network.

For example, your home devices might have:

```text
Laptop → 192.168.1.10
Phone  → 192.168.1.11
TV     → 192.168.1.12
Router → 192.168.1.1
```

These addresses work **inside your local network**.

Three IPv4 ranges are reserved for private networks:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

So these are private:

```text
10.20.30.40        ✅
172.16.5.10        ✅
192.168.1.25       ✅
```

But:

```text
8.8.8.8            ❌ Private
```

is a public IP.

---

# 🌍 Public IP Address

A **public IP** is globally routable on the public Internet.

Your home network typically has a public IP assigned by your ISP.

Conceptually:

```text
                 Internet
                    │
              Public IP
                    │
              ┌─────┴─────┐
              │   Router  │
              └─────┬─────┘
                    │
             Private Network
              /      |      \
         Laptop     Phone     TV
       192.168.1.10 ...
```

Your devices use private addresses internally, while your router/connection has a public Internet-facing address.

---

# 🔥 How can thousands of homes use private IPs?

Here's where **NAT** comes in.

**NAT = Network Address Translation**

Suppose:

```text
Laptop
192.168.1.10
```

wants to access a website.

The router can translate the private source address/port into its Internet-facing address/port.

Conceptually:

```text
Private Network                  Internet

192.168.1.10:5000
       │
       ▼
     Router
       │
       │ NAT
       ▼
203.x.x.x:62001 ───────────────► Web Server
```

When the response comes back, the router uses its NAT state to send it back to the correct internal device.

That's one major reason **many devices can share one public IPv4 address**.

---

# 🧠 Important distinction

A private IP isn't:

> "An IP that belongs to your laptop."

It's an address from a range specifically reserved for **private network use**.

For example, two completely different homes can both have:

```text
192.168.1.10
```

That's perfectly fine.

```text
Home A                  Home B

192.168.1.10            192.168.1.10
     │                        │
  Router A                 Router B
     │                        │
 Internet                  Internet
```

Because those private addresses exist in **different private networks**.

---

# 🎯 Quick comparison

|                                      | Private IP            | Public IP              |
| ------------------------------------ | --------------------- | ---------------------- |
| Scope                                | Private/local network | Public Internet        |
| Internet-routable                    | ❌ Not directly        | ✅ Yes                  |
| Example                              | `192.168.1.10`        | `8.8.8.8`              |
| Commonly assigned by                 | Local router/network  | ISP / network provider |
| Can be reused in different networks? | ✅ Yes                 | Generally no           |

### Remember:

> **Private IP = inside your network**

> **Public IP = Internet-facing address**

And one very important practical point:

**Private IP does not mean "secure," and public IP does not automatically mean "insecure."** Security depends on things like firewalls, NAT, access controls, and application configuration.

---

Next up: **Loopback Address** — a tiny topic, but surprisingly useful when you're developing APIs, backend servers, Docker applications, and local services.
