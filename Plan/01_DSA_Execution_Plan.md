# 01 — DSA EXECUTION PLAN (Reliability Engine)

> **Mission:** 60% cold-solve → **92%** by Week 18. Not "solve more". **Fail less.**
> Reference sheet: `../DSA_Patterns_MasterSheet.md` (133 patterns) — *lookup only, don't re-read.*
> **Knowledge layer:** `08_DSA_Pattern_Recognition_Bible.md` — triggers, 40 Java templates,
> adaptation twists, self-test. **Contest cadence:** `09_Contest_Strategy.md`.

---

## 1. THE CORE DIAGNOSIS

650 problems + 60% cold-solve = **you have the knowledge, not the retrieval.**
Cause: too much *recognition* practice (you read editorial, nod, move on) and too little
*recall* practice (blank file, timer running, no hints).

Fix = three mechanisms, run relentlessly:

| Mechanism | What it fixes |
|-----------|---------------|
| **Timed cold solves** | Recall under pressure |
| **Failure Queue (D+3/D+10/D+30)** | Turns 40% failures into permanent wins |
| **Trigger Cards** | Statement → pattern in <60 seconds |

---

## 2. DAILY DSA PROTOCOL (90 min, mornings 06:00–07:30)

> **This slot IS the daily "micro-contest"** described in `09_Contest_Strategy.md §2`.
> Treat every morning as a 45-minute rated contest with 2 problems. That is your answer to
> "should I do a virtual contest daily" — **yes, in this compressed form; no, as a full 90-min contest.**

```
00:00–00:05  Review Trigger Cards (Anki deck DSA-Triggers, 20 cards — source: 08_..._Bible.md Part 3)
00:05–00:30  Problem 1  — timed 25 min, out loud, blank file, TAGS HIDDEN
00:30–00:35  Log result (solved cold / hint / failed) + freeze point if failed
00:35–01:00  Problem 2  — timed 25 min
01:00–01:05  Log
01:05–01:25  Either Problem 3 (if both were fast) OR editorial + immediate re-solve of a failure
01:25–01:30  Write/update Trigger Card for today's pattern
```

### Rules
- **Blank file always.** No looking at your old solution first. Ever.
- **Out loud always.** Narrate: restate → brute force → complexity → pattern → code → dry run.
- **25-min hard stop** on Mediums, **40-min** on Hards. Timer, not vibes.
- On failure: write the freeze point in **≤10 words**. e.g. *"couldn't define dp state"*,
  *"missed that array was sorted"*, *"off-by-one in window shrink"*.
  These 10 words are worth more than the solution.
- **Re-solve every failure the same evening** from blank file, then queue it at D+3.

### Classification (log this for every problem)
| Code | Meaning |
|------|---------|
| **C** | Cold solve, optimal, within time |
| **CS** | Cold solve but slow (over time) |
| **H** | Needed a hint / peeked at tags |
| **F** | Failed — read editorial |

Cold-solve % = `C / (C + CS + H + F)`. Track weekly.

---

## 3. THE FAILURE QUEUE (your real syllabus)

Live in `06_Trackers.md`. Format:

```
| Problem | Pattern | Freeze point (≤10 words) | Fail date | D+3 | D+10 | D+30 | Status |
```

**Graduation rule:** 3 consecutive clean cold solves (D+3, D+10, D+30) → move to `Graduated`.
Any failure in the chain → reset the chain to D+3.

**Wednesday morning = Failure Queue day** (no new problems, only redos).
From Week 8, add a second redo slot on Friday if queue > 25 items.

> If your queue is not shrinking by Week 6, you are adding new problems too fast.
> Cut new-problem volume by half. Reliability > volume. Always.

---

## 4. TRIGGER CARDS — build 60 of them by Week 6

One card per pattern. Front = the *signal in the problem statement*. Back = pattern + 3-line template.

**Starter set (write these in Week 1, then add ~10/week):**

| Signal in statement | → Pattern |
|---------------------|-----------|
| "k-th largest / smallest / top-k" | Heap (size k, opposite comparator) |
| "subarray/substring with constraint" | Sliding Window (expand right, shrink left) |
| "next greater / smaller / span" | Monotonic Stack |
| "sorted array + find pair/triplet" | Two Pointers |
| "minimum/maximum X such that feasible(X)" | Binary Search on Answer |
| "count ways / min cost to reach" | DP (define state = position + constraint) |
| "grid + shortest path, unweighted" | BFS |
| "grid + all paths / islands / flood" | DFS |
| "weighted shortest path, non-negative" | Dijkstra |
| "dependencies / prerequisites / ordering" | Topological Sort |
| "connected / merge groups / same set" | Union-Find (DSU) |
| "generate all combinations/permutations" | Backtracking |
| "prefix / dictionary / autocomplete" | Trie |
| "range sum / range update" | Prefix Sum / Fenwick / Segment Tree |
| "median / running stats of stream" | Two Heaps |
| "cycle in linked list / find duplicate" | Floyd's Fast-Slow |
| "intervals overlapping / merge / rooms" | Sort by start + heap on end |
| "palindrome substrings" | Expand-around-center / DP |
| "XOR / count bits / subset via mask" | Bit Manipulation |
| "in-place, O(1) space, tree traversal" | Morris Traversal |
| "string matching / repeated substring" | KMP / Rolling Hash |
| "maximize/minimize by picking locally" | Greedy (prove exchange argument!) |
| "at most k / exactly k" | Sliding Window: `atMost(k) - atMost(k-1)` |
| "matrix as graph, cells as nodes" | Multi-source BFS |
| "tree path sums / diameter" | DFS returning (best-through-node, best-downward) |
| "LRU / LFU / O(1) get+put" | HashMap + Doubly Linked List |

**Review cadence:** 20 cards every morning (5 min, Anki). By Week 12 you should name the pattern
in <30 seconds for 90% of Mediums.

---

## 5. WEEK-BY-WEEK PATTERN SCHEDULE

Each week: **Mon / Tue / Thu = pattern days** (3 problems each), **Wed = Failure Queue**,
**Fri = blind mixed set** (4 problems, random, no topic label — this simulates the interview),
**Sat = contest + upsolve**, **Sun = mock**.

> **Blind Friday is the most valuable day of the week.** Use LeetCode "Random Problem" or a
> tag-hidden list. Recognizing a pattern when you know the topic is worthless.

### PHASE 1 — Weeks 1–6: Pattern Reload (Aug 3 – Sep 13)

| Wk | Mon | Tue | Thu | Must-do problems (representative — from your MasterSheet sections) |
|----|-----|-----|-----|---------------------------------------------------------------------|
| **1** | Two Pointers | Sliding Window (fixed) | Sliding Window (variable) | 3Sum · Container With Most Water · Trapping Rain Water · Longest Substring Without Repeat · Minimum Window Substring · Sliding Window Maximum · Fruit Into Baskets · Subarrays with K Different Integers |
| **2** | Binary Search (classic + rotated) | Binary Search on Answer | Intervals + Sorting | Search in Rotated Sorted Array · Find Min in Rotated · Median of Two Sorted Arrays · Koko Eating Bananas · Split Array Largest Sum · Capacity to Ship Packages · Merge Intervals · Insert Interval · Meeting Rooms II · Non-overlapping Intervals |
| **3** | Stack + Monotonic Stack | Prefix Sum + Hashing | Matrix / Simulation | Largest Rectangle in Histogram · Daily Temperatures · Next Greater Element II · Remove K Digits · Subarray Sum Equals K · Contiguous Array · Product of Array Except Self · Spiral Matrix · Rotate Image · Set Matrix Zeroes |
| **4** | Linked List | Trees I: traversal + BST | Trees II: recursion patterns | Reverse Nodes in k-Group · Merge k Sorted Lists · LRU Cache · Copy List with Random Pointer · Validate BST · Kth Smallest in BST · Binary Tree Level Order · Right Side View · Diameter · Max Path Sum · Serialize/Deserialize |
| **5** | Trees III: LCA/ancestor/Morris | Tries | Heaps / Top-K | LCA of BT · LCA of BST · Vertical Order · Flatten BT to LL · Morris Inorder · Implement Trie · Word Search II · Design Add & Search Words · Kth Largest in Array · Top K Frequent · Find Median from Data Stream · Task Scheduler |
| **6** | Graphs: BFS/DFS | Topological Sort | Union-Find | Number of Islands · Rotting Oranges · Word Ladder · Clone Graph · Pacific Atlantic · Course Schedule I & II · Alien Dictionary · Number of Connected Components · Redundant Connection · Accounts Merge |

**Gate G1 (end Wk 6):** 80% cold-solve on Mediums ≤25 min. 60 trigger cards. Queue documented.

### PHASE 2 — Weeks 7–12: Depth & Speed (Sep 14 – Oct 25)

| Wk | Mon | Tue | Thu | Must-do problems |
|----|-----|-----|-----|------------------|
| **7** | Dijkstra / Bellman-Ford | MST (Kruskal/Prim) | Advanced graphs (bridges, SCC, bipartite) | Network Delay Time · Cheapest Flights K Stops · Path with Max Probability · Swim in Rising Water · Min Cost to Connect Points · Critical Connections · Is Graph Bipartite · Reconstruct Itinerary |
| **8** | DP 1-D | Knapsack (0/1 + unbounded) | DP on subsequences | House Robber I/II · Decode Ways · Word Break · LIS (O(n log n)) · Partition Equal Subset Sum · Target Sum · Coin Change I/II · Perfect Squares · Combination Sum IV |
| **9** | DP 2-D grid | DP on strings | DP on trees | Unique Paths I/II · Minimum Path Sum · Dungeon Game · LCS · Edit Distance · Distinct Subsequences · Regular Expression Matching · Wildcard Matching · House Robber III · Binary Tree Cameras |
| **10** | DP intervals / MCM | DP bitmask | DP state machine (stocks) | Burst Balloons · Matrix Chain · Palindrome Partitioning II · Stone Game · Shortest Path Visiting All Nodes · Partition to K Equal Subsets · Best Time to Buy/Sell Stock II/III/IV/cooldown/fee |
| **11** | Heaps advanced + Greedy | Bit Manipulation | Math / Number Theory | Merge k Lists (heap) · Smallest Range Covering K Lists · IPO · Jump Game II · Gas Station · Partition Labels · Single Number I/II/III · Subsets via bitmask · Sum of Two Integers · Pow(x,n) · Sieve · GCD problems |
| **12** | Backtracking | Strings advanced (KMP/Rabin-Karp) | Design + Advanced (Segment Tree / Fenwick) | N-Queens · Sudoku Solver · Word Search · Palindrome Partitioning · Restore IP · Implement strStr (KMP) · Repeated Substring Pattern · Longest Duplicate Substring · Range Sum Query Mutable · Count of Smaller Numbers After Self · Design Twitter · Design Search Autocomplete |

**Gate G2 (end Wk 12):** 90% Medium, 60% Hard, LC rating ≥1750, queue < 25.

### PHASE 3 — Weeks 13–18: Company-Tagged (Oct 26 – Dec 6)

Switch entirely to **company-tagged lists, last 6 months**, sorted by frequency.
(LeetCode Premium is worth ₹2,500 for these 3 months — buy it in Week 12. Non-negotiable ROI.)

| Wk | Company focus | Notes |
|----|---------------|-------|
| 13 | **Amazon** | Heavy on: graphs/BFS, heaps, intervals, trees, string parsing. Also LP-behavioral pairing. |
| 14 | **Google** | Heavy on: DP, tricky greedy, math, "there is a follow-up" style. Practice discussing before coding. |
| 15 | **Meta** | Heavy on: arrays/strings, BFS/DFS, medium-fast (2 problems in 35 min). **Speed is the signal.** |
| 16 | **Uber / Microsoft / Atlassian** | Uber = LLD-flavoured DSA. MSFT = trees/linked lists/DP. Atlassian = practical coding. |
| 17 | **Weak-pattern blitz** | Take your 5 worst patterns from trackers → 4 problems/day on those only |
| 18 | **Weak-pattern blitz + full mocks** | 2 full 60-min DSA mocks + upsolve |

### PHASE 4 — Weeks 19–22: Maintenance (Dec 7 – Jan 3)
- **60 min/day only.** 2 problems: 1 random Medium + 1 Failure-Queue redo.
- Day before any interview: **zero new problems.** Only re-read Trigger Cards + 1 easy warm-up.
- Protect sleep above all else during loop weeks.

---

## 6. CONTEST PLAN (non-negotiable — this is your pressure gym)

> **Full strategy, including the "daily virtual contest?" answer, upsolve protocol and red flags:
> `09_Contest_Strategy.md`. Summary below.**

Your rating is 1490 after only 3 contests. That's a **sample-size problem**, not a ceiling.

| Platform | When | Cadence |
|----------|------|---------|
| **Daily micro-contest** | Weekday mornings | 45 min, 2 unseen problems — see §2 |
| **LeetCode Weekly** | Sunday 08:00 IST | Every week (Wk 1–18) |
| **LeetCode Biweekly** | Alternate Saturdays 20:00 IST | Every occurrence |
| **Virtual contest (full 90 min)** | Wednesday evening | **1/week from Week 7 only** — and only if last upsolve is done |
| **Codeforces Div 2 / Div 3** | As scheduled | 1× per fortnight from Week 5 |

### Contest protocol
1. **Rating is irrelevant.** The purpose is: perform the Stuck Protocol under a real clock.
2. **Upsolve is mandatory and worth 3× the contest.** Same day, solve every unsolved problem.
   Blank file, no editorial until you've spent 30 min.
3. Log in trackers: `problems solved / attempted`, `which one broke you`, `why`.
4. Rating trajectory expectation: 1490 (Wk 0) → 1600 (Wk 6) → 1750 (Wk 12) → 1900 (Wk 18).
   **21 contests is the goal, not 1900.** The rating follows attendance.

---

## 7. VOLUME TARGETS (guardrails, not goals)

| Phase | New problems/week | Redos/week | Total/week |
|-------|-------------------|------------|------------|
| Wk 1–6 | 12 | 8 | ~20 |
| Wk 7–12 | 14 | 8 | ~22 |
| Wk 13–18 | 12 (company-tagged) | 6 | ~18 |
| Wk 19–22 | 6 | 6 | ~12 |
| **Total over 22 weeks** | ~250 new | ~150 redos | **~400** |

You'll end at ~900 lifetime problems — but the number that matters is **92% cold-solve**.

> ⚠️ **Anti-goal:** If you ever find yourself choosing "more problems" over "fix the queue",
> you are repeating the mistake that produced 650 problems at 60% recall.

---

## 8. THE INTERVIEW-DAY DSA SCRIPT (memorise this)

```
"Let me make sure I understand — [restate]. So for input X I'd expect Y. Is that right?"
"What are the constraints on n? Can values be negative? Duplicates? Empty input?"
"The brute force would be [approach], which is O(...) time, O(...) space."
"That's too slow because we recompute [X]. Let me think about what we can cache/precompute…"
"This looks like [pattern] because [signal]."
"Here's the plan: [3 bullets]. Shall I code it?"
   → code, narrating variable purposes
"Let me dry-run on the example… [trace]"
"Edge cases: empty, single element, all same, overflow."
"Time O(...), space O(...). A follow-up optimisation could be [X]."
```
Practicing this script is worth more than 30 extra problems. Run it on **every** problem,
including ones you can solve instantly.




