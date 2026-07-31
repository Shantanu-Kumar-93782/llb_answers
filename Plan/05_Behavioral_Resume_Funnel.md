# 05 — BEHAVIORAL · RESUME · APPLICATION FUNNEL · NEGOTIATION

> **Brutal truth:** You can be a 95th-percentile candidate and get zero offers if the funnel is
> empty. **Preparation without applications is a hobby.** This file is the business end of the plan.

---

## 1. RESUME (Week 0 v1 → Week 2 v2 → Week 6 final)

### Rules
- **1 page. Always.** 5.3 YOE fits on one page. Two pages signals junior thinking.
- **No photo, no address, no "Objective", no skill bars, no hobbies.**
- Order: `Name/Contact | Skills | Experience | Projects (1–2) | Education`
- ATS-safe: single column, standard fonts, no tables/text-boxes/icons, save as PDF.
- **Every bullet = XYZ format:** *"Accomplished [X] as measured by [Y] by doing [Z]."*

### Bullet transformation examples
| ❌ Weak | ✅ Strong |
|---------|----------|
| "Worked on microservices using Spring Boot" | "Decomposed a monolith into 6 Spring Boot services, cutting deploy time 45 min → 6 min and enabling independent releases for 4 teams" |
| "Used Kafka for messaging" | "Redesigned Kafka consumer groups with cooperative-sticky rebalancing, eliminating 20-min lag spikes and reducing p99 event latency from 4s → 300ms" |
| "Optimized database queries" | "Diagnosed N+1 and missing composite indexes via EXPLAIN analysis; reduced order-history API p99 from 1.8s → 210ms at 3× traffic" |
| "Deployed on AWS with Kubernetes" | "Built CI/CD on Jenkins + EKS with canary deploys and automated rollback, reducing failed-release MTTR from 90 → 12 min" |

### The numbers hunt (do this in Week 0 — 2 hours)
Open your Jira/PRs/dashboards and extract real numbers for:
latency before/after · throughput/QPS handled · data volume · cost saved · users served ·
team size influenced · deploy frequency · incident count/MTTR · test coverage · build time.

> **If you can't find a number, estimate defensibly and say "approximately".** No numbers = no SDE2.

### Target: 12–16 bullets total across roles. Top 3 bullets carry 80% of the weight.

### Versions to maintain
- `resume_backend.pdf` — default (Java/Spring/microservices/scale)
- `resume_distributed.pdf` — emphasises Kafka/streaming/consistency (for infra roles)
- Tailor the top 3 bullets per company. Nothing else.

### LinkedIn (Week 1)
- Headline: `Senior Software Engineer | Java · Spring Boot · Microservices · Kafka · AWS · K8s`
- About: 4 lines — what you build, at what scale, with what stack, what you're looking for
- Turn on **Open to Work (recruiters only)**
- Experience section mirrors the resume bullets
- **This is how recruiters find you passively. Worth 3 hours, once.**

---

## 2. BEHAVIORAL — 12 STAR STORIES (1 per week, Weeks 1–12)

### Format (write each in ~250 words, in `06_Trackers.md`)
```
SITUATION  1–2 lines of context, with scale ("a payments service handling 2M txns/day")
TASK       your specific responsibility, not the team's
ACTION     60% of the story. "I did…" not "we did…". Include the trade-off you weighed.
RESULT     quantified. Then: what you learned / would do differently.
```

### The 12 stories to write

| # | Story | Maps to |
|---|-------|---------|
| 1 | Hardest technical problem you solved | All companies |
| 2 | Production incident you owned end-to-end | Amazon: Ownership, Dive Deep |
| 3 | Conflict/disagreement with a colleague or manager | Amazon: Have Backbone; Meta: collaboration |
| 4 | A failure / something that went badly wrong | Amazon: Earn Trust; Google: humility |
| 5 | Delivered under a tight deadline with trade-offs | Amazon: Bias for Action, Deliver Results |
| 6 | Took initiative beyond your assigned scope | Amazon: Ownership; Google: Googleyness |
| 7 | Mentored/unblocked a teammate | Amazon: Hire & Develop; Staff-track signal |
| 8 | Influenced a technical decision without authority | Amazon: Are Right A Lot; Staff signal |
| 9 | Simplified something / removed complexity | Amazon: Invent & Simplify |
| 10 | Handled ambiguity with no clear requirements | Google GCA; Meta: ambiguity |
| 11 | Pushed back / said no to protect quality | Amazon: Insist on Highest Standards |
| 12 | Something you're most proud of | Universal closer |

### Company-specific behavioral notes
| Company | What they actually test | Prep |
|---------|-------------------------|------|
| **Amazon** | 16 Leadership Principles. **2 stories per LP minimum.** Bar Raiser round is all LPs. Expect 4–6 deep follow-ups per story ("what data?", "what did *you* do?", "what would you change?") | Build an LP × story matrix. Rehearse follow-ups, not just the story |
| **Google** | GCA (General Cognitive Ability) + Googleyness. Less scripted, more "how do you think" | Focus on structured reasoning + collaboration + humility |
| **Meta** | "Move fast", impact, conflict resolution, ownership of ambiguity. E5 needs *cross-team* impact | Emphasise scope and speed |
| **Microsoft** | Growth mindset, collaboration, customer obsession | Learning-from-failure stories land well |
| **Uber/Netflix** | High ownership, judgment under ambiguity, "did you make the call?" | Emphasise autonomous decisions |

### Practice protocol
- Weeks 1–12: write 1 story/week
- Weeks 13–18: **record yourself answering 3 random behavioral questions every Sunday**, watch back
- Target: any story delivered in **2.5–3 min**, no rambling, ends with a quantified result

### Your "Tell me about yourself" (90 seconds — memorise, don't sound memorised)
```
Present:  "I'm a backend engineer with 5+ years, currently at <company> building
           <domain> on Java/Spring Boot microservices handling <scale>."
Proof:    "Most recently I <biggest achievement with a number>."
Past:     "Before that I <one line of trajectory — increasing scope>."
Future:   "I'm looking for a role where I can own larger distributed systems end-to-end,
           which is why I was interested in <this team/company specifically>."
```

---

## 3. TARGET COMPANY LIST — 40 companies, 3 tiers

Build the actual list in `06_Trackers.md → Application Log` during Week 0.

### Tier 3 — Warm-ups (apply Week 7–8) — *practice reps, real offers possible*
Zeta · Navi · Groww · CRED · Meesho · Jupiter · Slice · Rapido · Dream11 · MPL ·
Sprinklr · Chargebee · Whatfix · Postman · BrowserStack · Innovaccer

**Purpose:** get interview reps in a real, high-stakes environment before your dream companies.
Nothing else simulates the adrenaline. **Do not skip this step — this is the #1 reason people
fail their dream loop.**

### Tier 2 — Strong (apply Week 9–11) — *excellent outcomes, high LLD/HLD weight (your strength)*
Flipkart · Swiggy · Zomato · Razorpay · PhonePe · Atlassian · Salesforce · Adobe · Oracle Cloud ·
VMware/Broadcom · Intuit · Expedia · Booking.com · Zeta · Arcesium · Goldman Sachs · Morgan Stanley ·
DE Shaw · SAP Labs · Walmart Global Tech · Target · Wayfair · ServiceNow · Confluent · MongoDB Inc

### Tier 1 — Dream (apply Week 12+) — *only after G2 gate is passed*
Google · Amazon · Microsoft · Meta · Uber · Netflix · Apple · LinkedIn · Salesforce (senior orgs) ·
Nvidia · Airbnb · Stripe · Databricks · Snowflake · Rubrik · Coinbase · Atlassian (senior)

---

## 4. ⚠️ COOLDOWN RULES — READ BEFORE APPLYING ANYWHERE

Failing a tier-1 loop locks you out. Sequence deliberately.

| Company | Cooldown after rejection |
|---------|--------------------------|
| Google | 6–12 months (varies by stage) |
| Meta | 12 months (post-onsite) |
| Amazon | 6 months (usually), 12 for some orgs |
| Microsoft | 6–12 months, team-dependent |
| Apple | Team-dependent, often 6 months |
| Uber / Netflix | ~12 months |

**Therefore the sequence is non-negotiable:**
```
Week 7–8   → Tier 3 (burn nothing valuable, gain real reps)
Week 9–11  → Tier 2 (get a real offer in hand = leverage + confidence)
Week 12+   → Tier 1, and within Tier 1: least-wanted first, MOST-WANTED LAST
```
> Interview your dream company **7th, not 1st.** By then you'll have done 6 real loops.

---

## 5. THE FUNNEL MATH (why volume matters)

```
100 applications
 → 25 recruiter screens          (25% — with referrals; ~8% cold)
 → 15 technical phone screens    (60%)
 →  7 onsites                    (45%)
 →  3 offers                     (40%)
```
**Your goal: 70–90 applications by Week 16.** Target 3+ offers so you can negotiate from strength.

### Weekly funnel quota (from Week 7)
- [ ] 6–8 applications
- [ ] **5 referral requests** (referral converts ~3× better than cold apply)
- [ ] 2 recruiter LinkedIn messages
- [ ] 1 follow-up on anything silent for 10+ days

### Referral outreach template (LinkedIn DM / email)
```
Hi <Name> — I'm a backend engineer with 5+ years on Java/Spring Boot microservices
(Kafka, K8s, AWS), currently at <company> working on <one line of relevant scope>.

I saw <Role> on <Company>'s careers page (req <ID>) and it lines up closely with
what I've been building — specifically <one specific overlap>.

Would you be open to referring me? Happy to send my resume and a 3-line summary you
can paste. Totally understand if you'd rather not — thanks either way!
```
Send to: ex-colleagues → college alumni (LinkedIn alumni filter) → 2nd-degree connections →
people who post about hiring. **Expect a 20–30% reply rate. That's normal. Send more.**

### Recruiter response template (when they reach out)
Always reply within 24h, always positive, always ask: *"What's the interview process and
timeline?"* and *"Which team/org is this for?"* — then schedule with **enough runway** to prep.
You can always ask for 2–3 more weeks. Recruiters expect it.

---

## 6. INTERVIEW LOGISTICS (protect your performance)

- **Schedule loops for 10:00–13:00 IST** if possible — your peak, and post-sleep.
- Never two onsites in one day. Never an onsite the day after a bad night's sleep.
- **Day before an interview:** zero new content. Re-read your Trigger Cards, your STAR stories,
  and one of your own design write-ups. Sleep 8h.
- **30 min before:** one easy warm-up problem, out loud, to get the verbal engine running.
- Setup check: stable internet, backup hotspot, charged laptop, quiet room, water,
  CoderPad/Excalidraw pre-opened, phone on silent.
- **Within 24h after every round:** write the post-mortem in `06_Trackers.md → Interview Log`.
  Questions asked, what you fumbled, what to drill. This compounds enormously across 20+ rounds.

---

## 7. NEGOTIATION (Weeks 19–22) — worth ₹10–40L, for 3 hours of work

### The 8 rules
1. **Never give the first number.** *"I'd rather focus on fit first — I'm confident you'll make a
   competitive offer. What's the range budgeted for this level?"*
2. **Never reveal your current CTC** (illegal to require in some places, always optional here).
   Deflect: *"My current comp isn't a good benchmark — I'm targeting market rate for this scope."*
3. **Create parallelism.** Cluster your loops so offers land within ~2 weeks of each other.
   One offer = a request. Two offers = a negotiation. Three = leverage.
4. **Everything is negotiable:** base, joining bonus, RSU grant, refresher policy, level,
   start date, relocation, WFH arrangement. **Push level hardest — it compounds forever.**
5. **Always counter, exactly once, with a reason.** *"I'm really excited about this team. Based on
   my other offer at X and the scope of this role, if you can get to <number>, I'll sign today."*
6. **Get it in writing** before resigning. Verbal offers evaporate.
7. **Down-level defence:** if offered SDE1/E4, ask *"What specifically was missing for the next
   level?"* — then either negotiate to the higher level with evidence, or decline and re-apply
   after cooldown. **Do not accept a down-level out of fear** — you're 27 with ₹50L and a WFH job.
8. **Your BATNA is genuinely strong.** ₹50L savings + ₹21 LPA WFH job = you can walk away from
   any offer. Negotiate like someone who can. Because you can.

### Benchmarks to research (Week 18, use levels.fyi + Blind)
| Level | India total comp (2026 ballpark) |
|-------|----------------------------------|
| Google L4 / Amazon SDE2 / Meta E4 | ₹45–75 L |
| Google L5 / Amazon SDE3 / Meta E5 | ₹80–140 L |
| Uber L5a / Microsoft 62–63 | ₹55–100 L |
| Tier-2 product (Flipkart/Swiggy/Razorpay SDE3) | ₹45–80 L |

**Your realistic target from ₹21 LPA:** ₹45–70 L total comp at SDE2/L4/E4. That's a **2.5–3.5×**
jump. Do not anchor on "a good hike from 21". Anchor on the **market rate for the level.**

---

## 8. THE WEEKLY FUNNEL RITUAL (Sunday, 30 min, from Week 7)

- [ ] Send this week's 6–8 applications
- [ ] Send 5 referral requests
- [ ] Follow up on everything silent >10 days
- [ ] Update the Application Log status column
- [ ] Write post-mortems for any interviews this week
- [ ] Rehearse 2 STAR stories on camera
- [ ] Check: is the pipeline going to be empty in 3 weeks? If yes → double next week's applications

