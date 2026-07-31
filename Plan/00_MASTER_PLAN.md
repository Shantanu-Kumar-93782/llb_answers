# 🎯 MASTER PLAN — SDE2 @ Tier-1 (FAANG+) in 5 Months

> **Start:** Sat 01-Aug-2026 · **Offer target:** by 31-Dec-2026 / early Jan-2027
> **Owner:** 27yo · 5.3 YOE · Java/Spring/Microservices · Jaipur · WFH ₹21 LPA · ₹50L runway
> **Status of prep:** 650+ DSA problems, NeetCode + Striver done, 60% cold-solve, LC ~1490, strong HLD/LLD

---

## 0. THE ONE-PARAGRAPH TRUTH

You are **not** a beginner who needs to learn. You are an **85%-built engineer with a
reliability problem**. 650 problems solved but only 60% cold-solve means your issue is
**retrieval under pressure**, not knowledge. Your master sheets prove you know the map;
you just haven't drilled the *recall*. So this plan is not "learn more content." This plan is:

> **Convert knowledge → reliable, timed, out-loud performance. Then run a high-volume
> application funnel so that luck has enough surface area to find you.**

Everything below serves those two sentences.

---

## 1. THE 5 TRACKS (and why each exists)

| # | Track | Weekly hours | Why it exists | Detail file |
|---|-------|--------------|---------------|-------------|
| 1 | **DSA Reliability** | 12–14 | 60% → 92% cold-solve. This is the #1 rejection cause. | `01_DSA_Execution_Plan.md` |
| 2 | **System Design (HLD)** | 6–7 | At 5.3 YOE this is *the* differentiating round for SDE2/E5. | `02_SystemDesign_Execution_Plan.md` |
| 3 | **LLD / Machine Coding** | 4–5 | Uber/Flipkart/Atlassian/Swiggy/Razorpay gate on this. Your strength — make it a weapon. | `03_LLD_Execution_Plan.md` |
| 4 | **Core Tech Depth** | 4–5 | Java/Spring/Kafka/DB/K8s/AWS — the "deep dive" round that separates SDE2 from SDE1. | `04_CoreTech_Deep_Dive_Plan.md` |
| 5 | **Funnel & Behavioral** | 3–4 | 0 interviews = 0 offers. Resume + referrals + STAR + negotiation. | `05_Behavioral_Resume_Funnel.md` |

**Total: ~32–35 hrs/week** on top of a full-time WFH job. That is aggressive but doable
*only* if sleep is protected. See §6.

Trackers: `06_Trackers.md` · Post-offer ladder: `07_Beyond_SDE2_Staff_Roadmap.md`

---

## 2. THE 5 PHASES

```
Week 0     │ Aug 1–2      │ DIAGNOSTIC & SETUP      │ Measure the truth. Build the machine.
Weeks 1–6  │ Aug 3–Sep 13 │ REBUILD & RELIABILITY   │ Pattern reload, failure queue, HLD fundamentals
Weeks 7–12 │ Sep 14–Oct 25│ DEPTH & SPEED           │ Hards, contests, 16 design cases, machine coding
Weeks13–18 │ Oct 26–Dec 6 │ INTERVIEW MODE          │ 2 mocks/week, warm-up onsites, real loops
Weeks19–22 │ Dec 7–Jan 3  │ TIER-1 LOOPS & CLOSE    │ Dream-company onsites, negotiation, decide
```

### Phase gates — do NOT advance until the gate is passed

| Gate | When | Criteria (all must be true) |
|------|------|------------------------------|
| **G1** | End Wk 6 | 80% cold-solve on Medium in ≤25 min · all 18 pattern trigger-cards written · 6 design fundamentals notes · resume v2 done |
| **G2** | End Wk 12 | 90% cold-solve Medium · 60% Hard · LC rating ≥1750 · 16 design case studies written · 10 LLD problems shipped in ≤90 min · 12 STAR stories written |
| **G3** | End Wk 18 | 12+ mock interviews done · ≥2 real onsites completed · ≥1 offer or strong-hire feedback in hand |
| **G4** | End Wk 22 | Offer(s) in hand, negotiated, signed |

If a gate fails → **repeat the last 2 weeks of that phase, do not skip forward.**
Skipping a failed gate is how the last attempt died.

---

## 3. WEEKLY RHYTHM (the shape of every week)

| Day | Morning 06:00–07:30 (90m) | Evening 20:00–21:30 (90m) | Late 21:30–22:00 |
|-----|---------------------------|----------------------------|------------------|
| **Mon** | DSA — Pattern A (3 problems, timed) | LLD — 1 design + code | Log + Anki |
| **Tue** | DSA — Pattern B (3 problems, timed) | HLD — 1 fundamentals topic / deep dive | Log + Anki |
| **Wed** | DSA — Failure Queue redo (D+3) | Core Tech — Java/JVM/Concurrency | Log + Anki |
| **Thu** | DSA — Pattern C (3 problems, timed) | HLD — 1 case study (part 1) | Log + Anki |
| **Fri** | DSA — Mixed random set (blind, 4 problems) | Core Tech — Spring/Kafka/DB/K8s/AWS | Log + Anki |
| **Sat** | **Contest 90m** + upsolve 90m | LLD machine coding **90m timed** | Weekly review |
| **Sun** | **Mock interview (DSA or Design) 60m** + feedback 30m | HLD case study (part 2) + write-up | **Weekly Review + Plan next week** |

**Work hours (09:30–18:30)** stay work hours. Lunch break = 15 min Anki flashcards only.
**Sunday evening 21:00–22:00 = Weekly Review.** Non-negotiable ritual (§5).

### Non-negotiable daily rules
1. **Every problem is timed and out loud.** Silent solving does not transfer to interviews.
2. **25-minute stuck rule.** Stuck at 25 min → write down *exactly* where you froze → read
   editorial → close it → re-solve from blank file *the same day* → add to Failure Queue.
3. **No new topic after 22:00.** Sleep beats one more problem. Always.
4. **One-line log per day** in `06_Trackers.md`. If it isn't logged, it didn't happen.
5. **Never solve a problem you already know the answer to** to feel good. That's procrastination.

---

## 4. THE THREE ENGINES THAT MAKE THIS WORK

### Engine 1 — The Failure Queue (your real syllabus)
Every problem you fail, hint-peek, or exceed time on goes into `06_Trackers.md → Failure Queue`
with: *problem · pattern · exact freeze point (one line)*.

Redo schedule: **D+3 → D+10 → D+30**. Three clean cold solves = graduated, removed.
> Your 40% failure rate IS the syllabus. New problems are a distraction until that shrinks.

### Engine 2 — Trigger Cards (retrieval, not knowledge)
For every pattern in `DSA_Patterns_MasterSheet.md` write ONE line:
`<problem signal seen in statement>  →  <pattern name>  →  <template in 3 lines>`

Examples already in your Recovery plan:
- `k-th largest/smallest` → heap
- `subarray with constraint` → sliding window
- `next greater/smaller` → monotonic stack
- `grid path / decision counting` → DP

Target: **60 trigger cards by end of Week 6.** Reviewed every morning in 5 minutes.
This single artifact is what moves 60% → 90%.

### Engine 3 — The Stuck Protocol (say it out loud, every time)
```
1. Restate + 2 examples + edge cases        (60s)
2. Brute force + its complexity              (60s)
3. "What is the state? What am I recomputing?"
4. "Can I trade space for time? Sort? Preprocess? Invariant?"
5. Name the pattern out loud
6. Code it. Dry-run on the example. THEN optimize.
```
Interviewers hire **methodical drivers**, not people who happen to have seen the problem.
Practice this even when you instantly know the answer.

---

## 5. WEEKLY REVIEW RITUAL (Sunday 21:00, 45 min)

Answer in `06_Trackers.md`:
1. Cold-solve % this week? (target trend: 60 → 92)
2. Which 3 patterns failed most? → they become next week's Mon/Tue/Thu patterns
3. Failure Queue size — growing or shrinking? (must shrink from Week 4 onward)
4. Hours actually logged vs 32 target
5. Funnel: applications sent / referrals asked / interviews scheduled
6. Sleep average + energy 1–5
7. **One thing I will do differently next week** (exactly one)

---

## 6. HEALTH — THE HARD CONSTRAINT (read this twice)

Your `Recovery_And_Comeback_Plan.md` is the *precondition* for this plan, not a competitor to it.
Years of broken sleep from a deviated septum is why 650 problems only converted to 60% recall —
**memory consolidation happens in deep sleep.** Fixing the airway may be worth more than 200
extra problems.

**Rules that override everything in this document:**
- If septoplasty is not yet done → **do it before Week 7.** Weeks 1–6 are the cheapest weeks to lose.
- Post-op weeks 1–3: this plan drops to **"Recovery Mode"** — 45 min/day of Trigger Cards + reading
  only. Zero timed work. Then resume at the week you paused.
- **Sleep 7.5–8h, fixed 22:30–06:00.** No screens after 22:15. This is a *performance* rule.
- Gym / walk 4×/week (once surgeon clears). 20 min sunlight daily.
- Bloodwork (TSH, Vit D, B12, Ferritin, CBC, HbA1c) done and deficiencies corrected by Week 4.
  Low D3/B12/ferritin *feels exactly like* "I'm not smart enough." Rule it out with a lab test.

> Guilt is not a strategy. A rested brain solving 2 problems beats an exhausted one failing 6.

---

## 7. THE KILL LIST (things you must STOP doing until Jan 2027)

- ❌ Gift App / SaaS side project → **paused until 05-Jan-2027.** (`Gift_App_SaaS_Plan.md` is a
  *post-offer* project. It is a great idea and it will still be there.)
- ❌ Reading new master sheets cover-to-cover. Sheets are **lookup references now**, not reading material.
- ❌ Starting new courses/playlists. You have enough material for 3 years.
- ❌ Solving Easy problems for comfort.
- ❌ Checking LeetCode contest rating more than once a week.
- ❌ Applying to dream companies before Week 12 (cooldown burn — see `05_...Funnel.md §4`).

---

## 8. DATED CALENDAR — 22 WEEKS

| Wk | Dates | DSA focus | HLD | LLD | Core Tech | Funnel |
|----|-------|-----------|-----|-----|-----------|--------|
| **0** | Aug 1–2 | Diagnostic test (§9) | Setup | — | — | Resume v1 + target list |
| 1 | Aug 3–9 | Arrays, 2-Pointer, Sliding Window | Networking + Caching | SOLID recall + Parking Lot | JVM & GC | LinkedIn overhaul |
| 2 | Aug 10–16 | Binary Search, Sorting, Intervals | Storage/DB internals, Sharding | Creational patterns + Vending Machine | Collections + HashMap internals | Resume v2 + 5 referral DMs |
| 3 | Aug 17–23 | Stack, Monotonic Stack, Queue | Replication, CAP/PACELC, Consistent Hashing | Structural patterns + Elevator | Concurrency I (threads, locks, CAS) | 5 referral DMs |
| 4 | Aug 24–30 | Linked List, Trees I (traversal, BST) | Load Balancing, Msg Queues, Kafka internals | Behavioural patterns + Snake&Ladder | Concurrency II (executors, CompletableFuture) | 5 referrals · 3 STAR stories |
| 5 | Aug 31–Sep 6 | Trees II (LCA, path, Morris), Tries | Case 1: URL Shortener · Case 2: Rate Limiter | Splitwise | Spring Boot internals + AOP | 5 referrals · 3 STAR |
| 6 | Sep 7–13 | Graphs I (BFS/DFS/topo) | Case 3: Key-Value Store · Case 4: Web Crawler | BookMyShow | Spring Data/JPA, N+1, txns | **GATE G1** · resume final |
| 7 | Sep 14–20 | Graphs II (Dijkstra, MST, Union-Find) | Case 5: News Feed · Case 6: Chat/WhatsApp | Tic-Tac-Toe + Chess | MySQL indexes, isolation levels | Apply: 10 Tier-3 |
| 8 | Sep 21–27 | DP I (1-D, knapsack) | Case 7: Notification Sys · Case 8: Typeahead | LRU + TTL Cache | Redis internals & patterns | Apply: 10 Tier-3 · mock #1 |
| 9 | Sep 28–Oct 4 | DP II (2-D, grid, LCS/edit) | Case 9: YouTube/Netflix · Case 10: Dropbox | Rate Limiter (code) | Kafka deep dive | Apply: 10 Tier-2 |
| 10 | Oct 5–11 | DP III (intervals, bitmask, digit) | Case 11: Uber · Case 12: Google Maps | Food Delivery | Microservices patterns, Saga, CQRS | Apply: 10 Tier-2 · mock #2 |
| 11 | Oct 12–18 | Heap, Greedy, Bit Manipulation | Case 13: Ad Click Aggregator · Case 14: Metrics/Monitoring | Logging Framework | Docker + K8s | Apply: 10 Tier-2 |
| 12 | Oct 19–25 | Backtracking, Strings, advanced | Case 15: Payment Sys · Case 16: Distributed Job Scheduler | In-memory DB | AWS core + observability | **GATE G2** · Apply Tier-1 wave 1 |
| 13 | Oct 26–Nov 1 | Company-tagged: Amazon | Case 17: S3/Object Store | Mock LLD ×2 | System design deep-dive drills | Phone screens · mock ×2 |
| 14 | Nov 2–8 | Company-tagged: Google | Case 18: Ticketmaster | Mock LLD ×2 | — | Onsites (Tier-3) · mock ×2 |
| 15 | Nov 9–15 | Company-tagged: Meta | Case 19: Distributed Cache | Mock LLD ×2 | — | Onsites · Tier-1 wave 2 |
| 16 | Nov 16–22 | Company-tagged: Uber/MSFT | Case 20: Live Streaming | Mock LLD ×2 | — | Onsites · mock ×2 |
| 17 | Nov 23–29 | Weak-pattern blitz | Case 21: Digital Wallet | Mock LLD | Behavioural rehearsal | Onsites |
| 18 | Nov 30–Dec 6 | Weak-pattern blitz | Case 22: Stock Exchange | Mock LLD | Behavioural rehearsal | **GATE G3** |
| 19 | Dec 7–13 | Maintenance 60m/day | Case 23: Nearby Friends | Refresh | — | **Tier-1 loops** |
| 20 | Dec 14–20 | Maintenance | Case 24: Collaborative Editor | Refresh | — | **Tier-1 loops** |
| 21 | Dec 21–27 | Maintenance | Stretch cases | Refresh | — | Loops · negotiation prep |
| 22 | Dec 28–Jan 3 | Maintenance | Stretch cases | Refresh | — | **Negotiate · sign · GATE G4** |

---

## 9. WEEK 0 — DO THIS WEEKEND (Aug 1–2, 2026)

This is the most important 2 days of the plan. **Measure before you train.**

### Saturday Aug 1
- [ ] **Diagnostic DSA test — 3 hours, exam conditions, no hints, no IDE autocomplete.**
      Pick 5 **unseen** problems: 1 Easy, 3 Medium, 1 Hard. Time each. Record honest results in
      `06_Trackers.md → Baseline`. *(How to pick: LeetCode → Problems → filter `Status = Todo`,
      sort by Acceptance, pick randomly. Or use a recent contest Div-2 set you never attempted.
      The only requirement is that you have never seen them.)*
- [ ] **Diagnostic HLD — 45 min:** design a URL shortener out loud, record yourself on phone.
      Watch it back. Note: did you gather requirements? do estimations? give numbers?
- [ ] **Diagnostic LLD — 90 min:** code a Parking Lot system from blank file. Compiles + runs?
- [ ] Score yourself 1–10 on each of the 5 tracks → this sets your personal emphasis.

### Sunday Aug 2
- [ ] Set up tooling (§10)
- [ ] Write resume v1 (see `05_Behavioral_Resume_Funnel.md §1`)
- [ ] Build the 40-company target list (`05_... §3`)
- [ ] Block all study slots in Google Calendar for 22 weeks (recurring events, right now)
- [ ] Tell your fiancée / family the plan and the timeline. Social accountability doubles adherence.
- [ ] Book ENT follow-up / surgery date if pending (§6)

---

## 10. TOOLING SETUP (once, Week 0)

| Purpose | Tool | Notes |
|---------|------|-------|
| Problem tracking | LeetCode lists + `06_Trackers.md` | Lists: `Failure Queue`, `Graduated`, `Company-Tagged` |
| Spaced repetition | **Anki** (free) | Decks: `DSA-Triggers`, `Java-Core`, `Spring`, `SysDesign-Numbers`, `Behavioral` |
| Timed practice | Physical timer / `pomofocus.io` | Never use phone (notification risk) |
| Design diagrams | **Excalidraw** | Same tool used in most virtual onsites — build muscle memory |
| Mock interviews | **Pramp** (free), **interviewing.io** (paid, worth it), **meetapro.com**, peers | Book Sundays in advance |
| Recording | Phone / OBS | Record every mock. Watching yourself is brutal and the fastest fix |
| Contests | LeetCode Weekly (Sun 08:00 IST) + Biweekly (alt Sat 20:00 IST) | Virtual if live missed |
| Code repo | Private GitHub `interview-prep` | Every LLD solution committed. Doubles as portfolio |

### Books (reference only, do NOT read cover-to-cover)
- *Designing Data-Intensive Applications* — Ch 1,3,5,6,7,8,9,11 only (mapped in `02_...`)
- *System Design Interview Vol 1 & 2* — Alex Xu (your case-study spine)
- *Grokking the Advanced System Design* — for the deep-dive round
- Your own `Java_MasterSheet.md` / `Spring_MasterSheet.md` — lookup only

---

## 11. SUCCESS METRICS (track weekly in `06_Trackers.md`)

| Metric | Wk 0 baseline | Wk 6 | Wk 12 | Wk 18 |
|--------|---------------|------|-------|-------|
| Cold-solve % (Medium, ≤25 min) | 60% | 80% | 90% | 92% |
| Cold-solve % (Hard, ≤40 min) | ~25% | 40% | 60% | 70% |
| LeetCode contest rating | 1490 | 1600 | 1750 | 1900 |
| Contests attempted | 3 | 9 | 15 | 21 |
| Failure Queue size | — | peak | ↓ 50% | < 15 |
| HLD case studies written | 0 | 4 | 16 | 22 |
| LLD problems shipped ≤90 min | 0 | 5 | 12 | 20 |
| Mock interviews done | 0 | 2 | 6 | 14 |
| Applications sent | 0 | 0 | 40 | 70 |
| Onsites completed | 0 | 0 | 1 | 4+ |

---

## 12. IF THINGS GO WRONG — PRE-COMMITTED RESPONSES

| Situation | Response (decided now, so you don't decide when tired) |
|-----------|--------------------------------------------------------|
| Missed 2 days | Skip nothing. Resume today's slot. Do NOT "catch up" — that spirals. |
| Missed a whole week | Rerun the same week. The calendar shifts by 1 week. That's fine. |
| Surgery happens mid-plan | Recovery Mode 3 weeks (§6), calendar shifts by 3 weeks, offer target → Feb 2027. Still fine. |
| Contest rating drops | Ignore it entirely. Only the upsolve matters. |
| Rejected after onsite | Mandatory: write the post-mortem within 24h in `06_Trackers.md → Interview Log`. Then next application within 48h. |
| Feeling "too old / too late" at 27 with 5.3 YOE | Factually wrong. Median FAANG SDE2/E5 hire has 5–8 YOE. You are exactly in the window. |
| Burnout signs (3+ days of dread) | Take 2 full days off, zero guilt, then restart at 60% volume for a week. |
| Everything falling apart feeling | Read `Recovery_And_Comeback_Plan.md` §Safety note. iCall **9152987821** · Tele-MANAS **14416**. |

---

## 13. THE ONLY THING THAT MATTERS TODAY

> Open `06_Trackers.md`. Take the Week 0 diagnostic. Log the honest numbers.
> Everything else follows from knowing where you actually stand.

You have ₹50L of runway, a WFH job that gives you time, 5.3 years of real engineering,
strong design skills, and 650 problems already in your head. You are not starting.
**You are finishing.**


