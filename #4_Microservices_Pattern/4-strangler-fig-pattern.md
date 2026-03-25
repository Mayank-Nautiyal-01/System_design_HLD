# The Strangler Fig Pattern-[important]

## What Problem Are We Solving?

Imagine your company has a big, old software system — called a **Monolith** — that does everything in one giant chunk. It handles users, payments, orders, emails... all in one place.

Now your team wants to modernize it by breaking it into **microservices** (small, separate pieces). But here's the scary problem:

> **You cannot just stop the system, rewrite everything, and turn it back on.**
> Real users are using it every second. If it goes down, you lose money and trust.

This is the challenge the **Strangler Fig Pattern** solves.

---

## The Real-World Analogy First

Have you seen a **Strangler Fig tree**? It's a tropical plant that:
1. Starts growing **around** an existing tree
2. Slowly wraps itself branch by branch
3. Eventually the old tree dies and rots away
4. The new fig tree is left standing — fully formed

This software pattern works the **exact same way**:
- You build the new system **around** the old one
- You move one feature at a time (slowly, safely)
- Eventually the old system has nothing left to do
- You switch it off — and nobody even notices

---

## The Wrong Way: Big Bang Rewrite

Before learning the right way, let's see why the obvious approach is dangerous.

```
THE BIG BANG APPROACH (Don't do this!)
========================================

Month 1: "Let's stop everything and rebuild from scratch!"

    [OLD SYSTEM]                [NEW SYSTEM]
    ============                ============
    Still running...            Being built...
    Bugs piling up...           Getting out of date...
    No new features...          Requirements changing...

                     18 MONTHS LATER...

                     [ LAUNCH DAY ]
                          |
                     THINGS GO WRONG:
                     - New bugs appear
                     - Behavior is different
                     - Can't easily go back
                     - 18 months of risk hits ONE day
```

**Why this fails:** All your risk is saved up and released on one terrifying day.

---

## The Right Way: Strangler Fig Pattern

Think of it like **renovating your house room by room**, while still living in it — instead of tearing the whole house down.

### Step 1 — Put a "Front Door" in Front of Your Old System

The very first thing you do is add a **Router** (also called a Facade or API Gateway) in front of the old system. Think of it like a receptionist who decides where to send each request.

At the start, the router just sends EVERYTHING to the old system. Nothing changes for users.

```
STEP 1: Add a router (nothing changes yet)
==========================================

                  +----------+
  User  ------->  |  ROUTER  |  --------->  [ OLD MONOLITH ]
  (browser/app)   | (new)    |              (handles everything)
                  +----------+

  - Users don't notice anything different
  - The old system still does all the work
  - You are just adding a "middleman" in front
```

### Step 2 — Move ONE Feature to a New Service

Pick one feature — say, **User Accounts** — and rebuild it as a brand new, modern service. Then tell the Router:

> "Hey, when someone goes to `/users`, send them to the NEW service. Everything else still goes to the old system."

```
STEP 2: Move "User Accounts" to a new service
===============================================

                  +----------+
  User  ------->  |  ROUTER  |
                  +----------+
                  /           \
                 /             \
    /users goes HERE        Everything else goes HERE
         |                           |
         v                           v
  [ NEW User Service ]       [ OLD MONOLITH ]
  (modern, clean code)       (still running fine)
  [ its own database ]       [ its old database ]

  - Real users are using BOTH systems simultaneously
  - If the new User Service has a bug, just flip the
    router back to the old system in seconds
  - Zero risk!
```

### Step 3 — Keep Moving Features, One at a Time

Over the next few months, keep moving more features:

```
STEP 3: After moving several features (months later)
=====================================================

                     +----------+
  User  ---------->  |  ROUTER  |
                     +----------+
                    /   |   |    \
                   /    |   |     \
                  /     |   |      \
                 v      v   v       v
          [User]  [Pay] [Inv]    [OLD MONOLITH]
          Service Service Service  (getting smaller
                                    and smaller...)

  Old Monolith still runs, but handles fewer and fewer things.
```

### Step 4 — Switch Off the Old System

Once every feature has been moved, the old system receives zero requests. You can safely switch it off. Nobody notices — because nothing changed for users.

```
STEP 4: Final state — old system is gone!
==========================================

                     +----------+
  User  ---------->  |  ROUTER  |
                     +----------+
                    /   |   |   \
                   /    |   |    \
                  v     v   v     v
              [User] [Pay] [Inv] [Email]
              Service ...          Service

        [ OLD MONOLITH ] --> SWITCHED OFF. DELETED.

  You successfully modernized without any downtime!
```

---

## The Full Picture Side by Side

```
RENOVATION ANALOGY
==================

  WRONG WAY:                       RIGHT WAY:
  Tear down house,                 Renovate one room at a time
  rebuild from scratch             while still living in it
  ==================               =========================

  Month 1-18:                      Month 1-2:
  You are homeless                 Kitchen renovated.
  (system is offline)              Everyone still lives there.

  Month 18:                        Month 3-4:
  Move back in and hope            Bedroom renovated.
  everything works...              Still living there.

  (RISKY!)                         Month 5-12:
                                   All rooms done!
                                   Old stuff removed.
                                   (SAFE!)
```

---

## Key Rules to Remember

| Rule | What it means in plain English |
|------|-------------------------------|
| **Start with a router** | Add a middleman in front of the old system first |
| **Move one feature at a time** | Don't try to move everything at once |
| **Keep rollback ready** | If a new service breaks, flip the router back instantly |
| **Router only routes** | Don't put any business logic in the router |
| **Data comes later** | You can share the old database at first — fix that separately |

---

## Quick Summary

```
THE STRANGLER FIG PATTERN IN 3 SENTENCES:
==========================================

  1. Put a Router in front of your old monolith.

  2. Move one feature at a time to new microservices,
     updating the Router to send traffic to the new service.

  3. When everything is moved, switch off the old system.

  At every point, real users are happily using the system.
  There is NO scary launch day. Risk is always tiny.
```

---

## Interview Questions

**Q: Why is Strangler Fig better than rewriting everything at once?**

A: A full rewrite freezes your product for 12-24 months, and all the risk lands on one launch day. With Strangler Fig, only one feature is at risk at any time, you can always roll back in seconds by updating the router, and real users never experience downtime. Amazon used this exact approach to migrate their giant monolith to microservices over several years — while continuing to serve millions of customers throughout.

**Q: What do you do when the new microservice needs data that's still in the old system's database?**

A: In the first phase, let the new microservice read/write from the old database temporarily — this is fine as a starting point. Then, in a separate step, create a new private database for the microservice, copy the data over, run both databases in sync (write to both at the same time), verify they match, and then permanently cut over to the new database. You solve the routing problem and the data problem separately — not at the same time.
