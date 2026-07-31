# 09 — CONTEST & VIRTUAL CONTEST STRATEGY
### Direct answer: **should you do a virtual contest every day?**

---

# THE ANSWER: **NO. Not daily. Here's what to do instead.**

> **Short version:**
> **Daily virtual contest = ❌ wrong for you.**
> **Daily 45-minute "micro-contest" (2 random problems, timed, blind) = ✅ absolutely yes — it's already Friday's slot, and I'm upgrading it to every weekday.**
> **Full 90-minute virtual contests = 1–2 per week, ALWAYS with the upsolve attached.**

---

## 1. WHY DAILY FULL VIRTUAL CONTESTS WOULD ACTIVELY HURT YOU

This isn't a general rule — it's specific to **your** diagnosis.

| Reason | Explanation |
|---|---|
| **1. The upsolve is where 80% of the learning happens, and it takes longer than the contest** | A 90-min contest where you solve 2/4 generates **2–3 hours** of upsolve work. Daily contests = 1.5h contest + 2.5h upsolve = 4h/day on DSA alone. You don't have that, and it would crowd out System Design, LLD and Core Tech entirely. |
| **2. Daily contests = daily new problems = your existing failure mode** | You already have **650 problems at 60% cold-solve**. That number is the signature of *high intake, low consolidation*. Adding 4 fresh problems a day makes the ratio **worse**, not better. Your bottleneck is retention, not exposure. |
| **3. Contests optimise for speed on easy-medium; interviews optimise for depth + communication** | Contest skill and interview skill overlap ~60%. The other 40% (narrating, clarifying, dry-running, handling hints) is **actively untrained** by contests — you solve silently and fast. Over-indexing on contests can even make you *worse* at interviews, because you learn to skip the explanation. |
| **4. Cognitive load and burnout** | A rated contest is a genuine stress event. Daily stress events with no recovery is exactly how the last attempt ended. Given you're recovering from years of poor sleep, this matters more for you than for most people. |
| **5. Rating chasing is a trap** | LC/CF rating is **not a hiring signal.** No FAANG interviewer will ever see it. It is a *training instrument only*. Daily contests turn it into an identity metric, and a bad day starts feeling like a verdict. |

### The one exception where daily contests WOULD be right
If your problem were *"I know the patterns cold, I just freeze on unseen problems and I'm slow"* —
i.e. **95% cold-solve but bad at contests**. That is a speed/nerve problem, and volume fixes it.

**That is not your problem.** Yours is 60% cold-solve = a **consolidation** problem.
Different disease, different medicine.

---

## 2. WHAT TO DO INSTEAD — THE DAILY MICRO-CONTEST (45 min, Mon–Fri)

This gives you ~90% of the contest benefit at ~35% of the cost. **This replaces "just solving problems" — same time slot, far higher transfer.**

```
PROTOCOL — "Micro-Contest", 45 minutes, every weekday morning

Setup   : 2 problems you have NEVER seen. Tags HIDDEN. Difficulty labels HIDDEN if possible.
          Timer visible. Phone in another room. Nothing else open.
Rules   : 22 min for problem 1, 23 min for problem 2. Hard stop each.
          No hints, no editorial, no Google, no autocomplete-driven "let me just try".
          Narrate OUT LOUD the whole time (this is the part contests never train).
Scoring : Solved cold in time = C. Solved slow = CS. Peeked = H. Failed = F.
After   : 15 min — for each F/H, write the freeze point in ≤10 words → Failure Queue.
          Then re-solve ONE of them from a blank file the same evening.
```

**Where to get "unseen with hidden tags":**
- LeetCode → Problems → filter `Status = Todo` → **sort by Frequency** (Premium) or Random
- Old LeetCode contests you never attempted (Weekly 250–420) — take just Q2 + Q3
- Codeforces problemset filtered to rating **1400–1700**, sorted by solved count, tags collapsed
- Your own `08_DSA_Pattern_Recognition_Bible.md` Part 7 list, if you've forgotten the answers

> **Why this beats a daily contest:** same time pressure, same blind recognition test, but only
> 2 problems — so you can actually *process* the failures the same day. Processing is the whole game.

---

## 3. THE FULL CONTEST CADENCE (what you actually commit to)

| Phase | Live/rated contests | Virtual contests | Micro-contests | Total DSA hrs/wk |
|-------|---------------------|------------------|----------------|------------------|
| **Weeks 1–6** (Rebuild) | 1/week (LC Weekly, Sunday) | **0** | 5/week | ~12 |
| **Weeks 7–12** (Depth) | 1–2/week (LC Weekly + Biweekly) | **1/week** (Wed or Sat) | 5/week | ~14 |
| **Weeks 13–18** (Interview mode) | 1/week | **1/week**, company-flavoured | 5/week | ~12 |
| **Weeks 19–22** (Loops) | 1/week only if no interview that week | **0** | 3/week | ~6 |

### Fixed schedule
| When | What |
|------|------|
| **Sunday 08:00 IST** | **LeetCode Weekly Contest** — live, rated. Non-negotiable. |
| **Alternate Saturdays 20:00 IST** | **LeetCode Biweekly Contest** — live, rated |
| **Wednesday evening (Wk 7+)** | Virtual contest, 90 min — *only if last week's upsolve is complete* |
| **Every fortnight from Wk 5** | 1× **Codeforces Div 2 / Div 3** — trains a genuinely different muscle (ad-hoc reasoning, constructive proofs, no "known pattern"). Excellent for Google-style problems. |
| **Every weekday morning** | 45-min micro-contest (§2) |

**Target: 21 rated contests + ~12 virtuals by Week 18.**
**The metric that matters is attendance, not rating. Rating follows attendance.**

---

## 4. THE UPSOLVE PROTOCOL (worth 3× the contest itself — never skip)

> 🔒 **THE HARD RULE: Never enter a contest you don't have time to upsolve.**
> An un-upsolved contest is worse than no contest — it teaches your brain that failing is normal
> and consequence-free.

Same day as the contest, or the next morning at the latest:

```
PASS 1 — Solo retry (30–45 min per unsolved problem)
         Blank file. No editorial. The clock pressure is gone — now can you actually do it?
         → If you solve it now: it was a SPEED problem. Log it as such.
         → If you still can't: it's a KNOWLEDGE problem. Go to Pass 2.

PASS 2 — Read ONLY the first hint / the approach paragraph. Not the code. Close it.
         Try again from a blank file.
         → This is where most learning happens. Protect it: do NOT read the full solution yet.

PASS 3 — Read the full editorial. Then CLOSE IT and type the solution from memory.
         Not copy-paste. Typing it from understanding is the point.

PASS 4 — Write the entry:
         Problem | Pattern | Freeze point (≤10 words) | speed-problem or knowledge-problem?
         → Failure Queue in 06_Trackers.md, scheduled D+3 / D+10 / D+30.
         → If the pattern isn't in 08_DSA_Pattern_Recognition_Bible.md Part 3 → ADD IT.
```

**The speed vs knowledge split is the single most useful diagnostic you'll collect.**
- Mostly **speed** problems → you're ready; increase contest frequency, reduce study.
- Mostly **knowledge** problems → cut contests to 1/week; spend the time in the Bible + Failure Queue.

Track the ratio weekly in `06_Trackers.md § E`.

---

## 5. HOW TO PICK A VIRTUAL CONTEST (so it trains the right thing)

| Your goal that week | Pick |
|---|---|
| Interview realism (default) | **LeetCode Weekly 300–420**, virtual, full 90 min |
| Pattern breadth | An older LC Weekly (contests 180–260) — more classic-pattern-heavy |
| Ad-hoc reasoning / Google-style | **Codeforces Div 2** (do A–D) or Div 3 (A–E) |
| Speed under pressure (Meta prep) | LC Weekly but give yourself **60 min instead of 90** |
| Hard-problem stamina | LC Biweekly Q3+Q4 only, 75 min |
| Company-flavoured (Wk 13+) | Build a custom 4-problem set from that company's tagged list, self-timed 90 min |

**Rules for a virtual to count as real:**
- Full timer, no pausing, no bathroom-break thinking
- Tags and difficulty hidden
- No editorial, no discussion tab, no AI, no Google — **not even for syntax**
- Submit properly and record the score as if it were rated

---

## 6. WHAT CONTESTS DO AND DON'T TRANSFER TO INTERVIEWS

| ✅ Transfers well | ❌ Doesn't transfer (must be trained separately) |
|---|---|
| Fast pattern recognition | Explaining your approach before coding |
| Reading constraints → picking complexity | Asking clarifying questions |
| Coding speed and typing accuracy | Dry-running on an example out loud |
| Debugging under time pressure | Taking and using a hint gracefully |
| Not panicking when stuck | Discussing trade-offs and follow-ups |
| Handling unseen problems | Writing clean, named, readable code |
| Edge-case instinct | Collaborating with the interviewer |

> **Consequence:** contests alone will *never* get you hired. Which is why the plan pairs them with
> **Sunday mocks** (`00_MASTER_PLAN.md §3`) and the **out-loud interview script**
> (`01_DSA_Execution_Plan.md §8`). Contest = raw ability. Mock = the actual exam.

---

## 7. RATING — WHAT IT MEANS AND WHAT IT DOESN'T

| LeetCode rating | Rough meaning |
|---|---|
| 1500 | Solves Q1+Q2 reliably. ~ Medium-comfortable. |
| 1700 | Solves Q1–Q3 most weeks. **Enough for most SDE2 loops.** |
| 1900 (Knight) | Solid Q3, occasional Q4. **Comfortable for any FAANG SDE2 round.** |
| 2100+ (Guardian) | Q4 regularly. Far beyond what an SDE2 interview requires. |

**Your trajectory target:** 1490 → 1600 (Wk 6) → 1750 (Wk 12) → 1900 (Wk 18).

### Three honest truths about the number
1. **No interviewer will ever see it.** It is a training thermometer, nothing more.
2. **1750 is already sufficient** for a Google/Amazon/Meta SDE2 coding round. Everything above
   that is insurance, not a requirement. Don't delay applications waiting for a number.
3. **You're at 1490 after 3 contests.** That's a *sample size* of 3. Rating is meaningless below
   ~10 contests. Yours will move a lot in the first 8 weeks simply from attendance.

**Rule: check your rating exactly once a week, during the Sunday review. Never during the week.**

---

## 8. CONTEST-DAY PROTOCOL (the 6 rules that add ~150 rating for free)

1. **Read ALL problems first (3 min).** Solve in order of *your* difficulty, not the listed order.
   Q3 is sometimes easier than Q2 for your specific pattern strengths.
2. **Q1 in under 8 minutes.** If it's taking longer, you misread it. Re-read the constraints.
3. **Check constraints before choosing an approach.** (`08_..._Bible.md` Part 1). Most contest
   failures are "wrote the O(n²) when n = 1e5".
4. **Brute-force n=1,2,3 by hand when stuck.** The pattern almost always falls out of small cases.
5. **Don't over-invest in one problem.** If 20 min pass with no progress, move on. Come back later.
6. **Submit partial/brute-force if nothing else works.** A TLE submission still tells you your
   understanding was right — valuable data for the upsolve.

---

## 9. RED FLAGS — when to CUT contests immediately

Stop or reduce to zero if any of these are true:

- ❌ You have **unupsolved contests piling up** (2+ backlog) → stop until the backlog is clear
- ❌ Your **Failure Queue is growing** week over week → you're taking in more than you can consolidate
- ❌ You feel **dread** before a contest, or your mood for the day depends on the rank
- ❌ Contest time is eating **System Design / LLD** slots → those matter more for SDE2 than rating
- ❌ You're **checking the rating more than once a week**
- ❌ **Sleep is under 7h** → skip the contest, sleep. Non-negotiable (`00_MASTER_PLAN.md §6`).

> Contests are a **tool**, not a scoreboard, not an identity, and not a measure of your worth
> as an engineer. The moment they become any of those, they've stopped working.

---

## 10. ONE-PAGE SUMMARY (pin this)

```
DAILY   (Mon–Fri, 45 min)  ✅  MICRO-CONTEST: 2 unseen problems, hidden tags, timed,
                               out loud, then log every failure. ← your real daily contest

WEEKLY  (Sun 08:00 IST)    ✅  LeetCode Weekly, LIVE, rated. Never skip.
        (alt Sat 20:00)    ✅  LeetCode Biweekly, live.
        (Wed, Wk 7+)       ✅  ONE virtual contest — only if last upsolve is done.
        (fortnightly)      ✅  Codeforces Div 2/3 — trains ad-hoc reasoning.

DAILY FULL VIRTUAL CONTEST ❌  NO. It multiplies intake and starves consolidation —
                               which is precisely the failure mode that produced
                               650 problems at 60% cold-solve.

THE IRON RULE              🔒  Never enter a contest you don't have time to upsolve.
                               The upsolve IS the training. The contest is just the diagnostic.

THE METRIC THAT MATTERS    📊  Cold-solve % (60 → 92), not rating.
                               Attendance (21 contests), not rank.
```

