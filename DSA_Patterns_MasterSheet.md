# DSA Patterns Master Sheet — FAANG Interview Bible (5 YOE)

> Every pattern, when to use it, key signals to recognize it, and must-do problems.
> ★ = Missing from ThitaAiSheet (added by this guide)

---

## I. TWO POINTER PATTERNS

### Pattern 1: Converging (Left + Right)
**When to use:** Sorted array, find pairs/triplets with target sum, maximize/minimize distance between two elements, container/area problems.
**Signals:** "sorted array", "two numbers that sum to", "pair", "maximize area", "three numbers"
**Problems:** 11, 15, 16, 18, 167, 349, 881, 977, 259

### Pattern 2: Fast & Slow (Floyd's Cycle Detection)
**When to use:** Detect cycle in linked list or sequence, find duplicate in [1..n] array, determine if a sequence converges.
**Signals:** "cycle", "repeated", "loop", "happy number", "duplicate in range [1,n]"
**Problems:** 141, 142, 202, 287, 392

### Pattern 3: Fixed Separation (Two pointers at known gap)
**When to use:** Find kth from end, find middle of linked list.
**Signals:** "nth from end", "middle node", "delete middle"
**Problems:** 19, 876, 2095

### Pattern 4: In-place Array Modification
**When to use:** Remove/rearrange elements in-place with O(1) space, partition array by condition.
**Signals:** "in-place", "remove duplicates", "move zeroes", "sort colors", "O(1) space"
**Problems:** 26, 27, 75, 80, 283, 443, 905, 2337, 2938

### Pattern 5: String Comparison with Special Characters
**When to use:** Compare strings that have backspace/delete operations.
**Signals:** "backspace", "cancel character", "process string with deletions"
**Problems:** 844, 1598, 2390

### Pattern 6: Expanding From Center
**When to use:** Find palindromic substrings, expand outward from each index.
**Signals:** "palindromic substring", "longest palindrome", "count palindromes"
**Problems:** 5, 647

### Pattern 7: String Reversal
**When to use:** Reverse words, characters, vowels, or segments of a string.
**Signals:** "reverse", "flip", "words in reverse order"
**Problems:** 151, 344, 345, 541

### Pattern 8 ★: Trapping Water / Histogram (Two Pointer + Precompute)
**When to use:** Trapping rain water, find water trapped between bars.
**Signals:** "elevation map", "trapped water", "histogram bars"
**Problems:** 42. Trapping Rain Water, 407. Trapping Rain Water II

---

## II. SLIDING WINDOW PATTERNS

### Pattern 9: Fixed Size Window
**When to use:** Compute something over every subarray of size K.
**Signals:** "subarray of size k", "window of size k", "average of k elements"
**Problems:** 346, 643, 2985, 3254, 3318

### Pattern 10: Variable Size Window
**When to use:** Find smallest/longest subarray/substring satisfying a condition. Expand right to satisfy, shrink left to optimize.
**Signals:** "longest substring", "minimum window", "at most k distinct", "subarray sum ≥ target"
**Decision:** Use when the constraint is monotonic — if a window is valid, shrinking can only break it; if invalid, expanding can only fix it.
**Problems:** 3, 76, 209, 219, 424, 713, 904, 1004, 1438, 1493, 1658, 1838, 2461, 2516, 2762, 2779, 2981, 3026, 3346, 3347

### Pattern 11: Monotonic Deque for Window Max/Min
**When to use:** Find max/min in every window of size k, or optimize DP transitions.
**Signals:** "sliding window maximum", "max in every subarray of size k", "jump game with range"
**Problems:** 239, 862, 1696

### Pattern 12: Character Frequency Matching
**When to use:** Check if a window is a permutation/anagram of a pattern.
**Signals:** "anagram", "permutation in string", "character frequency match"
**Problems:** 1, 438, 567

### Pattern 13 ★: Sliding Window + HashMap (At Most K Distinct)
**When to use:** Count subarrays with exactly/at most K distinct elements. Use `atMost(k) - atMost(k-1)` trick.
**Signals:** "exactly k distinct", "at most k different", "subarrays with k colors"
**Problems:** 340. Longest Substring with At Most K Distinct Characters, 992. Subarrays with K Different Integers, 1248. Count Number of Nice Subarrays

---

## III. TREE TRAVERSAL PATTERNS (DFS & BFS)

### Pattern 14: Level Order Traversal (BFS)
**When to use:** Process tree level by level, find depth, right/left side view.
**Signals:** "level order", "by level", "right side view", "average of levels", "zigzag"
**Problems:** 102, 103, 199, 515, 1161

### Pattern 15: Recursive Preorder (Root → Left → Right)
**When to use:** Process node before children — construct tree, flatten, compare structure.
**Signals:** "construct tree from traversal", "flatten", "same tree", "invert"
**Problems:** 100, 101, 105, 114, 226, 257, 988

### Pattern 16: Recursive Inorder (Left → Root → Right)
**When to use:** BST problems — elements come out sorted in inorder.
**Signals:** "BST", "kth smallest", "validate BST", "sorted order"
**Problems:** 94, 98, 173, 230, 501, 530

### Pattern 17: Recursive Postorder (Left → Right → Root)
**When to use:** Need children's results before processing current node — height, diameter, path sum.
**Signals:** "depth", "height", "balanced", "diameter", "max path sum", "delete nodes"
**Problems:** 104, 110, 124, 145, 337, 366, 543, 863, 1110, 2458

### Pattern 18: Lowest Common Ancestor
**When to use:** Find common ancestor of two nodes.
**Signals:** "lowest common ancestor", "LCA", "common parent"
**Problems:** 235, 236, 1644, 1676

### Pattern 19: Serialization and Deserialization
**When to use:** Convert tree to string and back, detect duplicate subtrees.
**Signals:** "serialize", "encode tree", "duplicate subtrees"
**Problems:** 297, 572, 652

### Pattern 20 ★: Morris Traversal (O(1) Space Tree Traversal)
**When to use:** Inorder/preorder traversal without recursion or stack, O(1) space.
**Signals:** "O(1) space traversal", "threaded binary tree"
**Problems:** 99. Recover Binary Search Tree, 94 (Morris variant)

### Pattern 21 ★: Binary Tree Path Sum Variants
**When to use:** Find paths in tree with target sum, count paths, max/min path.
**Signals:** "path sum", "root to leaf sum", "any path equals target"
**Problems:** 112. Path Sum, 113. Path Sum II, 437. Path Sum III, 129. Sum Root to Leaf Numbers

---

## IV. GRAPH TRAVERSAL PATTERNS (DFS & BFS)

### Pattern 22: DFS - Connected Components / Island Counting
**When to use:** Count/mark connected regions in grid or adjacency list.
**Signals:** "number of islands", "connected components", "flood fill", "regions"
**Problems:** 130, 200, 417, 547, 695, 733, 841, 1020, 1254, 1905, 2101

### Pattern 23: BFS - Shortest Path in Unweighted Graph / Multi-source BFS
**When to use:** Shortest path in unweighted graph, nearest distance from multiple sources.
**Signals:** "shortest path unweighted", "minimum steps", "nearest 0", "rotting oranges"
**Problems:** 542, 994, 1091

### Pattern 24: DFS - Cycle Detection (Directed Graph)
**When to use:** Detect cycles in directed graph, find safe nodes.
**Signals:** "prerequisites", "cycle in directed graph", "course schedule", "safe states"
**Color coding: WHITE (unvisited) → GRAY (in progress) → BLACK (done). Cycle if you visit GRAY.
**Problems:** 207, 210, 802, 1059

### Pattern 25: BFS - Topological Sort (Kahn's Algorithm)
**When to use:** Order tasks with dependencies, detect cycle in DAG, parallel scheduling.
**Signals:** "order of tasks", "prerequisites", "dependency order", "build order", "parallel courses"
**Problems:** 210, 269, 310, 444, 1136, 1857, 2050, 2115, 2392

### Pattern 26: Deep Copy / Cloning
**When to use:** Clone a graph/linked list with random pointers.
**Signals:** "clone", "deep copy", "copy with random pointer"
**Problems:** 133, 138, 1334, 1490

### Pattern 27: Shortest Path - Dijkstra's Algorithm
**When to use:** Shortest path in weighted graph with non-negative weights.
**Signals:** "minimum cost path", "network delay", "weighted shortest path"
**Problems:** 743, 778, 1514, 1631, 1976, 2045, 2203, 2290, 2577, 2812

### Pattern 28: Shortest Path - Bellman-Ford / BFS with K stops
**When to use:** Shortest path with at most K edges, or graph with negative weights.
**Signals:** "at most k stops", "cheapest flight within k", "negative weights"
**Problems:** 787, 1129

### Pattern 29: Union-Find (Disjoint Set Union)
**When to use:** Dynamic connectivity, merge groups, detect cycle in undirected graph, connected components with union operations.
**Signals:** "connect", "merge accounts", "redundant connection", "are they connected?", "dynamic islands"
**Optimization:** Path compression + union by rank → nearly O(1) per operation.
**Problems:** 200, 261, 305, 323, 547, 684, 721, 737, 947, 952, 959, 1101

### Pattern 30: Strongly Connected Components (Kosaraju / Tarjan)
**When to use:** Find groups where every node can reach every other node in directed graph.
**Signals:** "strongly connected", "mutual reachability"
**Problems:** 210, 547, 1192, 2127

### Pattern 31: Bridges & Articulation Points (Tarjan low-link)
**When to use:** Find edges/nodes whose removal disconnects the graph.
**Signals:** "critical connections", "bridges", "articulation points", "single point of failure"
**Problems:** 1192, 2360

### Pattern 32: Minimum Spanning Tree (Kruskal / Prim)
**When to use:** Connect all nodes with minimum total edge weight.
**Signals:** "minimum cost to connect", "spanning tree", "connect all cities"
**Problems:** 1135, 1584, 1168, 1489

### Pattern 33: Bidirectional BFS
**When to use:** Shortest path between two known nodes — search from both ends to reduce branching.
**Signals:** "word ladder", "transformation sequence", "shortest transformation"
**Problems:** 127, 126, 815

### Pattern 34 ★: Graph Coloring / Bipartite Check
**When to use:** Check if graph is 2-colorable, assign groups without conflict.
**Signals:** "bipartite", "two groups", "can we split into 2 sets", "odd cycle"
**Problems:** 785. Is Graph Bipartite?, 886. Possible Bipartition

### Pattern 35 ★: Euler Path / Hamilton Path
**When to use:** Visit every edge exactly once (Euler) or every node exactly once (Hamilton).
**Signals:** "reconstruct itinerary", "visit every edge once"
**Problems:** 332. Reconstruct Itinerary, 753. Cracking the Safe

### Pattern 36 ★: Multi-State BFS / BFS with State
**When to use:** BFS where the state is more than just position (e.g., position + keys collected, position + walls broken).
**Signals:** "shortest path with conditions", "keys and locks", "break at most k walls"
**Problems:** 864. Shortest Path to Get All Keys, 1293. Shortest Path in a Grid with Obstacles Elimination, 847. Shortest Path Visiting All Nodes

---

## V. DYNAMIC PROGRAMMING (DP) PATTERNS

### Pattern 37: Fibonacci Style (Linear DP)
**When to use:** Current state depends on previous 1-2 states.
**Signals:** "climbing stairs", "ways to reach", "house robber", "decode ways"
**Problems:** 70, 91, 198, 213, 337, 509, 740, 746

### Pattern 38: Kadane's Algorithm (Max/Min Subarray)
**When to use:** Maximum/minimum sum contiguous subarray.
**Signals:** "maximum subarray", "max sum contiguous", "max product subarray"
**Decision:** Reset or extend — `dp[i] = max(nums[i], dp[i-1] + nums[i])`
**Problems:** 53, 152, 918, 1749, 2321

### Pattern 39: Coin Change / Unbounded Knapsack
**When to use:** Unlimited items, find min coins or number of ways to make amount.
**Signals:** "unlimited supply", "coin change", "minimum number of coins", "combination sum with repetition"
**Problems:** 322, 377, 518

### Pattern 40: 0/1 Knapsack / Subset Sum
**When to use:** Each item used at most once, partition into subsets, target sum with +/-.
**Signals:** "partition equal subset", "target sum with +/-", "select or skip each item"
**Problems:** 416, 494, 1049. Last Stone Weight II

### Pattern 41: Word Break
**When to use:** Can a string be segmented into dictionary words?
**Signals:** "word break", "segment string", "dictionary words"
**Problems:** 139, 140

### Pattern 42: Longest Common Subsequence (LCS)
**When to use:** Two sequences, find longest common/shared subsequence.
**Signals:** "longest common subsequence", "edit operations on two strings", "shortest common supersequence"
**Problems:** 1143, 1092, 1312

### Pattern 43: Edit Distance / String Transformation
**When to use:** Minimum operations to convert one string to another.
**Signals:** "edit distance", "minimum deletions", "transform string"
**Problems:** 72, 583, 712

### Pattern 44: Grid DP (Unique Paths / Min Path Sum)
**When to use:** Count paths or find optimal path in a grid moving right/down.
**Signals:** "unique paths", "minimum path sum", "grid traversal", "triangle"
**Problems:** 62, 63, 64, 120, 221, 931, 1277

### Pattern 45: Interval DP
**When to use:** Optimal way to merge/split intervals, process subarray [i..j].
**Signals:** "burst balloons", "merge stones", "optimal BST", "matrix chain"
**Problems:** 312, 546, 1039. Minimum Score Triangulation of Polygon

### Pattern 46: Catalan Numbers
**When to use:** Count structurally unique BSTs, valid parentheses combinations, polygon triangulations.
**Signals:** "unique BSTs", "structurally unique", "number of ways to parenthesize"
**Problems:** 95, 96, 241

### Pattern 47: Longest Increasing Subsequence (LIS)
**When to use:** Longest strictly increasing subsequence, patience sorting.
**Signals:** "longest increasing", "envelopes", "mountain array"
**Optimization:** O(n log n) using binary search + patience sorting.
**Problems:** 300, 354, 1671, 2407

### Pattern 48: Stock Buy/Sell with State Machine
**When to use:** Buy/sell stock with constraints (cooldown, transaction limit, fee).
**Signals:** "buy and sell stock", "at most k transactions", "cooldown", "transaction fee"
**State Machine:** `hold`, `sold`, `rest` states with transitions.
**Problems:** 121, 122, 123, 188, 309, 714. Best Time to Buy and Sell Stock with Transaction Fee

### Pattern 49 ★: Bitmask DP
**When to use:** DP over subsets, N ≤ 20, assign items to groups, TSP-like problems.
**Signals:** "assign n items", "visit all nodes", "small n (≤ 20)", "subsets", "TSP"
**State:** `dp[mask]` where each bit represents whether item i is used.
**Problems:** 526. Beautiful Arrangement, 691. Stickers to Spell Word, 698. Partition to K Equal Sum Subsets, 847. Shortest Path Visiting All Nodes, 943. Find the Shortest Superstring, 1494. Parallel Courses II, 1595. Minimum Cost to Connect Two Groups of Points, 2035. Partition Array Into Two Arrays to Minimize Sum Difference

### Pattern 50 ★: Digit DP
**When to use:** Count numbers in range [L, R] satisfying digit-based constraints.
**Signals:** "count numbers from 1 to N with property", "digit sum", "numbers without repeated digits"
**State:** `dp[position][tight][...constraints]`
**Problems:** 233. Number of Digit One, 357. Count Numbers with Unique Digits, 600. Non-negative Integers without Consecutive Ones, 902. Numbers At Most N Given Digit Set, 1012. Numbers With Repeated Digits, 2376. Count Special Integers

### Pattern 51 ★: DP on Trees (Rerooting)
**When to use:** Compute answer for every node as root — "what if this node were the root?"
**Signals:** "sum of distances to all nodes", "answer for each node as root"
**Technique:** Two DFS passes — first root at node 0, then re-root to children.
**Problems:** 310. Minimum Height Trees, 834. Sum of Distances in Tree, 2581. Count Number of Possible Root Nodes

### Pattern 52 ★: Probability / Expected Value DP
**When to use:** Expected number of steps, probability of reaching a state.
**Signals:** "probability", "expected value", "random walk", "dice"
**Problems:** 688. Knight Probability in Chessboard, 808. Soup Servings, 837. New 21 Game, 1230. Toss Strange Coins

### Pattern 53 ★: String DP (Distinct Subsequences / Interleaving)
**When to use:** Count distinct subsequences, check interleaving, regex matching.
**Signals:** "distinct subsequences", "interleaving string", "regex match", "wildcard"
**Problems:** 10. Regular Expression Matching, 44. Wildcard Matching, 97. Interleaving String, 115. Distinct Subsequences, 940. Distinct Subsequences II

### Pattern 54 ★: DP with Monotonic Stack/Deque Optimization
**When to use:** DP transition looks at a range of previous states — optimize from O(n²) to O(n).
**Signals:** "jump game with range", "max sum with gap constraint"
**Problems:** 1696. Jump Game VI, 1425. Constrained Subsequence Sum, 2944. Minimum Number of Coins for Fruits

---

## VI. HEAP (PRIORITY QUEUE) PATTERNS

### Pattern 55: Top K Elements
**When to use:** Find kth largest/smallest, top k frequent elements.
**Signals:** "kth largest", "top k", "k most frequent", "k closest"
**Decision:** Min-heap of size k for kth largest. Max-heap of size k for kth smallest.
**Problems:** 215, 347, 451, 506, 703, 973, 1046, 2558

### Pattern 56: Two Heaps for Median
**When to use:** Maintain running median of a data stream.
**Signals:** "median from stream", "median dynamically"
**Technique:** Max-heap for lower half, min-heap for upper half.
**Problems:** 295, 1825, 480. Sliding Window Median

### Pattern 57: K-way Merge
**When to use:** Merge k sorted lists/arrays, find kth smallest across sorted collections.
**Signals:** "merge k sorted", "k sorted lists", "kth smallest in matrix"
**Problems:** 23, 373, 378, 632

### Pattern 58: Scheduling / Greedy with Heap
**When to use:** Schedule meetings, allocate resources, minimize cost with priority.
**Signals:** "meeting rooms", "minimum cost", "schedule tasks", "furthest building"
**Problems:** 253, 767, 857, 1642, 1792, 1834, 1942, 2402

### Pattern 59 ★: Lazy Deletion Heap
**When to use:** Need to delete arbitrary elements from heap but heap doesn't support it.
**Signals:** "remove specific element from heap", "invalidate entries"
**Technique:** Mark as deleted, skip when popping.
**Problems:** 480. Sliding Window Median, 2353. Design a Food Rating System

---

## VII. BACKTRACKING PATTERNS

### Pattern 60: Subsets (Include/Exclude)
**When to use:** Generate all subsets, combinations of size k.
**Signals:** "all subsets", "power set", "combinations", "letter combinations"
**Dedup:** Sort + skip duplicates for Subsets II.
**Problems:** 17, 77, 78, 90

### Pattern 61: Permutations
**When to use:** Generate all orderings.
**Signals:** "all permutations", "next permutation", "arrangements"
**Problems:** 31, 46, 47. Permutations II, 60

### Pattern 62: Combination Sum
**When to use:** Find combinations that sum to target (with/without reuse).
**Signals:** "combination sum", "numbers that add up to target"
**Problems:** 39, 40, 216. Combination Sum III

### Pattern 63: Parentheses Generation
**When to use:** Generate valid parentheses, remove minimum invalid parentheses.
**Signals:** "generate parentheses", "valid parentheses combinations"
**Problems:** 22, 301

### Pattern 64: Word Search / Grid Path Finding
**When to use:** Find word in grid by moving to adjacent cells.
**Signals:** "word search in grid", "path in matrix", "find word"
**Problems:** 79, 212, 2018

### Pattern 65: N-Queens / Constraint Satisfaction
**When to use:** Place items satisfying multiple constraints.
**Signals:** "n-queens", "sudoku", "place items without conflict"
**Problems:** 37, 51, 52. N-Queens II

### Pattern 66: Palindrome Partitioning
**When to use:** Split string into palindromic parts.
**Signals:** "partition into palindromes", "palindrome partitioning"
**Problems:** 131, 132, 1457

---

## VIII. GREEDY PATTERNS

### Pattern 67: Interval Merging / Scheduling
**When to use:** Merge overlapping intervals, find free time, intersections.
**Signals:** "merge intervals", "overlapping", "meeting rooms", "insert interval"
**Sort by:** Start time (merge), end time (scheduling/max non-overlapping).
**Problems:** 56, 57, 435. Non-overlapping Intervals, 452. Minimum Number of Arrows to Burst Balloons, 759, 986, 2406

### Pattern 68: Jump Game (Reachability / Minimization)
**When to use:** Can you reach the end? Minimum jumps to reach end?
**Signals:** "jump game", "can reach", "minimum jumps"
**Problems:** 45, 55, 1306. Jump Game III, 1345. Jump Game IV

### Pattern 69: Buy/Sell Stock (Greedy variant)
**When to use:** Single or unlimited transactions (no cooldown/limit).
**Signals:** "best time to buy and sell", "maximum profit"
**Problems:** 121, 122

### Pattern 70: Gas Station / Circular
**When to use:** Find start point in circular route where resource never runs out.
**Signals:** "gas station", "circular route", "enough fuel"
**Problems:** 134, 2202

### Pattern 71: Task Scheduling
**When to use:** Schedule tasks with cooldown, rearrange with constraints.
**Signals:** "task scheduler", "cooldown between same tasks", "reorganize"
**Problems:** 621, 767, 1054

### Pattern 72: Sorting-Based Greedy
**When to use:** Sort by some criterion, then greedily assign.
**Signals:** "assign cookies", "queue reconstruction", "two city scheduling", "candy"
**Problems:** 135, 406, 455, 1029

### Pattern 73 ★: Line Sweep
**When to use:** Process events sorted by coordinate, track active intervals/shapes.
**Signals:** "overlapping intervals at a point", "skyline", "rectangle area", "max overlap", "meeting rooms"
**Technique:** Convert intervals to +1 (start) and -1 (end) events, sort and sweep.
**Problems:** 218. The Skyline Problem, 252. Meeting Rooms, 253. Meeting Rooms II, 391. Perfect Rectangle, 850. Rectangle Area II, 1589. Maximum Sum Obtained of Any Permutation, 2021. Brightest Position on Street, 2158. Amount of New Area Painted Each Day

### Pattern 74 ★: Greedy + Sorting by Two Keys
**When to use:** Items have two attributes; sort by one, greedily process by other.
**Signals:** "people heights", "envelopes", "activities with start+end"
**Problems:** 354, 406, 452, 630. Course Schedule III, 1851. Minimum Interval to Include Each Query

### Pattern 75 ★: Greedy Deletion / Stack-Based Greedy
**When to use:** Remove k digits/characters to optimize result, build smallest/largest number.
**Signals:** "remove k digits", "most competitive subsequence", "smallest number after removal"
**Problems:** 316. Remove Duplicate Letters, 402. Remove K Digits, 1673. Find the Most Competitive Subsequence, 321. Create Maximum Number

---

## IX. BINARY SEARCH PATTERNS

### Pattern 76: Standard Binary Search on Sorted Array
**When to use:** Find target in sorted array, search insert position.
**Signals:** "sorted array", "find target", "search position"
**Problems:** 35, 69, 74, 278, 374, 540, 704, 1539

### Pattern 77: Rotated Sorted Array / Peak Finding
**When to use:** Array was sorted then rotated, find min/target/peak.
**Signals:** "rotated sorted", "peak element", "mountain array"
**Decision:** Compare `mid` with `right` to determine which half is sorted.
**Problems:** 33, 81, 153, 162, 852, 1095

### Pattern 78: Binary Search on Answer (Parametric Search)
**When to use:** Minimize the maximum or maximize the minimum of something. The answer is monotonic — if X works, then X+1 also works (or vice versa).
**Signals:** "minimize the maximum", "maximize the minimum", "minimum days/speed/capacity to achieve goal"
**Template:** Binary search on answer space, use a `canAchieve(mid)` helper.
**Problems:** 410, 774, 875, 1011, 1482, 1760, 2064, 2226, 1283. Find the Smallest Divisor Given a Threshold, 2560. House Robber IV

### Pattern 79: Find First/Last Occurrence (Bisect Left/Right)
**When to use:** Find leftmost/rightmost position of target in sorted array with duplicates.
**Signals:** "first and last position", "lower bound", "upper bound"
**Problems:** 34, 658

### Pattern 80: Median / Kth Across Sorted Arrays
**When to use:** Find kth element across two sorted arrays, kth smallest pair distance.
**Signals:** "median of two sorted arrays", "kth smallest pair"
**Problems:** 4, 378, 719

### Pattern 81 ★: Binary Search + Prefix Sum
**When to use:** Random pick with weight, find threshold in prefix sums.
**Signals:** "random pick weighted", "cumulative probability"
**Problems:** 528. Random Pick with Weight, 497. Random Point in Non-overlapping Rectangles

---

## X. STACK PATTERNS

### Pattern 82: Valid Parentheses Matching
**When to use:** Check if brackets are balanced, find longest valid, minimum removals.
**Signals:** "valid parentheses", "balanced brackets", "minimum remove"
**Problems:** 20, 32, 921, 1249, 1963

### Pattern 83: Monotonic Stack (Next Greater/Smaller Element)
**When to use:** For each element, find next/previous greater/smaller element.
**Signals:** "next greater", "daily temperatures", "stock span", "subarray minimums"
**Technique:** Maintain stack in decreasing (for next greater) or increasing (for next smaller) order.
**Problems:** 402, 496, 503, 739, 901, 907, 962, 1475, 1673

### Pattern 84: Expression Evaluation
**When to use:** Evaluate math expressions with operators and parentheses.
**Signals:** "calculator", "evaluate expression", "reverse polish notation"
**Problems:** 150, 224, 227, 772

### Pattern 85: Simulation / Nesting Helper
**When to use:** Process nested structures, decode strings, simulate collisions.
**Signals:** "decode string", "nested brackets", "asteroid collision", "simplify path"
**Problems:** 71, 394, 735

### Pattern 86: Min Stack / Special Stack Design
**When to use:** Stack with O(1) getMin/getMax, frequency stack.
**Signals:** "min stack", "max frequency stack", "O(1) minimum"
**Problems:** 155, 895, 901

### Pattern 87: Largest Rectangle in Histogram
**When to use:** Find largest rectangle under histogram bars, maximal rectangle in matrix.
**Signals:** "largest rectangle", "histogram", "maximal rectangle of 1s"
**Technique:** Monotonic stack to find left/right boundaries for each bar.
**Problems:** 84, 85

---

## XI. BIT MANIPULATION PATTERNS

### Pattern 88: XOR — Finding Single/Missing Number
**When to use:** Find the one element that appears odd times, find missing number.
**Signals:** "single number", "appears once", "missing number", "XOR pairs"
**Key:** `a ^ a = 0`, `a ^ 0 = a`
**Problems:** 136, 137, 260. Single Number III, 268, 389

### Pattern 89: Counting Set Bits / Hamming
**When to use:** Count 1-bits, hamming distance between numbers.
**Signals:** "number of 1 bits", "hamming distance", "total hamming"
**Problems:** 191, 231, 477

### Pattern 90: Bitwise DP — Counting Bits
**When to use:** Count bits for all numbers 0..n, subset XOR.
**Signals:** "counting bits", "dp with XOR"
**Problems:** 338, 1442, 1494

### Pattern 91: Power of Two/Four Check
**When to use:** Check if number is power of 2 or 4.
**Signals:** "power of two", "power of four"
**Key:** `n & (n-1) == 0` for power of 2.
**Problems:** 231, 342

### Pattern 92 ★: Bit Manipulation — Subsets Enumeration
**When to use:** Iterate over all subsets of a bitmask.
**Signals:** "enumerate subsets of a mask", "submask enumeration"
**Technique:** `submask = (submask - 1) & mask` to iterate all submasks.
**Problems:** 78 (bit approach), 698, 1994. The Number of Good Subsets

---

## XII. LINKED LIST MANIPULATION PATTERNS

### Pattern 93: In-place Reversal
**When to use:** Reverse entire list or portion, check palindrome.
**Signals:** "reverse linked list", "reverse between positions", "palindrome linked list"
**Problems:** 83, 92, 206, 25, 234, 82

### Pattern 94: Merging Sorted Lists
**When to use:** Merge two or k sorted linked lists.
**Signals:** "merge sorted lists", "merge k lists"
**Problems:** 21, 23

### Pattern 95: Number Addition
**When to use:** Add numbers represented as linked lists.
**Signals:** "add two numbers", "plus one linked list"
**Problems:** 2, 369, 445. Add Two Numbers II

### Pattern 96: Intersection Detection
**When to use:** Find intersection node of two linked lists.
**Signals:** "intersection point", "where two lists meet"
**Technique:** Two pointers, swap heads when reaching end.
**Problems:** 160, 599

### Pattern 97: Reordering / Partitioning
**When to use:** Reorder nodes, swap pairs, rotate, partition around value.
**Signals:** "reorder list", "swap pairs", "rotate", "partition"
**Problems:** 24, 61, 86, 143, 328

---

## XIII. ARRAY / MATRIX MANIPULATION PATTERNS

### Pattern 98: In-place Rotation
**When to use:** Rotate matrix 90°, rotate array by k positions.
**Signals:** "rotate image", "rotate array"
**Technique:** Transpose + reverse for 90° rotation.
**Problems:** 48, 189, 867

### Pattern 99: Spiral Traversal
**When to use:** Read/fill matrix in spiral order.
**Signals:** "spiral order", "spiral matrix"
**Problems:** 54, 59, 885, 2326

### Pattern 100: In-place Marking
**When to use:** Use the matrix itself to mark states, avoid extra space.
**Signals:** "set zeroes", "game of life", "O(1) space matrix"
**Problems:** 73, 289, 498

### Pattern 101: Prefix Sum / Prefix Product
**When to use:** Range sum queries, product except self, subarray sum equals K.
**Signals:** "product except self", "subarray sum", "range sum"
**Problems:** 238, 303. Range Sum Query, 304. Range Sum Query 2D, 560. Subarray Sum Equals K, 845, 2483

### Pattern 102: Plus One / String Math
**When to use:** Arithmetic on numbers represented as arrays/strings.
**Signals:** "plus one", "add binary", "multiply strings"
**Problems:** 43, 66, 67, 989

### Pattern 103: In-place from End (Merge from Back)
**When to use:** Merge sorted arrays in-place using extra space at end.
**Signals:** "merge sorted array in-place"
**Problems:** 88, 977

### Pattern 104: Cyclic Sort
**When to use:** Array contains numbers in range [0,n] or [1,n], find missing/duplicate.
**Signals:** "numbers in range 1 to n", "find missing", "find duplicate", "first missing positive"
**Technique:** Place each number at its correct index.
**Problems:** 41, 268, 287, 442, 448

### Pattern 105 ★: Prefix XOR
**When to use:** XOR of subarray [i..j] = prefix[j+1] ^ prefix[i]. Count pairs with XOR condition.
**Signals:** "subarray XOR", "XOR queries"
**Problems:** 1310. XOR Queries of a Subarray, 1442. Count Triplets

### Pattern 106 ★: Difference Array
**When to use:** Apply range updates [l, r] += val efficiently.
**Signals:** "range increment", "add value to range", "booking", "car pooling"
**Technique:** `diff[l] += val`, `diff[r+1] -= val`, then prefix sum.
**Problems:** 370. Range Addition, 1094. Car Pooling, 1109. Corporate Flight Bookings, 2381. Shifting Letters II

---

## XIV. STRING MANIPULATION PATTERNS

### Pattern 107: Palindrome Check
**When to use:** Check if string is palindrome with constraints.
**Signals:** "valid palindrome", "at most one deletion"
**Problems:** 9, 125, 680

### Pattern 108: Anagram Check / Grouping
**When to use:** Check anagram, group anagrams — sort or use frequency count.
**Signals:** "anagram", "group anagrams"
**Problems:** 49, 242

### Pattern 109: Roman ↔ Integer
**When to use:** Convert roman numerals to/from integers.
**Problems:** 12, 13

### Pattern 110: String to Integer (atoi)
**When to use:** Parse number from string with edge cases.
**Problems:** 8, 65

### Pattern 111: Manual Simulation (Big Number Math)
**When to use:** Multiply/add large numbers as strings.
**Problems:** 43, 67, 415

### Pattern 112: String Matching (KMP / Rabin-Karp / Z-Algorithm)
**When to use:** Find pattern in text efficiently, repeated patterns.
**Signals:** "find substring", "pattern matching", "shortest palindrome prefix"
**KMP:** O(n+m), build failure function.
**Rabin-Karp:** Rolling hash, good for multiple pattern search.
**Z-Algorithm:** Z-array, pattern matching alternative.
**Problems:** 28, 214, 686, 796, 3008

### Pattern 113: Repeated Substring Detection
**When to use:** Check if string is built by repeating a substring.
**Signals:** "repeated substring", "string period"
**Problems:** 459, 28, 686

### Pattern 114 ★: Suffix Array / Suffix Automaton
**When to use:** Complex substring queries, longest repeated substring.
**Signals:** "longest repeated substring", "number of distinct substrings"
**Problems:** 1044. Longest Duplicate Substring, 1698. Number of Distinct Substrings in a String

---

## XV. DESIGN PATTERNS (Data Structure Design)

### Pattern 115: LRU / LFU Cache
**When to use:** Design cache with eviction policy.
**Signals:** "LRU cache", "LFU cache", "evict least recently/frequently used"
**Technique:** HashMap + Doubly Linked List for O(1) LRU.
**Problems:** 146, 460

### Pattern 116: General Data Structure Design
**When to use:** Design specific data structures with required time complexities.
**Signals:** "design", "implement", "O(1) insert/delete/getRandom"
**Problems:** 155, 225, 232, 251, 271, 295, 341, 346, 353, 359, 362, 379, 380, 432, 604, 622, 641, 642, 706, 715, 900, 981, 1146, 1348, 1352, 1381, 1756, 2013, 2034, 2296, 2336

### Pattern 117: Trie (Prefix Tree)
**When to use:** Autocomplete, prefix search, word dictionary with wildcards.
**Signals:** "prefix", "autocomplete", "starts with", "word search with '.' wildcard"
**Problems:** 208, 211, 425, 642, 648, 720, 745

---

## XVI. MATH & NUMBER THEORY PATTERNS ★

### Pattern 118 ★: GCD / LCM (Euclidean Algorithm)
**When to use:** Reduce fractions, find common divisors, LCM-based problems.
**Signals:** "GCD", "greatest common divisor", "fraction simplification"
**Problems:** 365. Water and Jug Problem, 1071. Greatest Common Divisor of Strings, 2344. Minimum Deletions to Make Array Divisible

### Pattern 119 ★: Sieve of Eratosthenes
**When to use:** Find all primes up to N.
**Signals:** "count primes", "prime numbers up to n"
**Problems:** 204. Count Primes, 952. Largest Component Size by Common Factor

### Pattern 120 ★: Modular Arithmetic / Fast Exponentiation
**When to use:** Large results mod 10^9+7, matrix exponentiation for Fibonacci.
**Signals:** "answer mod 10^9+7", "power mod", "large exponent"
**Problems:** 50. Pow(x, n), 372. Super Pow, 1922. Count Good Numbers

### Pattern 121 ★: Reservoir Sampling
**When to use:** Random sampling from stream of unknown size.
**Signals:** "random node from linked list", "random from stream"
**Problems:** 382. Linked List Random Node, 398. Random Pick Index

### Pattern 122 ★: Fisher-Yates Shuffle
**When to use:** Generate random permutation uniformly.
**Signals:** "shuffle array", "random permutation"
**Problems:** 384. Shuffle an Array

### Pattern 123 ★: Boyer-Moore Voting Algorithm
**When to use:** Find majority element (appears > n/2 or > n/3 times).
**Signals:** "majority element", "appears more than n/2 times"
**Problems:** 169. Majority Element, 229. Majority Element II

---

## XVII. ADVANCED / MISCELLANEOUS PATTERNS ★

### Pattern 124 ★: Segment Tree
**When to use:** Range queries + point/range updates in O(log n).
**Signals:** "range sum with updates", "range min/max with updates", "count of elements in range"
**Variants:** Lazy propagation for range updates.
**Problems:** 307. Range Sum Query - Mutable, 315. Count of Smaller Numbers After Self, 493. Reverse Pairs, 2426. Number of Pairs Satisfying Inequality

### Pattern 125 ★: Binary Indexed Tree (Fenwick Tree)
**When to use:** Same as segment tree but simpler — prefix sums with updates.
**Signals:** "prefix sum with updates", "count inversions", "range sum mutable"
**Problems:** 307, 315, 493, 1649. Create Sorted Array through Instructions

### Pattern 126 ★: Ordered Set / SortedList / Policy-based DS
**When to use:** Maintain sorted order with O(log n) insert/delete/rank queries.
**Signals:** "count smaller/greater", "rank in sorted set", "sliding window with sorted order"
**Problems:** 220. Contains Duplicate III, 315, 327. Count of Range Sum, 1649

### Pattern 127 ★: Coordinate Compression
**When to use:** Values are huge but count is small — map to [0, n).
**Signals:** "large coordinates", "sparse values"
**Often combined with:** Segment tree, BIT, sweep line.

### Pattern 128 ★: Offline Query Processing
**When to use:** All queries known upfront — sort queries to process efficiently.
**Signals:** "answer queries about ranges", "offline", "Mo's algorithm"
**Problems:** 1851. Minimum Interval to Include Each Query

### Pattern 129 ★: Rolling Hash (Rabin-Karp)
**When to use:** Compare substrings in O(1), longest duplicate substring.
**Signals:** "duplicate substring", "compare substrings efficiently"
**Problems:** 187. Repeated DNA Sequences, 1044. Longest Duplicate Substring, 1062. Longest Repeating Substring

### Pattern 130 ★: Meet in the Middle
**When to use:** Brute force is 2^N but N ≤ 40 — split into two halves of 2^20.
**Signals:** "subset sum with N ≤ 40", "split in half"
**Problems:** 1755. Closest Subsequence Sum, 2035. Partition Array Into Two Arrays to Minimize Sum Difference

### Pattern 131 ★: Topological Sort + DP (DAG DP)
**When to use:** Longest/shortest path in DAG, count paths in DAG.
**Signals:** "longest path in DAG", "count paths with constraints in directed graph"
**Problems:** 1857. Largest Color Value in a Directed Graph, 2050. Parallel Courses III, 329. Longest Increasing Path in a Matrix

### Pattern 132 ★: Matrix Exponentiation
**When to use:** Solve linear recurrences in O(log n).
**Signals:** "nth fibonacci in O(log n)", "linear recurrence with huge n"
**Problems:** 509 (optimized), 1137. N-th Tribonacci Number (optimized)

### Pattern 133 ★: Minimax / Game Theory
**When to use:** Two players, both play optimally, determine winner.
**Signals:** "game", "two players", "optimal play", "stone game", "predict winner"
**Problems:** 292. Nim Game, 464. Can I Win, 486. Predict the Winner, 877. Stone Game, 1140. Stone Game II

---

## XVIII. QUICK REFERENCE: PATTERN SELECTION GUIDE

| If you see... | Think... |
|---|---|
| Sorted array + find pair | Two Pointers (converging) |
| Contiguous subarray/substring + condition | Sliding Window |
| Tree structure | DFS/BFS (pick traversal order based on need) |
| Graph + connectivity | Union-Find or DFS |
| Graph + shortest path (unweighted) | BFS |
| Graph + shortest path (weighted) | Dijkstra / Bellman-Ford |
| Graph + ordering/dependencies | Topological Sort |
| Optimal substructure + overlapping subproblems | DP |
| "Minimum/Maximum" + "split/partition" + monotonic | Binary Search on Answer |
| Generate all possibilities | Backtracking |
| Intervals + overlap/merge | Sort + Greedy / Line Sweep |
| Next greater/smaller element | Monotonic Stack |
| Stream of data + median/kth | Heap |
| Counting in range [L, R] by digit property | Digit DP |
| N ≤ 20 + assign/permute | Bitmask DP |
| Range query + updates | Segment Tree / BIT |
| "Missing/duplicate in [1..n]" | Cyclic Sort or XOR |
| "Majority element" | Boyer-Moore Voting |
| "Random from stream" | Reservoir Sampling |
| Range increment/decrement events | Difference Array |
| Prefix/autocomplete | Trie |
| Two players, optimal play | Minimax / Game Theory DP |
| Brute force 2^N, N ≤ 40 | Meet in the Middle |
| Events at coordinates, active count | Line Sweep |

---

**Total: 133 Patterns | 700+ Problems | Covers Every FAANG DSA Scenario**

> Master these patterns and you won't see a single problem in a FAANG interview that doesn't map to one (or a combination) of these. The key is RECOGNITION — see the signals, pick the pattern, adapt the template. Good luck! 🚀

