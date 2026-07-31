# Design Notes

One file per system design case study, written by **you**, in **your own words**.

**Naming:** `01_url_shortener.md`, `02_rate_limiter.md`, … `24_collaborative_editor.md`
(list and order in `../02_SystemDesign_Execution_Plan.md §5`)

## Template — copy this for every case study

```markdown
# <System Name>
> Learn pass: <date> · Timed 45-min pass: <date> · Self-score: __/10

## 1. Requirements (0–5 min)
**Functional**
-
**Non-functional** (scale / latency / consistency / availability)
-
**Non-goals**
-

## 2. Estimation (5–10 min)
- DAU: ___ → actions/day: ___ → QPS avg: ___ → **QPS peak (×3): ___**
- Storage/day: ___ → /year: ___ → ×3 replication: ___
- Bandwidth: ___
- Servers / shards needed: ___

## 3. API + Data Model (10–15 min)
| Method | Path | Params | Returns |
|--------|------|--------|---------|
| | | | |

**Entities / schema:**
**SQL or NoSQL, and WHY:**
**Indexes / partition key, and WHY:**

## 4. High-Level Design (15–28 min)
<Excalidraw export or ASCII>

**Write path (end to end):**
**Read path (end to end):**

## 5. Deep Dives (28–40 min) — pick 2
### Deep dive 1: <topic>
- Problem:
- Options considered:
- **Decision + trade-off accepted:**

### Deep dive 2: <topic>
- Problem:
- Options considered:
- **Decision + trade-off accepted:**

## 6. Bottlenecks & Failure (40–45 min)
- SPOFs:
- What breaks first at 10×:
- Hot key / celebrity handling:
- Graceful degradation:
- Key metrics & alerts:

## 7. Self-review
- Did I do estimation before designing? Y/N
- Did I justify every component with "because…"? Y/N
- Did I volunteer deep dives unprompted? Y/N
- **Where I fumbled:**
- **Redo this case on:** <date>
```

> Re-reading your own 24 write-ups in Weeks 19–22 is your entire revision material.
> Written-by-you beats read-from-a-book by an enormous margin for recall.

