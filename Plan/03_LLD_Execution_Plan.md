# 03 — LLD / MACHINE CODING EXECUTION PLAN

> **Mission:** Ship a clean, extensible, running design in **90 minutes**, 20 times.
> Reference: `../LLD_MasterSheet.md` + `../LLD_Java_Spring_MasterSheet.md` — *lookup only.*
> **Who gates on this:** Uber, Flipkart, Atlassian, Swiggy, Razorpay, PhonePe, Zomato, Navi,
> Salesforce, Adobe, Arcesium, ThoughtWorks, most Indian product companies + several FAANG India loops.

---

## 1. WHY THIS TRACK IS YOUR CHEAT CODE

Most candidates are weak at LLD because it needs *real engineering taste*, which only comes from
shipping production code — which you have 5.3 years of. Meanwhile it's **learnable to
near-perfection** because the problem space is small (~25 recurring problems, ~10 patterns).

**Consequence:** LLD-heavy companies are your fastest path to a strong offer. Use them as
both warm-ups AND as leverage (a competing offer) for the tier-1 loops.

---

## 2. THE 90-MINUTE MACHINE-CODING PROTOCOL

```
 0–10 min  REQUIREMENTS & SCOPE
           Clarify. List 5–7 functional requirements. State assumptions out loud.
           Explicitly declare non-goals ("no persistence, in-memory; no auth").

10–20 min  ENTITIES & RELATIONSHIPS
           Nouns → classes. Draw a quick class diagram. Identify enums/value objects.
           Decide: what varies? → that's where the interface goes.

20–30 min  INTERFACES & PATTERNS
           Name the patterns explicitly and WHY:
           "Strategy for pricing because rules change per city"
           "Observer for notifications because subscribers are unknown at compile time"
           "State for the booking lifecycle because transitions are constrained"

30–75 min  CODE — compile-ready, runnable
           Package structure · interfaces first · then impls · then a service/facade
           Custom exceptions · thread-safety where needed · NO god class

75–85 min  DEMO / DRIVER + tests
           A `Main`/`Demo` class exercising the happy path + 1 edge case.
           At least 2 JUnit tests if time allows.

85–90 min  EXTENSIBILITY WALKTHROUGH
           "If we added X tomorrow, I'd only change ___ ." ← THIS is the scoring moment.
```

### The 8 things graders actually check
1. ✅ **It compiles and runs.** (Shocking number of candidates fail here — always leave 10 min.)
2. ✅ Correct separation: model / service / repository / strategy / factory
3. ✅ **No god class**, no business logic in `main`
4. ✅ Interfaces at the variation points (open-closed)
5. ✅ Meaningful names, no `Manager1`, no `data`, no `temp`
6. ✅ Custom exceptions, not `throw new RuntimeException("error")`
7. ✅ Concurrency addressed where it matters (booking/seat/inventory → locking or atomic)
8. ✅ You can articulate **one trade-off you deliberately made**

### Standard package skeleton (memorise, use every time)
```
com.<domain>
├── model/          // entities, value objects, enums
├── repository/     // in-memory stores behind an interface
├── service/        // orchestration / use-cases
├── strategy/       // pluggable algorithms (pricing, allocation, matching)
├── factory/        // object creation
├── observer/       // event notification
├── exception/      // domain exceptions
└── Demo.java       // driver
```

---

## 3. THE 10 PATTERNS THAT COVER 95% OF LLD ROUNDS

| Pattern | Trigger phrase in the problem | Classic use |
|---------|-------------------------------|-------------|
| **Strategy** | "different ways of / configurable rules" | Pricing, parking-spot allocation, payment method, ride matching |
| **Factory / Abstract Factory** | "create different types of" | Vehicle, Notification channel, Piece (chess) |
| **Singleton** | "single shared registry/manager" | Config, ID generator, in-memory store (⚠️ use sparingly, mention DI alternative) |
| **Builder** | "object with many optional fields" | Order, Pizza, Report, Request |
| **Observer** | "notify when X happens" | Notifications, leaderboard, stock ticker, order status |
| **State** | "lifecycle / transitions / status" | Vending machine, ATM, elevator, booking, order |
| **Command** | "undo / queue of actions / remote control" | Text editor, task scheduler, TV remote |
| **Decorator** | "add features dynamically" | Coffee/Pizza toppings, middleware, logging |
| **Chain of Responsibility** | "pass through handlers until one handles" | Logger levels, ATM cash dispenser, approval flow |
| **Repository / DAO** | Always | Any in-memory store |

**Plus concurrency primitives you must be fluent in:**
`ConcurrentHashMap` · `AtomicInteger/AtomicLong` · `ReentrantLock` + `tryLock(timeout)` ·
`ReadWriteLock` · `synchronized` on the right granularity · `BlockingQueue` ·
`ScheduledExecutorService` · optimistic locking via `compareAndSet`

> Seat booking / inventory / wallet problems: **always** raise the race condition unprompted.
> "Two users booking the same seat — I'll guard with a per-show lock / atomic CAS on seat state."

---

## 4. THE 20-PROBLEM LADDER (Weeks 1–18)

**Cadence:** Mon evening = design + partial code (90 min). Sat evening = **timed 90-min full build**.
Every solution committed to your private `interview-prep` GitHub repo.

### Level 1 — Foundations (Weeks 1–4)
| Wk | Problem | Patterns to force yourself to use | Twist to handle |
|----|---------|-----------------------------------|-----------------|
| 1 | **Parking Lot** | Strategy (spot allocation), Factory (vehicle), Singleton | Multi-floor, pricing by duration, spot types |
| 2 | **Vending Machine** | State, Strategy (payment) | Change dispensing, out-of-stock, refund on cancel |
| 3 | **Elevator System** | State, Strategy (scheduling), Observer | Multiple lifts, SCAN/LOOK algorithm, direction priority |
| 4 | **Snake & Ladder** | Factory, Strategy (dice), Observer | N players, multiple dice, generalised board |

### Level 2 — Real Products (Weeks 5–9)
| Wk | Problem | Patterns | Twist |
|----|---------|----------|-------|
| 5 | **Splitwise** | Strategy (split type), Observer | Equal/exact/percent splits, simplify debts algorithm |
| 6 | **BookMyShow** | State, Factory, **locking** | **Seat concurrency**, hold-with-TTL, payment timeout |
| 7 | **Tic-Tac-Toe → Chess** | Strategy (move validation), Factory (piece), Command (undo) | Generalise to N×N; chess piece movement rules |
| 8 | **LRU + LFU + TTL Cache** | — (pure DS design) | O(1) get/put, thread-safe, eviction listener, expiry sweeper |
| 9 | **Rate Limiter (code)** | Strategy (algorithm) | Token bucket + sliding window, per-user, thread-safe |

### Level 3 — Systems Flavour (Weeks 10–14)
| Wk | Problem | Patterns | Twist |
|----|---------|----------|-------|
| 10 | **Food Delivery (Swiggy)** | Strategy (matching, pricing), State (order), Observer | Restaurant search, delivery partner assignment, order lifecycle |
| 11 | **Logging Framework** | Chain of Responsibility, Singleton, Strategy (appender) | Levels, async logging, multiple sinks, formatters |
| 12 | **In-Memory Key-Value DB** | Command, Repository | Transactions (BEGIN/COMMIT/ROLLBACK), nested txns, TTL |
| 13 | **Ride Hailing (Uber)** | Strategy (matching/pricing), State (trip), Observer | Driver-rider matching, surge, trip state machine |
| 14 | **Notification Service** | Observer, Strategy (channel), Chain, Builder | Email/SMS/Push, retry with backoff, templates, preferences |

### Level 4 — Interview Simulation (Weeks 15–18, 2 per week, strictly timed)
| # | Problem | Why |
|---|---------|-----|
| 15 | **ATM Machine** | State + Chain of Responsibility (cash dispenser) |
| 16 | **Library Management** | Classic CRUD-with-rules; tests clean layering |
| 17 | **Car Rental System** | Search + inventory + pricing strategy |
| 18 | **Meeting Scheduler / Calendar** | Interval conflicts + recurring events + notifications |
| 19 | **Text Editor with Undo/Redo** | Command + Memento |
| 20 | **Task Scheduler / Cron** | Priority queue + `ScheduledExecutorService` + retries |

### Stretch (only if ahead)
Digital Wallet · Auction System · Cricinfo Scorecard · Traffic Signal · File System ·
Inventory Management · Payment Gateway · Hotel Booking · Airline Reservation · Amazon Locker

---

## 5. MACHINE CODING vs LLD-DISCUSSION — know which round you're in

| | **Machine Coding** (Uber, Flipkart, Atlassian, Swiggy) | **LLD Discussion** (most FAANG) |
|---|---|---|
| Duration | 90–120 min, you code alone, then review | 45–60 min, interactive |
| Deliverable | Running code + demo | Class diagram + key code snippets |
| Priority | **Working > perfect.** Compile early, commit often | **Reasoning > code volume** |
| Killer mistake | Nothing runs at the end | Jumping to code without clarifying |
| Prep emphasis | Speed, muscle-memory skeleton, typing | Verbalising trade-offs, extensibility |

Train both: **Saturday = machine coding (silent, timed).** **Week 15–18 mocks = discussion style.**

---

## 6. THE EXTENSIBILITY QUESTIONS THEY WILL ASK (prepare answers per problem)

For every problem you build, pre-write answers to:
1. "Add a new *type* of X" (new vehicle / payment / notification channel) → *should be a new class only*
2. "Make it distributed / multi-node" → what moves to Redis/DB, what breaks
3. "Two users act simultaneously" → your locking story
4. "Add persistence" → repository interface already isolates it, swap the impl
5. "Add an audit trail" → Observer / event stream
6. "What would you change if you had 2 more hours?" → **always have an honest answer ready**

> If your answer to #1 is "I'd modify the switch statement", your design failed Open-Closed.
> Fix it *while building*, not in the interview.

---

## 7. SELF-REVIEW RUBRIC (score every solution out of 20)

| Criterion | 0 | 1 | 2 |
|-----------|---|---|---|
| Compiles & runs with demo | no | partly | yes |
| Requirements covered | <60% | 60–85% | >85% |
| SOLID adherence | violations | minor | clean |
| Patterns used *appropriately* (not over-engineered) | none/forced | some | apt |
| Layering (model/service/repo) | flat | partial | clean |
| Naming & readability | poor | ok | excellent |
| Exceptions & edge cases | none | some | solid |
| Concurrency handled where needed | ignored | mentioned | implemented |
| Tests / demo quality | none | demo only | demo + tests |
| Finished within 90 min | no | ~ | yes |

Log the score in `06_Trackers.md`. **Target: 16+/20 consistently by Week 12.**
Any solution scoring <14 gets **redone from scratch two weeks later** — same rule as the DSA
Failure Queue.

---

## 8. RESOURCES (fixed)

- Your `../LLD_MasterSheet.md` (SOLID + 23 patterns) — **lookup during design, not before**
- Your `../LLD_Java_Spring_MasterSheet.md` — for Spring-flavoured LLD questions
- **Refactoring Guru** (refactoring.guru) — pattern refresher, 10 min max per pattern
- **"Head First Design Patterns"** — only if a pattern won't stick
- GitHub: search `low-level-design` repos to *compare after* your attempt, never before
- Your own repo — by Week 18 you'll have 20 solutions; skim it the night before an LLD round


