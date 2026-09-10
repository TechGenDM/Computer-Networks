# 🔄 Module 8 — Topic 6: Convergence

This topic connects **Distance Vector, Link State, RIP, OSPF, and BGP**.

The core idea is actually very simple:

> **Convergence = the process by which routers update their routing information after a network change until they have a consistent, usable view of the new topology.**

---

## 1. First, imagine everything is working

```text
A ─── B ─── C ─── D
```

Suppose A's route to D is:

```text
A → B → C → D
```

Everything is normal.

Now suddenly:

```text
A ─── B ─── C ❌ D
```

The C-D link fails.

But here's the important part:

**Every router doesn't instantly know that the link failed.**

Information has to propagate.

That period is where **convergence** happens.

---

# 2. What happens during convergence?

Think of it as:

```text
Network change
      ↓
Router detects change
      ↓
Routing information is updated
      ↓
Information propagates
      ↓
Routers recalculate routes
      ↓
New routes are installed
      ↓
Network converges
```

Before convergence:

```text
Some routers → old routes
Some routers → new routes
```

After convergence:

```text
Routers → consistent usable routing information
```

---

# 3. Simple example

Consider:

```text
A ── B ── C
     │
     D
```

Suppose B has a route:

```text
B → C
```

Then the B-C link fails:

```text
A ── B    C
          ❌
```

B detects:

> "C is no longer reachable through this link."

B must update its routing information.

Depending on the protocol, the mechanism differs.

---

# 4. Distance Vector convergence

Let's use RIP.

Suppose:

```text
A ── B ── C
```

A reaches C through B:

```text
A → B → C
```

Then B-C fails.

B eventually advertises:

```text
C = unreachable
```

For RIP:

```text
Metric = 16
```

Other routers receive the update and modify their routing tables.

Eventually:

```text
A → C = unreachable
```

The network has converged around the failure.

---

# 5. Link State convergence

Now consider OSPF.

Same failure:

```text
A ── B ── C
         ❌
```

B detects the failed link.

OSPF can generate updated link-state information.

Conceptually:

```text
Link failure
     ↓
Updated LSA
     ↓
Flooded through the OSPF area
     ↓
Routers update LSDB
     ↓
Run SPF/Dijkstra
     ↓
New routes
```

So OSPF's convergence process is quite different from RIP's.

---

# 6. BGP convergence

BGP is different again.

Imagine:

```text
AS 100 → AS 200 → AS 300
```

The route through AS 200 disappears.

BGP routers exchange route updates/withdrawals.

Then routers perform BGP's best-path selection based on:

- routing policy
- BGP attributes
- AS path
- other route-selection rules

Eventually, routers settle on available paths.

---

# 7. Fast vs slow convergence

This is an important networking concept.

### Fast convergence

```text
Failure
  ↓
Quick detection
  ↓
Quick update
  ↓
Quick recalculation
  ↓
New route
```

Less disruption.

### Slow convergence

```text
Failure
  ↓
Delayed detection
  ↓
Old information remains
  ↓
Incorrect forwarding
  ↓
Eventually updated
```

This can cause:

- packet loss
- temporary routing loops
- black holes
- connectivity interruptions

---

# 8. What is a routing black hole?

Imagine:

```text
A ── B ── C
```

C becomes unreachable.

But A hasn't learned this yet.

A continues sending:

```text
A → B → C
```

B may drop the packet because C is no longer reachable.

The packet effectively disappears.

This situation is often called a **routing black hole**.

```text
        ❌
A → B → [DROP]
```

---

# 9. Convergence and routing loops

During convergence, different routers can temporarily have inconsistent information.

For example:

```text
A thinks:
C → B

B thinks:
C → A
```

So:

```text
A → B → A → B → A...
```

That's a **routing loop**.

This is particularly associated with distance-vector behavior, though transient loops can occur in other routing situations too.

TTL/Hop Limit protects the packet from looping forever.

---

# 10. What determines convergence speed?

Several things affect it:

### ① Failure detection

How quickly does the router realize something is wrong?

```text
Fast detection → faster convergence
```

### ② Update propagation

How quickly does the information reach other routers?

### ③ Route calculation

How quickly can routers calculate new paths?

### ④ Protocol behavior

Different protocols have different mechanisms and timers.

---

# 11. RIP vs OSPF convergence

This is a useful comparison.

### RIP

```text
Failure
 ↓
Distance-vector updates
 ↓
Neighbor exchanges
 ↓
Routing table updates
 ↓
Convergence
```

Generally slower and more susceptible to issues such as count-to-infinity.

### OSPF

```text
Failure
 ↓
Link-state change
 ↓
LSA flooding
 ↓
LSDB update
 ↓
Dijkstra/SPF
 ↓
New routing table
 ↓
Convergence
```

Generally faster and more scalable for larger internal networks.

---

# 12. Convergence does NOT mean "all routers have identical routing tables"

This is a subtle but important point.

Suppose:

```text
A → B → C
```

A's table might say:

```text
C → B
```

B's table might say:

```text
C → C
```

Those tables are obviously different.

Convergence means that routers have **consistent, valid routing information appropriate to their own positions and policies**, not that every router has identical next-hop entries.

---

# 13. A real-world mental model 🚦

Imagine Google Maps.

You're driving:

```text
Home → Highway → Office
```

Suddenly:

```text
🚧 Highway CLOSED
```

Your navigation system needs to:

```text
Detect closure
     ↓
Receive/update traffic information
     ↓
Recalculate route
     ↓
Give you a new route
```

That process is analogous to **routing convergence**.

---

# 🧠 The most important distinction

### Routing update

> "Something changed."

### Route calculation

> "What is the best path now?"

### Convergence

> **"The network has processed the change and settled on the new routing state."**

---

# 🔥 Exam-ready definition

> **Routing convergence is the process by which routers detect a topology or reachability change, exchange or process updated routing information, recalculate routes, and eventually reach a stable routing state.**

---

## Module 8 progress

✅ Distance Vector Routing  
✅ Link State Routing  
✅ RIP  
✅ OSPF  
✅ BGP  
✅ **Convergence**  
⬜ Routing Loops

We're now at the **final topic of Module 8: Routing Loops** 🔁.

That's where we'll take the problems we've already seen—especially **count-to-infinity**—and understand exactly **how loops form, why they happen, and how routing protocols prevent them.**