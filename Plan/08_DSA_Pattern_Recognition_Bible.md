# 08 — DSA PATTERN RECOGNITION BIBLE
### The "zero knowledge-gap" file — if you miss a problem after this, it's practice, not knowledge

> **Companion to** `../DSA_Patterns_MasterSheet.md`.
> That file tells you **which patterns exist + which problems use them.**
> **This file tells you how to RECOGNISE one in 60 seconds and TYPE it from memory.**
>
> Three layers of pattern knowledge — most people only have Layer 1, which is why they fail:
> | Layer | What it is | Symptom if missing |
> |---|---|---|
> | **1. Recognition** | statement → pattern name | "I had no idea where to start" |
> | **2. Template** | pattern → code you can type blind | "I knew it was sliding window but botched the shrink" |
> | **3. Adaptation** | template → this problem's twist | "My code was 90% right but WA on edge cases" |
>
> **Everything below is Layers 2 and 3.** Read once fully (~2 hrs). Then use §2, §3 and §14 daily.

---

# PART 0 — THE 60-SECOND DECISION PROCEDURE

Run this **out loud, in order, on every problem**. It is designed so that by step 4 you have
named the pattern in >90% of interview problems.

```
STEP 1 — READ THE CONSTRAINTS FIRST (before the story). §1 maps n → allowed complexity.
         n ≤ 20?  → bitmask / backtracking.  n = 1e5? → O(n log n) max. This ALONE eliminates 80%.

STEP 2 — CLASSIFY THE OUTPUT. What am I returning?
         a boolean/count/max-min number → DP, greedy, binary-search-on-answer
         all/one valid configuration      → backtracking
         an index / pair / subarray       → two pointers, sliding window, prefix+hash
         an ordering                      → sort, topological sort
         a structure (tree/graph/list)    → traversal / construction
         a data structure with operations → design (HashMap+DLL, heap, trie, BIT)

STEP 3 — CLASSIFY THE INPUT. §2 maps input shape → 2–3 candidate patterns.
         sorted? · linked list? · grid? · graph/edges? · string? · intervals? · stream?

STEP 4 — MATCH THE KEYWORD. §3 is the 120-line trigger dictionary. Scan for the phrase.

STEP 5 — STATE THE BRUTE FORCE + ITS COMPLEXITY OUT LOUD. Then ask the ONE question that
         converts brute force → optimal:
         "What am I recomputing?"        → memoise / prefix-sum / DP
         "What am I re-scanning?"        → two pointers / sliding window / monotonic stack
         "What am I re-sorting?"         → heap / TreeMap / bucket
         "Can I search the answer space?"→ binary search on answer
         "Is the local choice provably safe?" → greedy
```

> **If steps 1–5 don't produce a pattern in 3 minutes, it's an "invent it" problem
> (usually greedy + a clever observation). Say so out loud and start reasoning from small cases:
> n=1, n=2, n=3. Brute-force the tiny cases by hand and look for the invariant.**

---

# PART 1 — CONSTRAINTS → COMPLEXITY → ALGORITHM (the single highest-value table in this file)

Assume ~10⁸ simple operations per second in an interview/contest.

| n (input size) | Max allowed complexity | Almost always one of |
|----------------|------------------------|----------------------|
| n ≤ 10 | O(n!) , O(n⁶) | Permutations, brute-force backtracking |
| n ≤ 20 | O(2ⁿ) , O(2ⁿ·n) | **Bitmask DP**, subset enumeration, meet-in-the-middle |
| n ≤ 40 | O(2^(n/2)) | **Meet in the middle** (split into halves) |
| n ≤ 100 | O(n⁴) , O(n³) | **Floyd–Warshall**, interval DP, MCM, 3-nested-loop DP |
| n ≤ 500 | O(n³) | Interval DP, matrix DP, Bellman-Ford |
| n ≤ 2,000 | O(n²) , O(n² log n) | 2-D DP, LCS/edit distance, all-pairs comparisons |
| n ≤ 10⁴ | O(n²) borderline / O(n√n) | DP, sqrt decomposition |
| n ≤ 10⁵ | **O(n log n)** | **Sort, heap, binary search, segment tree/BIT, Dijkstra, DSU, sliding window** |
| n ≤ 10⁶ | **O(n)** , O(n log n) tight | Two pointers, prefix sums, counting sort, single-pass hashing |
| n ≤ 10⁸ | O(n) with tiny constant, or O(log n) | Math, sieve-free number theory |
| n ≤ 10⁹ / 10¹⁸ | **O(log n)** or **O(1)** | **Binary search on answer**, math/formula, **fast power**, **matrix exponentiation**, digit DP |

### Reverse lookup — "what does this constraint hint at?"
| Constraint clue | Strong hint |
|---|---|
| `1 ≤ n ≤ 10⁵` and answer is a **number you could guess and verify** | Binary search on answer |
| `k ≤ 20` while `n ≤ 10⁵` | Bitmask over k, or heap of size k |
| Values bounded (`a[i] ≤ 100`) but n huge | Counting/bucket, frequency array, or DP over value range |
| `sum of n over all test cases ≤ 2·10⁵` | Per-test O(n log n) is fine (Codeforces idiom) |
| Answer modulo 1e9+7 | **Counting DP** or combinatorics (never a greedy/optimisation answer) |
| "return the k-th …" | Heap, quickselect, or binary search on the answer |
| Coordinates up to 1e9 but only 1e5 points | **Coordinate compression** |
| Many queries (`q ≤ 1e5`) on a static array | Prefix sums / sparse table |
| Many queries with updates | **Fenwick / Segment tree** |

> **Say the constraint reasoning out loud in the interview.** "n is 1e5, so I need at most
> O(n log n) — that rules out the O(n²) DP and points me at sorting or a heap." This single
> sentence reads as *senior* to every interviewer.

---

# PART 2 — INPUT SHAPE → CANDIDATE PATTERNS (fast narrowing)

| If the input is… | First 3 things to try (in this order) |
|---|---|
| **Sorted array** | Two pointers → binary search → sliding window |
| **Unsorted array, need pairs/sums** | HashMap → sort then two pointers → prefix sum + hash |
| **Array + "contiguous subarray"** | Sliding window → prefix sum (+ hashmap) → Kadane → monotonic deque |
| **Array + "subsequence" (non-contiguous)** | DP (LIS / knapsack / LCS) → greedy + heap |
| **Array + "next/previous greater/smaller"** | **Monotonic stack** (always) |
| **Array of intervals** | Sort by start → sweep line / difference array → heap on end |
| **Array with values in [1..n]** | **Cyclic sort** or index-as-hash (negation marking) |
| **Linked list** | Fast/slow pointers → dummy head → reverse in place |
| **Binary tree** | DFS postorder returning info → BFS level order → inorder (if BST) |
| **N-ary tree / general tree** | DFS with children loop → tree DP |
| **Grid / matrix** | BFS (shortest) / DFS (connectivity) → DP (paths) → treat cells as graph nodes |
| **Graph, unweighted** | BFS → DFS → topological sort → union-find |
| **Graph, weighted non-negative** | Dijkstra → MST (Kruskal/Prim) |
| **Graph, negative weights** | Bellman-Ford → Floyd-Warshall (small n) |
| **Graph, "dependencies/order"** | **Topological sort** (+ DP on DAG) |
| **Graph, "groups / merge / connected"** | **Union-Find** |
| **String, pattern/prefix** | Trie → KMP / rolling hash |
| **String, window/anagram** | Sliding window + frequency array |
| **String, palindrome** | Expand-around-centre → DP → Manacher (rare) |
| **String, "transform A into B"** | Edit-distance style 2-D DP |
| **Stream / online data** | Heap (two heaps) → reservoir sampling → design DS |
| **"Design a class with O(1) ops"** | HashMap + (DLL / array) combo |
| **Numbers, huge exponent/modulo** | Fast power, modular inverse, matrix exponentiation |
| **"How many ways"** | Counting DP / combinatorics (nCr, Catalan) |
| **"Minimum/maximum such that possible"** | **Binary search on answer** + feasibility check |
| **"Optimal play / two players"** | Game DP / minimax / Grundy numbers |
| **Choose items with a capacity** | Knapsack DP |
| **"At most k operations"** | DP with k as a dimension, or sliding window with k |

---

# PART 3 — THE TRIGGER DICTIONARY (statement phrase → pattern)

**This is the file's core. Turn each row into an Anki card (`DSA-Triggers` deck).**
Target: name the pattern in <30 seconds for 90% of Mediums.

### Arrays / Sequences
| Phrase in the problem | Pattern |
|---|---|
| "two numbers that sum to target", sorted | Two pointers (converging) |
| "two numbers that sum to target", unsorted | HashMap complement |
| "three/four numbers sum to" | Sort + fix one + two pointers |
| "contiguous subarray with sum = k" | Prefix sum + HashMap |
| "contiguous subarray with sum ≥ k", positives | Sliding window (shrinking) |
| "contiguous subarray, max sum" | **Kadane** |
| "longest substring/subarray with ≤ k distinct" | Variable sliding window + count map |
| "exactly k distinct" | `atMost(k) − atMost(k−1)` |
| "subarray of size k" | Fixed sliding window |
| "max/min in every window of size k" | **Monotonic deque** |
| "next greater / next smaller / span" | **Monotonic stack** |
| "largest rectangle / trapped water / histogram" | Monotonic stack (or two pointers) |
| "product of all except self" | Prefix + suffix products |
| "range sum queries, no updates" | Prefix sum |
| "range sum queries, with updates" | **Fenwick (BIT) / segment tree** |
| "add v to range [l,r] many times, then read" | **Difference array** |
| "numbers 1..n, one missing/duplicate" | **Cyclic sort** or XOR or index-negation |
| "move/remove elements in-place, O(1) space" | Two pointers (read/write) |
| "sort colours / 3 groups" | **Dutch national flag** |
| "k-th largest / smallest" | Heap of size k, or **quickselect** |
| "top k frequent" | HashMap + heap / bucket sort |
| "merge k sorted lists/arrays" | Min-heap of k heads |
| "median of a stream" | **Two heaps** |
| "rotate array / string by k" | Reverse 3 times |
| "find peak element" | Binary search on slope |
| "rotated sorted array" | Modified binary search (find the sorted half) |
| "minimise the maximum …" / "maximise the minimum …" | **Binary search on answer** |
| "can we do it in ≤ X time/capacity?" | Binary search on answer + greedy feasibility |
| "count inversions" | Merge sort / BIT |
| "coordinates up to 1e9, only 1e5 points" | Coordinate compression + sweep |

### Intervals
| Phrase | Pattern |
|---|---|
| "merge overlapping intervals" | Sort by start, merge |
| "insert an interval" | Linear scan in 3 phases |
| "minimum meeting rooms / max concurrent" | Sort + min-heap on end, or **sweep line** |
| "maximum non-overlapping intervals" | Greedy: sort by **end**, take earliest finishing |
| "minimum arrows / remove to make non-overlapping" | Same greedy (sort by end) |
| "employee free time / interval intersection" | Two pointers on two sorted lists |
| "car pooling / booking calendar" | Difference array / sweep line / TreeMap |

### Linked List
| Phrase | Pattern |
|---|---|
| "detect cycle / find cycle start" | **Floyd's fast-slow** |
| "middle node" | Fast-slow |
| "n-th from end" | Two pointers with a gap of n |
| "reverse / reverse in groups of k" | Iterative reverse with prev/cur/next |
| "merge two/k sorted lists" | Dummy head (+ heap for k) |
| "reorder / palindrome linked list" | Find middle → reverse second half → merge |
| "copy list with random pointer" | Interleave nodes, or HashMap old→new |
| "operations must be O(1)" | HashMap + doubly linked list |

### Trees
| Phrase | Pattern |
|---|---|
| "level by level / right side view / zigzag" | **BFS level order** (size-loop) |
| "depth / height / balanced / diameter" | **Postorder DFS returning info** |
| "path sum from root to leaf" | Preorder DFS with running state + backtrack |
| "max path sum any-node-to-any-node" | Postorder: return downward best, update global with l+r+node |
| "BST" + "kth smallest / validate / sorted" | **Inorder traversal** |
| "lowest common ancestor" | Recursive LCA (or parent map + set, or binary lifting) |
| "serialize / deserialize / duplicate subtrees" | Preorder with null markers + HashMap |
| "construct tree from preorder+inorder" | Recursion + index HashMap |
| "O(1) space traversal" | **Morris traversal** |
| "count/choose nodes with parent-child constraint" | **Tree DP** (rob-house-III style: return {take, skip}) |
| "answer for every node as root" | **Rerooting technique** |
| "distance k from a node" | Convert to graph (parent pointers) + BFS |
| "prefix / dictionary / autocomplete / word search II" | **Trie** |

### Graphs
| Phrase | Pattern |
|---|---|
| "shortest path, unweighted" | **BFS** |
| "shortest path, weights ≥ 0" | **Dijkstra** |
| "shortest path, weights can be negative" | Bellman-Ford (or SPFA) |
| "shortest path between all pairs, n ≤ 400" | **Floyd–Warshall** |
| "edges weighted 0 or 1" | **0-1 BFS** (deque) |
| "minimum spanning / connect all with min cost" | **Kruskal (DSU) / Prim** |
| "prerequisites / build order / dependencies" | **Topological sort** |
| "detect cycle, directed" | DFS 3-colour, or Kahn's (order size < n) |
| "detect cycle, undirected" | DSU, or DFS with parent |
| "number of connected components / islands" | DFS/BFS flood fill, or **DSU** |
| "merge accounts / friend circles / equations" | **Union-Find** |
| "two groups / can we 2-colour" | **Bipartite check** (BFS colouring) |
| "spread from multiple starting points simultaneously" | **Multi-source BFS** |
| "critical connections / cut edges" | **Tarjan bridges** |
| "strongly connected components" | Kosaraju / Tarjan SCC |
| "use every edge exactly once / itinerary" | **Eulerian path (Hierholzer)** |
| "longest path in a DAG" | Topological order + DP |
| "word ladder / min transformations" | BFS on implicit graph (**bidirectional BFS** to optimise) |
| "grid + shortest with obstacles/keys" | BFS with state = (cell, bitmask of keys) |
| "maximum matching / assignment" | Bipartite matching / max flow (rare) |

### DP
| Phrase | Pattern |
|---|---|
| "how many ways to …" | Counting DP |
| "min/max cost to reach the end" | DP over positions |
| "climb stairs / can't pick adjacent" | 1-D linear DP (House Robber) |
| "pick items, capacity limit, each once" | **0/1 knapsack** (reverse inner loop) |
| "unlimited copies of each item" | **Unbounded knapsack** (forward inner loop) |
| "min coins to make amount" | Unbounded knapsack (min) |
| "number of combinations to make amount" | Unbounded knapsack (count) — **loop coins outer** |
| "number of permutations to make amount" | Loop **amount outer**, coins inner |
| "split array into two equal-sum halves" | Subset-sum DP |
| "longest increasing subsequence" | Patience/binary-search DP O(n log n) |
| "longest common subsequence / edit distance" | 2-D string DP |
| "count distinct subsequences equal to t" | 2-D string DP |
| "regex / wildcard matching" | 2-D DP with the `*` case split |
| "palindrome partitioning (min cuts)" | Precompute isPal + 1-D DP |
| "burst balloons / matrix chain / merge stones" | **Interval DP** (loop by length) |
| "TSP / visit all nodes / assign n to n" | **Bitmask DP** |
| "count numbers ≤ N with a digit property" | **Digit DP** |
| "buy/sell stock with k transactions/cooldown/fee" | **State machine DP** |
| "two players play optimally" | **Game DP / minimax** (or Grundy for combinatorial games) |
| "expected value / probability after k steps" | Probability DP |
| "DP but n = 1e18 with linear recurrence" | **Matrix exponentiation** |
| "DP transition = max over a sliding range" | DP + **monotonic deque** optimisation |

### Greedy
| Phrase | Pattern |
|---|---|
| "maximum number of activities/intervals" | Sort by end time |
| "minimum number of platforms/rooms/arrows" | Sort + heap / sweep |
| "can you reach the end" (jump game) | Track furthest reachable |
| "minimum jumps" | Greedy BFS-levels on ranges |
| "gas station circular tour" | Running deficit reset |
| "task scheduler with cooldown" | Frequency math / max-heap |
| "reorganise string / no two adjacent same" | Max-heap on frequency |
| "assign cookies / boats to save people" | Sort both + two pointers |
| "partition labels" | Last-occurrence map + sweep |
| "minimum cost to connect ropes/sticks" | Min-heap (Huffman) |
| "buy low sell high, unlimited transactions" | Sum of positive deltas |

### Strings
| Phrase | Pattern |
|---|---|
| "anagram / permutation of" | Frequency array (26) + sliding window |
| "longest palindromic substring" | Expand around centre (2n−1 centres) |
| "count palindromic substrings" | Expand around centre |
| "find pattern occurrences in text" | **KMP** / rolling hash |
| "longest happy/duplicate substring" | Binary search on length + **rolling hash** |
| "group anagrams" | Sorted-string or count-signature as HashMap key |
| "word break" | DP + set (or Trie) |
| "shortest palindrome / prefix=suffix" | **KMP failure function** |
| "encode/decode strings" | Length-prefixed protocol |
| "valid parentheses / expression" | **Stack** |
| "calculator with +−×÷ and brackets" | Stack (+ recursion for brackets) |

### Design & Misc
| Phrase | Pattern |
|---|---|
| "O(1) get and put with eviction" | **HashMap + doubly linked list** (LRU) |
| "O(1) with frequency eviction" | HashMap + freq→DLL map (LFU) |
| "insert/delete/getRandom in O(1)" | ArrayList + HashMap(value→index), swap-with-last |
| "time-based key-value store" | HashMap<key, List<(ts,val)>> + binary search |
| "design Twitter feed" | HashMap + heap merge of k lists |
| "iterator / peeking / flatten nested" | Stack-based lazy iterator |
| "rate limiter" | Deque of timestamps / token bucket |
| "random pick with weights" | Prefix sums + binary search |
| "random node from a stream" | **Reservoir sampling** |
| "shuffle an array" | Fisher–Yates |
| "single number / find the odd one" | **XOR** |
| "count set bits for 0..n" | `dp[i] = dp[i>>1] + (i&1)` |
| "maximum XOR pair" | **Bitwise trie** |
| "n is huge, need a^b mod m" | **Fast power** |
| "count primes ≤ n" | **Sieve of Eratosthenes** |

---

# PART 4 — THE TEMPLATES (Layer 2 — type these blind until automatic)

> **Drill:** every Sunday, pick 5 templates at random and type them from memory into a blank file.
> If you can't, that template is next week's priority. **Target: all 40 typed cold by Week 8.**

## 4.1 Binary search — the ONE template that never has off-by-one bugs
```text
// LOWER BOUND: smallest index i with a[i] >= target  (returns n if none)
int lowerBound(int[] a, int target) {
    int lo = 0, hi = a.length;              // hi is EXCLUSIVE
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;       // never lo+hi (overflow)
        if (a[mid] >= target) hi = mid;     // keep mid as a candidate
        else lo = mid + 1;                  // mid is definitely not the answer
    }
    return lo;                              // lo == hi == answer
}
// UPPER BOUND: change the condition to  a[mid] > target
```
**Gotcha:** never write `hi = mid - 1` with an exclusive `hi`. Pick one style and *never* deviate.

## 4.2 Binary search on the ANSWER (the highest-yield pattern in interviews)
```text
// Find the SMALLEST x in [lo, hi] such that feasible(x) is true.
// Requires monotonicity: feasible is F,F,F,...,T,T,T
int lo = MIN_ANS, hi = MAX_ANS;
while (lo < hi) {
    int mid = lo + (hi - lo) / 2;
    if (feasible(mid)) hi = mid;
    else lo = mid + 1;
}
return lo;

// For the LARGEST x with feasible(x): use  mid = lo + (hi - lo + 1) / 2;
//   if (feasible(mid)) lo = mid; else hi = mid - 1;   ← note the +1 to avoid infinite loop
```
**Recognise it:** "minimum largest…", "maximum smallest…", "min days/capacity/speed such that…".
**The whole trick:** write `feasible(x)` as a simple greedy O(n) check.

## 4.3 Sliding window — LONGEST valid window
```text
int left = 0, best = 0;
Map<Character, Integer> cnt = new HashMap<>();
for (int right = 0; right < s.length(); right++) {
    cnt.merge(s.charAt(right), 1, Integer::sum);          // 1. expand
    while (/* window is INVALID */ cnt.size() > k) {      // 2. shrink until valid
        char c = s.charAt(left++);
        if (cnt.merge(c, -1, Integer::sum) == 0) cnt.remove(c);
    }
    best = Math.max(best, right - left + 1);              // 3. record (window is valid here)
}
```

## 4.4 Sliding window — SHORTEST valid window
```text
int left = 0, best = Integer.MAX_VALUE, sum = 0;
for (int right = 0; right < n; right++) {
    sum += a[right];                                     // 1. expand
    while (sum >= target) {                              // 2. while VALID, record + shrink
        best = Math.min(best, right - left + 1);
        sum -= a[left++];
    }
}
return best == Integer.MAX_VALUE ? 0 : best;
```
> **The only difference between 4.3 and 4.4:** longest records *outside* the while, shortest
> records *inside*. Burn this into memory — it's the #1 sliding-window bug.

## 4.5 "Exactly K" trick
```text
int exactlyK(int[] a, int k) { return atMost(a, k) - atMost(a, k - 1); }
// atMost() is just template 4.3 counting subarrays: res += right - left + 1;
```

## 4.6 Monotonic stack (next greater element)
```text
int[] res = new int[n];
Arrays.fill(res, -1);
Deque<Integer> st = new ArrayDeque<>();     // stores INDICES; values are decreasing
for (int i = 0; i < n; i++) {
    while (!st.isEmpty() && a[st.peek()] < a[i]) res[st.pop()] = a[i];
    st.push(i);
}
```
**Variants:** next smaller → flip `<` to `>`. Previous greater → iterate right-to-left.
**Histogram/rectangle:** push indices, on pop compute `width = i - st.peek() - 1`, use sentinels.

## 4.7 Monotonic deque (max of every window of size k)
```text
Deque<Integer> dq = new ArrayDeque<>();     // indices, values decreasing
int[] res = new int[n - k + 1];
for (int i = 0; i < n; i++) {
    while (!dq.isEmpty() && dq.peekFirst() <= i - k) dq.pollFirst();   // drop out-of-window
    while (!dq.isEmpty() && a[dq.peekLast()] <= a[i]) dq.pollLast();   // drop useless
    dq.offerLast(i);
    if (i >= k - 1) res[i - k + 1] = a[dq.peekFirst()];
}
```

## 4.8 Prefix sum + HashMap (subarray sum equals k)
```text
Map<Integer, Integer> seen = new HashMap<>();
seen.put(0, 1);                        // ← the empty prefix. Forgetting this is THE classic bug
int sum = 0, count = 0;
for (int x : a) {
    sum += x;
    count += seen.getOrDefault(sum - k, 0);
    seen.merge(sum, 1, Integer::sum);
}
```
**Variants:** divisible by k → key = `((sum % k) + k) % k`. Equal 0s and 1s → map 0 to −1.

## 4.9 Difference array (range updates, then read once)
```text
int[] diff = new int[n + 1];
for (int[] q : updates) { diff[q[0]] += q[2]; diff[q[1] + 1] -= q[2]; }
int run = 0;
for (int i = 0; i < n; i++) { run += diff[i]; a[i] = run; }
```

## 4.10 Union-Find (DSU) — path compression + union by rank
```text
class DSU {
    int[] parent, rank; int components;
    DSU(int n) {
        parent = new int[n]; rank = new int[n]; components = n;
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    int find(int x) {                                   // iterative + path halving
        while (parent[x] != x) { parent[x] = parent[parent[x]]; x = parent[x]; }
        return x;
    }
    boolean union(int a, int b) {
        int ra = find(a), rb = find(b);
        if (ra == rb) return false;                     // already connected → cycle
        if (rank[ra] < rank[rb]) { int t = ra; ra = rb; rb = t; }
        parent[rb] = ra;
        if (rank[ra] == rank[rb]) rank[ra]++;
        components--;
        return true;
    }
}
```

## 4.11 BFS on a grid (shortest path / multi-source)
```text
int[][] DIRS = {{1,0},{-1,0},{0,1},{0,-1}};
Deque<int[]> q = new ArrayDeque<>();
boolean[][] vis = new boolean[m][n];
// MULTI-SOURCE: push ALL sources before the loop starts
q.add(new int[]{sr, sc}); vis[sr][sc] = true;
int steps = 0;
while (!q.isEmpty()) {
    for (int sz = q.size(); sz > 0; sz--) {      // ← level-by-level = distance
        int[] cur = q.poll();
        for (int[] d : DIRS) {
            int r = cur[0] + d[0], c = cur[1] + d[1];
            if (r < 0 || r >= m || c < 0 || c >= n || vis[r][c] || grid[r][c] == WALL) continue;
            vis[r][c] = true;
            q.add(new int[]{r, c});
        }
    }
    steps++;
}
```
**Key rule:** mark visited **when enqueuing**, not when dequeuing (otherwise duplicates blow up).

## 4.12 Topological sort (Kahn's) + cycle detection
```text
int[] indeg = new int[n];
for (int u = 0; u < n; u++) for (int v : adj.get(u)) indeg[v]++;
Deque<Integer> q = new ArrayDeque<>();
for (int i = 0; i < n; i++) if (indeg[i] == 0) q.add(i);
List<Integer> order = new ArrayList<>();
while (!q.isEmpty()) {
    int u = q.poll(); order.add(u);
    for (int v : adj.get(u)) if (--indeg[v] == 0) q.add(v);
}
if (order.size() != n) { /* CYCLE exists */ }
```

## 4.13 Dijkstra
```text
long[] dist = new long[n];
Arrays.fill(dist, Long.MAX_VALUE);
dist[src] = 0;
PriorityQueue<long[]> pq = new PriorityQueue<>((x, y) -> Long.compare(x[1], y[1]));
pq.add(new long[]{src, 0});
while (!pq.isEmpty()) {
    long[] cur = pq.poll();
    int u = (int) cur[0];
    if (cur[1] > dist[u]) continue;               // ← stale entry (lazy deletion). Don't skip this line
    for (int[] e : adj.get(u)) {                  // e = {to, weight}
        if (dist[u] + e[1] < dist[e[0]]) {
            dist[e[0]] = dist[u] + e[1];
            pq.add(new long[]{e[0], dist[e[0]]});
        }
    }
}
```
**Variant — state-augmented Dijkstra:** when there's an extra resource (k stops, fuel, keys),
make dist a 2-D array `dist[node][k]`. This is a very common Hard.

## 4.14 0-1 BFS (weights only 0 or 1)
```text
Deque<Integer> dq = new ArrayDeque<>();
dq.addFirst(src);
// weight 0 edge → addFirst ; weight 1 edge → addLast.  O(V+E), no heap needed.
```

## 4.15 Backtracking — the universal skeleton
```text
List<List<Integer>> res = new ArrayList<>();
void backtrack(int start, List<Integer> path) {
    if (/* goal */) { res.add(new ArrayList<>(path)); return; }   // ← COPY the list
    for (int i = start; i < n; i++) {
        if (i > start && a[i] == a[i - 1]) continue;   // skip duplicates (array must be SORTED)
        if (/* invalid choice */) continue;            // pruning
        path.add(a[i]);
        backtrack(i + 1, path);   // i + 1 → each element once   |   i → element reusable
        path.remove(path.size() - 1);                  // undo
    }
}
```
| Problem type | `start` handling |
|---|---|
| Subsets / combinations | pass `i + 1` |
| Combination sum (reuse allowed) | pass `i` |
| Permutations | loop from 0 with a `used[]` boolean array |
| Permutations with duplicates | sort + `if (i>0 && a[i]==a[i-1] && !used[i-1]) continue;` |

## 4.16 Kadane (max subarray)
```text
int best = a[0], cur = a[0];
for (int i = 1; i < n; i++) { cur = Math.max(a[i], cur + a[i]); best = Math.max(best, cur); }
```
**Variant — max circular:** answer = `max(kadaneMax, totalSum − kadaneMin)`, guard all-negative.

## 4.17 0/1 knapsack (1-D, space optimised)
```text
int[] dp = new int[W + 1];
for (int i = 0; i < n; i++)
    for (int w = W; w >= wt[i]; w--)                 // ← REVERSE = each item once
        dp[w] = Math.max(dp[w], dp[w - wt[i]] + val[i]);
// Unbounded knapsack: make the inner loop FORWARD (w = wt[i] .. W)
```
**The single most-tested detail:** direction of the inner loop = 0/1 vs unbounded. Know why.

## 4.18 Coin change — combinations vs permutations
```text
// COUNT COMBINATIONS (order doesn't matter): coins OUTER
for (int c : coins) for (int a = c; a <= amount; a++) dp[a] += dp[a - c];

// COUNT PERMUTATIONS (order matters): amount OUTER
for (int a = 1; a <= amount; a++) for (int c : coins) if (a >= c) dp[a] += dp[a - c];
```

## 4.19 LIS in O(n log n)
```text
int[] tails = new int[n]; int len = 0;
for (int x : a) {
    int lo = 0, hi = len;
    while (lo < hi) { int mid = (lo + hi) >>> 1; if (tails[mid] < x) lo = mid + 1; else hi = mid; }
    tails[lo] = x;
    if (lo == len) len++;
}
// strictly increasing → tails[mid] < x     |     non-decreasing → tails[mid] <= x
```

## 4.20 Interval DP
```text
for (int len = 2; len <= n; len++)
    for (int i = 0; i + len - 1 < n; i++) {
        int j = i + len - 1;
        for (int k = i; k < j; k++)
            dp[i][j] = Math.min(dp[i][j], dp[i][k] + dp[k + 1][j] + cost(i, k, j));
    }
```
**Recognise it:** "merge/burst/split a sequence, cost depends on the segment". Loop by **length**.

## 4.21 Bitmask DP (TSP / assignment)
```text
int FULL = (1 << n) - 1;
int[][] dp = new int[1 << n][n];
for (int[] row : dp) Arrays.fill(row, INF);
dp[1][0] = 0;
for (int mask = 1; mask <= FULL; mask++)
    for (int u = 0; u < n; u++) {
        if ((mask & (1 << u)) == 0 || dp[mask][u] == INF) continue;
        for (int v = 0; v < n; v++) {
            if ((mask & (1 << v)) != 0) continue;
            dp[mask | (1 << v)][v] = Math.min(dp[mask | (1 << v)][v], dp[mask][u] + w[u][v]);
        }
    }
```
**Submask enumeration** (for set-partition DP): `for (int sub = mask; sub > 0; sub = (sub-1) & mask)`

## 4.22 State-machine DP (stocks with k transactions)
```text
int[] buy = new int[k + 1], sell = new int[k + 1];
Arrays.fill(buy, Integer.MIN_VALUE / 2);
for (int p : prices)
    for (int t = 1; t <= k; t++) {
        buy[t]  = Math.max(buy[t],  sell[t - 1] - p);
        sell[t] = Math.max(sell[t], buy[t] + p);
    }
return sell[k];
```

## 4.23 Tree DFS returning info (the "postorder aggregate" pattern)
```text
int best = Integer.MIN_VALUE;
int dfs(TreeNode node) {                 // returns the best DOWNWARD path through node
    if (node == null) return 0;
    int l = Math.max(0, dfs(node.left));   // clamp negatives to 0 = "don't take this branch"
    int r = Math.max(0, dfs(node.right));
    best = Math.max(best, l + r + node.val);   // answer THROUGH node (can't go up)
    return node.val + Math.max(l, r);          // what the parent can use
}
```
> **This one shape solves:** diameter, max path sum, longest univalue path, balanced check,
> house-robber-III, count good nodes, LCA-with-info. Memorise the "return one thing, update
> the global with another" split — it's the most reused idea in tree problems.

## 4.24 Trie
```text
class Trie {
    Trie[] next = new Trie[26];
    boolean end;
    void insert(String w) {
        Trie cur = this;
        for (char c : w.toCharArray()) {
            int i = c - 'a';
            if (cur.next[i] == null) cur.next[i] = new Trie();
            cur = cur.next[i];
        }
        cur.end = true;
    }
}
```

## 4.25 Fenwick tree (BIT) — point update, prefix query
```text
class BIT {
    int n; long[] t;
    BIT(int n) { this.n = n; t = new long[n + 1]; }
    void add(int i, long v) { for (++i; i <= n; i += i & -i) t[i] += v; }
    long sum(int i) { long s = 0; for (++i; i > 0; i -= i & -i) s += t[i]; return s; }
    long range(int l, int r) { return sum(r) - (l > 0 ? sum(l - 1) : 0); }
}
```

## 4.26 Segment tree (iterative, point update + range query)
```text
int n; int[] seg;
void build(int[] a) { n = a.length; seg = new int[2 * n];
    for (int i = 0; i < n; i++) seg[n + i] = a[i];
    for (int i = n - 1; i > 0; i--) seg[i] = Math.max(seg[2*i], seg[2*i+1]); }
void update(int i, int v) { for (seg[i += n] = v; i > 1; i >>= 1) seg[i>>1] = Math.max(seg[i], seg[i^1]); }
int query(int l, int r) {            // [l, r)
    int res = Integer.MIN_VALUE;
    for (l += n, r += n; l < r; l >>= 1, r >>= 1) {
        if ((l & 1) == 1) res = Math.max(res, seg[l++]);
        if ((r & 1) == 1) res = Math.max(res, seg[--r]);
    }
    return res;
}
```

## 4.27 Two heaps (running median)
```text
PriorityQueue<Integer> lo = new PriorityQueue<>(Comparator.reverseOrder()); // max-heap, lower half
PriorityQueue<Integer> hi = new PriorityQueue<>();                          // min-heap, upper half
void add(int x) {
    lo.add(x);
    hi.add(lo.poll());                       // balance value-wise
    if (hi.size() > lo.size()) lo.add(hi.poll());   // balance size-wise
}
double median() { return lo.size() > hi.size() ? lo.peek() : (lo.peek() + hi.peek()) / 2.0; }
```

## 4.28 Quickselect (k-th largest in O(n) average)
```text
int quickSelect(int[] a, int lo, int hi, int k) {   // k = index in sorted order
    int p = partition(a, lo, hi);
    if (p == k) return a[p];
    return p < k ? quickSelect(a, p + 1, hi, k) : quickSelect(a, lo, p - 1, k);
}
```
**Always shuffle first** (or pick a random pivot) or worst case is O(n²) on sorted input.

## 4.29 Cyclic sort (values are 1..n)
```text
for (int i = 0; i < n; i++)
    while (a[i] > 0 && a[i] <= n && a[a[i] - 1] != a[i]) {
        int t = a[a[i] - 1]; a[a[i] - 1] = a[i]; a[i] = t;
    }
// then the first index where a[i] != i+1 is the missing/duplicate answer
```

## 4.30 Fast power + modular arithmetic
```text
static final int MOD = 1_000_000_007;
long power(long b, long e, long m) {
    long r = 1; b %= m;
    while (e > 0) { if ((e & 1) == 1) r = r * b % m; b = b * b % m; e >>= 1; }
    return r;
}
long modInverse(long a) { return power(a, MOD - 2, MOD); }   // MOD must be prime (Fermat)
```

## 4.31 Sieve of Eratosthenes
```text
boolean[] comp = new boolean[n + 1];
for (int i = 2; (long) i * i <= n; i++)
    if (!comp[i]) for (int j = i * i; j <= n; j += i) comp[j] = true;
```

## 4.32 KMP failure function
```text
int[] lps(String p) {
    int[] f = new int[p.length()]; int k = 0;
    for (int i = 1; i < p.length(); i++) {
        while (k > 0 && p.charAt(i) != p.charAt(k)) k = f[k - 1];
        if (p.charAt(i) == p.charAt(k)) k++;
        f[i] = k;
    }
    return f;
}
```

## 4.33 Rolling hash (Rabin–Karp)
```text
long BASE = 131, MOD = (1L << 61) - 1;   // or use two different mods to avoid collisions
// h = (h * BASE + c) % MOD  ; to roll: h = (h - old * BASE^(len-1)) * BASE + newC
```

## 4.34 Merge intervals
```text
Arrays.sort(iv, (x, y) -> Integer.compare(x[0], y[0]));
List<int[]> out = new ArrayList<>();
for (int[] cur : iv) {
    if (out.isEmpty() || out.get(out.size() - 1)[1] < cur[0]) out.add(cur);
    else out.get(out.size() - 1)[1] = Math.max(out.get(out.size() - 1)[1], cur[1]);
}
```

## 4.35 Minimum meeting rooms / max concurrency
```text
Arrays.sort(iv, (a, b) -> a[0] - b[0]);
PriorityQueue<Integer> endTimes = new PriorityQueue<>();
for (int[] x : iv) {
    if (!endTimes.isEmpty() && endTimes.peek() <= x[0]) endTimes.poll();
    endTimes.add(x[1]);
}
return endTimes.size();
```
**Sweep-line alternative:** `TreeMap<time, delta>`, +1 at start, −1 at end, running max.

## 4.36 LRU cache (HashMap + doubly linked list)
```text
class LRU {
    class Node { int k, v; Node prev, next; }
    Map<Integer, Node> map = new HashMap<>();
    Node head = new Node(), tail = new Node();   // sentinels
    int cap;
    LRU(int c) { cap = c; head.next = tail; tail.prev = head; }
    void remove(Node x) { x.prev.next = x.next; x.next.prev = x.prev; }
    void addFront(Node x) { x.next = head.next; x.prev = head; head.next.prev = x; head.next = x; }
    // get: remove + addFront ; put: evict tail.prev when over capacity
}
```
**Interview shortcut:** if allowed, `LinkedHashMap` with `removeEldestEntry` — but **know the
manual version**, that's what they actually want.

## 4.37 Insert / Delete / GetRandom in O(1)
```text
List<Integer> list = new ArrayList<>();
Map<Integer, Integer> idx = new HashMap<>();
// remove: swap the target with the LAST element, update its index, then remove last
```

## 4.38 Sweep line with TreeMap (skyline / calendar / booking)
```text
TreeMap<Integer, Integer> delta = new TreeMap<>();
for (int[] e : events) { delta.merge(e[0], 1, Integer::sum); delta.merge(e[1], -1, Integer::sum); }
int running = 0, max = 0;
for (int d : delta.values()) { running += d; max = Math.max(max, running); }
```

## 4.39 Binary lifting (LCA in O(log n) per query)
```text
int LOG = 17;
int[][] up = new int[LOG][n];    // up[j][v] = 2^j-th ancestor of v
// up[0][v] = parent(v); up[j][v] = up[j-1][ up[j-1][v] ];
// lift the deeper node to equal depth, then lift both while up[j][a] != up[j][b]
```

## 4.40 Meet in the middle (n ≤ 40)
```text
// split into two halves, enumerate all 2^(n/2) subset sums of each,
// sort the second half, then binary search for each first-half value.  O(2^(n/2) * n)
```

---

# PART 5 — COVERAGE TIERS (how to reach 90% confidence, in priority order)

| Tier | Patterns | Cumulative interview coverage | When to master |
|------|----------|-------------------------------|----------------|
| **S — the vital 20** | Two pointers · Sliding window (both variants) · Prefix sum + hash · Binary search (bound) · **Binary search on answer** · Monotonic stack · Heap/top-K · Merge intervals · Fast-slow pointers · Tree DFS-postorder · Tree BFS level order · BST inorder · Graph BFS/DFS · Topological sort · Union-Find · Backtracking skeleton · 1-D DP · Knapsack · Grid DP · HashMap design (LRU) | **~70%** | **Weeks 1–6** |
| **A — the next 25** | Monotonic deque · atMost(k) trick · Dutch flag · Cyclic sort · Quickselect · Two heaps · Trie · Dijkstra · MST · Bipartite · Multi-source BFS · 0-1 BFS · LIS n log n · LCS/edit distance · State-machine DP · Interval DP · Greedy interval scheduling · Sweep line · Difference array · Kadane · XOR tricks · Bit counting · Fast power · Sieve · Expand-around-centre | **~90%** ← **your target** | **Weeks 7–12** |
| **B — the depth 25** | Bitmask DP · Digit DP · Tree DP / rerooting · Segment tree · Fenwick · Binary lifting · Tarjan bridges · SCC · Eulerian path · KMP · Rolling hash · Manacher · Bitwise trie · Coordinate compression · Meet in the middle · Game DP / Grundy · Probability DP · Matrix exponentiation · Combinatorics/Catalan · Reservoir sampling · Fisher–Yates · Monotonic-deque DP optimisation · Persistent-ish tricks · Bidirectional BFS · Line sweep + multiset | **~97%** | **Weeks 13–18** |
| **C — awareness only** | Lazy propagation · HLD · Mo's algorithm · Suffix array/automaton · Max flow / bipartite matching · Convex hull trick · Li Chao tree · FFT · Simplex | **~99%** | Know the *name* and *when it applies*. Never needed in an SDE2 loop. |

> **You are aiming for Tier S + Tier A = ~90%.** Tier B is what turns Hards from "impossible"
> into "solvable". Tier C is contest-only — **do not spend interview-prep time there.**

---

# PART 6 — THE 30 "ADAPTATION" TWISTS (Layer 3 — where 90%-correct answers die)

You know the pattern, you typed the template, and it's still wrong. It's almost always one of these:

| # | Twist | Fix |
|---|-------|-----|
| 1 | Sliding window with **negative numbers** | Window shrinking is no longer monotonic → use prefix sum + HashMap instead |
| 2 | Sliding window: longest vs shortest | Record outside vs inside the `while` (§4.3 vs §4.4) |
| 3 | Binary search on answer isn't monotonic | Verify: if `feasible(x)` then `feasible(x+1)`. If not, BS is invalid |
| 4 | Overflow | Use `long` for sums/products; `lo + (hi-lo)/2` for mid; `(int)` cast only at the end |
| 5 | Modulo with subtraction | `((a - b) % MOD + MOD) % MOD` |
| 6 | Dijkstra with an extra constraint (k stops, fuel) | Make dist 2-D: `dist[node][resource]` |
| 7 | Graph is implicit (words, states, board positions) | Build neighbours on the fly; hash the state as the visited key |
| 8 | Grid BFS but cells revisitable with different state | Visited key = `(cell, bitmask/steps%k)` not just cell |
| 9 | Cycle detection: directed vs undirected | Directed → 3-colour DFS or Kahn. Undirected → DSU or DFS-with-parent |
| 10 | Topological sort with lexicographic requirement | Replace the queue with a **PriorityQueue** |
| 11 | Backtracking with duplicates | **Sort first**, then `if (i > start && a[i]==a[i-1]) continue;` |
| 12 | Backtracking too slow | Add pruning: sort descending, early-exit on partial sum, memoise the state |
| 13 | DP: combinations vs permutations | Loop order (§4.18). Get this wrong and the count is wrong |
| 14 | DP: 0/1 vs unbounded | Inner loop direction (§4.17) |
| 15 | DP state is insufficient | Ask: "given only this state, can I compute the transition?" If no, add a dimension |
| 16 | DP with a "cooldown"/"at most k" | Add a dimension for the extra resource |
| 17 | Memory limit on 2-D DP | Roll to 2 rows, or 1 row with careful iteration direction |
| 18 | Tree recursion returns the wrong thing | Separate "what I return to my parent" from "the global answer I update" (§4.23) |
| 19 | Tree problem needs the parent | Build a parent map first (or pass parent down), then treat it as a graph |
| 20 | Monotonic stack boundary | Push sentinels (−1 at start, n at end) or handle the leftover stack after the loop |
| 21 | Heap needs removal of an arbitrary element | Use **lazy deletion** (skip stale entries on poll) or a TreeMap |
| 22 | "k-th" with duplicates | Decide whether rank counts duplicates; state your assumption out loud |
| 23 | Intervals: `[1,2]` and `[2,3]` — overlap? | **Ask the interviewer.** Half-open vs closed changes `<` to `<=` |
| 24 | Two pointers on an unsorted array | Sort first — but only if the order doesn't matter for the answer |
| 25 | Strings: unicode vs 26 lowercase | Ask. `int[26]` vs `HashMap<Character,Integer>` |
| 26 | Recursion depth (n = 1e5 chain) | Convert DFS to an explicit stack, or note the risk out loud |
| 27 | `HashMap` too slow in a tight loop | Use `int[]` when the key space is bounded |
| 28 | Answer needs the actual path, not just the length | Store a `parent[]`/`choice[]` array and reconstruct backwards |
| 29 | Multiple valid answers | Ask "any valid one, or a specific one (lexicographically smallest)?" |
| 30 | Off-by-one on `mid` in "find largest x" | Use `mid = lo + (hi - lo + 1) / 2` (§4.2) or you infinite-loop |

---

# PART 7 — SELF-TEST: 40 STATEMENTS → NAME THE PATTERN IN 30 SECONDS

Cover the right column. **Score ≥36/40 = your knowledge gap is closed.** Retake every 4 weeks.

| # | Problem statement (paraphrased) | Pattern |
|---|---|---|
| 1 | Longest substring with at most 2 distinct chars | Variable sliding window + count map |
| 2 | Min number of days to ship all packages with capacity C | Binary search on answer |
| 3 | For each day, how many days until a warmer temperature | Monotonic stack |
| 4 | Count subarrays whose sum is divisible by k | Prefix sum + HashMap on `sum%k` |
| 5 | Can these courses be finished given prerequisites | Topological sort / cycle detection |
| 6 | Number of provinces / friend circles | Union-Find or DFS components |
| 7 | Cheapest flight with at most k stops | Dijkstra with state `(node, stops)` or Bellman-Ford k rounds |
| 8 | Max sum of a non-adjacent subset | 1-D DP (House Robber) |
| 9 | Ways to make change for amount, order irrelevant | Unbounded knapsack, coins outer loop |
| 10 | Minimum window in S containing all chars of T | Sliding window (shortest) + count map |
| 11 | Median of a data stream | Two heaps |
| 12 | Kth largest element in an array | Heap size k or quickselect |
| 13 | Longest increasing subsequence, n = 1e5 | Patience sort + binary search |
| 14 | Word search II (many words in a grid) | Trie + DFS backtracking |
| 15 | Rotting oranges | Multi-source BFS |
| 16 | Largest rectangle in a histogram | Monotonic stack |
| 17 | Trapping rain water | Two pointers (or monotonic stack) |
| 18 | Merge k sorted linked lists | Min-heap of heads (or divide & conquer) |
| 19 | Find the duplicate number, O(1) space, array 1..n | Floyd's cycle detection |
| 20 | Minimum meeting rooms | Sort + min-heap on end times |
| 21 | Burst balloons for max coins | Interval DP |
| 22 | Shortest path visiting all nodes | Bitmask DP + BFS |
| 23 | Count numbers ≤ N with no repeating digits | Digit DP |
| 24 | Serialize and deserialize a binary tree | Preorder with null markers |
| 25 | Binary tree maximum path sum | Postorder DFS, return-downward/update-global |
| 26 | Lowest common ancestor with 1e5 queries | Binary lifting |
| 27 | Range sum query with updates | Fenwick / segment tree |
| 28 | Number of islands | DFS/BFS flood fill or DSU |
| 29 | Alien dictionary letter order | Build graph + topological sort |
| 30 | Maximum XOR of two numbers in an array | Bitwise trie |
| 31 | Critical connections in a network | Tarjan bridges |
| 32 | Reconstruct itinerary using all tickets | Eulerian path (Hierholzer) |
| 33 | Sliding window maximum | Monotonic deque |
| 34 | Partition array into two equal-sum subsets | Subset-sum DP |
| 35 | Best time to buy/sell stock with cooldown | State-machine DP |
| 36 | Design LRU cache | HashMap + doubly linked list |
| 37 | Random pick with weight | Prefix sums + binary search |
| 38 | Count primes below n | Sieve |
| 39 | (a^b) mod m with b = 1e18 | Fast power |
| 40 | Two players remove stones optimally | Game DP / minimax (or Grundy) |

**Where to log your score:** `06_Trackers.md § D`.
Any row you miss → it goes straight into your Anki `DSA-Triggers` deck **today**.

---

# PART 8 — HOW TO USE THIS FILE (the 4-week internalisation loop)

| When | What |
|---|---|
| **Once, now** | Read Parts 0–3 fully. Take the Part 7 self-test cold and record the score. |
| **Every morning, 5 min** | Anki review of Part 3 trigger cards |
| **Every Sunday, 20 min** | Type 5 random templates from Part 4 into a blank file, from memory |
| **After every failed problem** | Find its row in Part 3 or Part 6. If it's missing → **add it.** This file should grow. |
| **Every 4 weeks** | Retake the Part 7 self-test. Target: 30 → 36 → 40. |
| **Night before an interview** | Read Part 0, Part 1, and Part 6. Nothing else. |

> **When you can name the pattern in Part 7 for 36+/40, AND type 35+/40 templates from memory,
> your knowledge gap is closed by definition.** Everything after that is reps, speed, and nerves —
> which is exactly what `09_Contest_Strategy.md` is for.

