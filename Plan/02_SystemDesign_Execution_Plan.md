# 02 — SYSTEM DESIGN (HLD) EXECUTION PLAN

> **Mission:** Turn `../SystemDesign_HLD_Topics.md` (a great *index*) into **24 rehearsed,
> timed, out-loud designs** with real numbers and real trade-offs.
> At 5.3 YOE this round decides **SDE1 vs SDE2 vs SDE3**. Your existing HLD strength is the
> single biggest lever you have — sharpen it into an unfair advantage.

---

## 1. WHAT INTERVIEWERS ACTUALLY SCORE (know the rubric)

| Signal | SDE1 answer | **SDE2 answer (your target)** | SDE3 answer |
|--------|-------------|-------------------------------|-------------|
| Requirements | Starts designing immediately | **Drives clarification, states scope + non-goals** | Negotiates scope with business framing |
| Estimation | Skips it | **QPS, storage, bandwidth with real numbers** | Uses numbers to *drive* design choices |
| API/Data model | Vague | **Concrete endpoints + schema + index choice** | Discusses evolution/versioning |
| HLD | Boxes and arrows | **Justified components, data flow end-to-end** | Multiple viable architectures compared |
| Deep dive | Waits to be asked | **Volunteers 2 deep dives, owns the trade-offs** | Drives to the hardest sub-problem |
| Failure modes | Not mentioned | **Bottlenecks, hot keys, SPOF, degradation** | Operational: rollout, migration, on-call |
| Communication | Answers questions | **Leads the session, checks in, time-manages** | Interviewer feels like a collaborator |

> **The #1 reason 5-YOE candidates get down-levelled: they describe components instead of
> defending trade-offs.** Every claim must be followed by "…because" and "…the cost is".

---

## 2. THE 45-MINUTE FRAMEWORK (drill until automatic)

```
 0–5 min   REQUIREMENTS
           Functional (3–5 bullets) · Non-functional (scale, latency, consistency, availability)
           Explicit NON-goals. "I'll focus on X, Y; I'll skip Z unless you want it."

 5–10 min  ESTIMATION  ← never skip, this is where SDE2 is proven
           DAU → QPS (peak = 2–3× avg) → storage/day, /year → bandwidth → #servers, #shards
           Write it on the board. Reference these numbers later to justify decisions.

10–15 min  API + DATA MODEL
           3–5 endpoints (verb, path, params, response). Then: entities, PK, indexes,
           SQL vs NoSQL WITH the reason. Estimate row size × rows.

15–28 min  HIGH-LEVEL DESIGN
           Client → LB → API GW → services → cache → DB → queue → workers → storage
           Walk ONE write path and ONE read path end-to-end out loud.

28–40 min  DEEP DIVES (pick 2, or ask "which would you like me to go deeper on?")
           Sharding key · caching strategy + invalidation · consistency model ·
           hot key/celebrity problem · idempotency · fan-out read vs write · backpressure

40–45 min  BOTTLENECKS, FAILURE, WRAP
           SPOFs · what breaks at 10× · graceful degradation · monitoring & alerts ·
           "if I had more time I'd explore ___"
```

**Practice rule: every case study is done TWICE.** First pass = learn (unlimited time, write-up).
Second pass = perform (45-min timer, recorded, no notes). Second pass is what actually counts.

---

## 3. THE NUMBERS YOU MUST KNOW COLD (Anki deck: `SysDesign-Numbers`)

| Quantity | Value |
|----------|-------|
| L1 cache ref | 0.5 ns |
| Main memory ref | 100 ns |
| SSD random read | 100 µs |
| Round trip within DC | 500 µs |
| Disk seek (HDD) | 10 ms |
| RTT India ↔ US | ~150 ms |
| 1 MB seq read from memory | ~10 µs |
| 1 MB seq read from SSD | ~100 µs–1 ms |
| Seconds in a day | 86,400 (~10⁵) |
| 1 M DAU, 10 req/user/day | ~115 QPS avg, ~300 peak |
| Single MySQL box | ~1–5 K QPS (reads w/ cache: 10× more) |
| Redis single node | ~100 K ops/s |
| Kafka single broker | ~100 K–1 M msg/s |
| Modern server | 64 GB RAM, 8–32 cores, ~1 Gbps NIC |
| Char = 1 B · int = 4 B · long/double = 8 B · UUID = 16 B · timestamp = 8 B |
| 1 KB tweet × 500 M/day | 500 GB/day ≈ 180 TB/year |
| Availability | 99.9% = 8.7 h/yr · 99.99% = 52 min/yr · 99.999% = 5 min/yr |

**Estimation shortcut to say out loud:**
`QPS_avg = DAU × actions_per_day / 86400` → `QPS_peak ≈ 3 × QPS_avg`
`Storage/day = writes/day × avg_object_size` → `×365 ×replication_factor(3)`

---

## 4. PHASE A — FUNDAMENTALS (Weeks 1–4, Tue + Thu evenings)

Source: your `../SystemDesign_HLD_Topics.md` + DDIA chapters. **Deliverable per topic:
a 1-page note in your own words + 10 Anki cards + one "when NOT to use it" line.**

| Wk | Tue topic | Thu topic | DDIA | Must be able to answer |
|----|-----------|-----------|------|------------------------|
| **1** | Networking: HTTP/1.1 vs 2 vs 3, TCP vs UDP, WebSocket vs SSE vs long-poll, gRPC, DNS, CDN | Caching: cache-aside vs write-through vs write-behind vs write-around, LRU/LFU, invalidation, **stampede/thundering herd**, Bloom filters | Ch 1 | "Push or pull for notifications, and why?" · "How do you prevent a cache stampede?" |
| **2** | Storage: SQL vs NoSQL decision tree, ACID vs BASE, B-Tree vs **LSM+SSTable+memtable+compaction**, indexing, WAL, MVCC, optimistic vs pessimistic lock | Sharding: hash vs range vs directory, **consistent hashing + vnodes**, resharding, hot partition, read/write splitting, connection pooling | Ch 3, 6 | "Why is Cassandra write-optimised?" · "How do you reshard without downtime?" |
| **3** | Replication & Consistency: single/multi-leader/leaderless, sync vs async, **CAP + PACELC**, quorum (W+R>N), read-your-writes, eventual vs strong, vector clocks, CRDTs | Consensus & Coordination: leader election, Raft (understand it properly), Paxos (awareness), ZooKeeper/etcd, distributed locks (Redlock + its critique), 2PC vs Saga | Ch 5, 9 | "Explain quorum with N=3,W=2,R=2" · "Why is Redlock controversial?" |
| **4** | Load balancing & Traffic: L4 vs L7, algorithms, health checks, sticky sessions, API gateway, reverse proxy, **rate limiting algorithms (token bucket, leaky bucket, sliding window log/counter)**, circuit breaker, bulkhead, backpressure | Messaging & Streaming: queue vs log, **Kafka internals (partitions, offsets, ISR, consumer groups, rebalancing)**, exactly-once vs at-least-once, idempotency keys, DLQ, ordering guarantees, event sourcing, CQRS, outbox pattern | Ch 7, 8, 11 | "How does Kafka guarantee ordering?" · "Implement exactly-once with at-least-once delivery" |

**Also cover during Phase A (30 min each, spread across the 4 weeks):**
- Observability: metrics vs logs vs traces, RED/USE method, SLI/SLO/SLA, error budgets
- Security: authN vs authZ, OAuth2 flows, JWT (and its pitfalls), TLS handshake, rate limiting abuse
- Deployment: blue-green, canary, feature flags, schema migration (expand-contract)

---

## 5. PHASE B — 24 CASE STUDIES (Weeks 5–20, 2 per week)

**Weekly cadence per case study:**
- **Thu 20:00–21:30** — Case A learn pass (read Alex Xu / Grokking / engineering blog, write the doc)
- **Sun 20:00–21:30** — Case B: 45-min timed performance on Excalidraw + recorded, then write-up

**Every case gets a write-up saved as `Plan/design_notes/<case>.md`** using §2's structure.
Re-reading your OWN write-ups in Week 19–22 is your revision material.

### Tier 1 — The Canon (Weeks 5–12, must-know, appear constantly)

| # | Case | Core lesson to nail | Signature deep-dive |
|---|------|---------------------|---------------------|
| 1 | **URL Shortener** | ID generation, base62, KV store, cache | Counter vs hash vs Snowflake; collision handling |
| 2 | **Distributed Rate Limiter** | Token bucket, Redis atomicity | Distributed counters, sliding window log vs counter |
| 3 | **Distributed Key-Value Store (Dynamo)** | Consistent hashing, quorum, gossip | Vector clocks, Merkle trees, hinted handoff |
| 4 | **Web Crawler** | BFS at scale, politeness, dedup | URL frontier design, Bloom filter dedup, trap detection |
| 5 | **News Feed (Facebook)** | **Fan-out on write vs read** | Celebrity problem → hybrid fan-out; ranking pipeline |
| 6 | **Chat System (WhatsApp/Slack)** | WebSockets, presence, delivery receipts | Message ordering, offline queue, E2E encryption, group fan-out |
| 7 | **Notification System** | Multi-channel, retries, DLQ | Idempotency, rate limits per user, template service, priority queues |
| 8 | **Search Autocomplete / Typeahead** | Trie + top-k, prefix caching | Trie sharding, updating trie without downtime, personalisation |
| 9 | **YouTube / Netflix** | Blob storage, transcoding pipeline, CDN | Adaptive bitrate (HLS/DASH), chunking, upload resumability |
| 10 | **Google Drive / Dropbox** | Chunking, dedup, delta sync | Conflict resolution, metadata service, versioning, cold storage tiering |
| 11 | **Uber / Ride Hailing** | Geo-index (**QuadTree/S2/geohash**), matching | Real-time location ingest, surge pricing, driver-rider state machine |
| 12 | **Google Maps / Proximity Service** | Spatial indexing, routing | Tiling, ETA computation, map data updates |
| 13 | **Ad Click Aggregator** | Stream processing, windowing | Exactly-once counting, late events/watermarks, lambda vs kappa |
| 14 | **Metrics & Monitoring (Datadog)** | Time-series DB, downsampling | Cardinality explosion, rollups, alerting engine |
| 15 | **Payment System** | Idempotency, ledger, reconciliation | Exactly-once money movement, **Saga vs 2PC**, double-entry bookkeeping |
| 16 | **Distributed Job Scheduler** | Leader election, at-least-once execution | Cron at scale, delayed queues, fairness, failure retry semantics |

### Tier 2 — Depth Builders (Weeks 13–20)

| # | Case | Signature deep-dive |
|---|------|---------------------|
| 17 | **S3 / Object Storage** | Erasure coding vs replication, metadata scale, multipart upload |
| 18 | **Ticketmaster / BookMyShow** | Seat locking, inventory race conditions, virtual waiting room |
| 19 | **Distributed Cache (Redis-like)** | Eviction, consistent hashing, cluster mode, hot key mitigation |
| 20 | **Live Streaming (Twitch)** | Low latency (LL-HLS/WebRTC), edge fan-out, chat at scale |
| 21 | **Digital Wallet (Paytm)** | Ledger consistency, double spend, reconciliation, PCI concerns |
| 22 | **Stock Exchange / Order Matching** | In-memory matching engine, determinism, sequencer, ultra-low latency |
| 23 | **Nearby Friends** | Real-time geo pub/sub, privacy, TTL of location |
| 24 | **Collaborative Editor (Google Docs)** | **OT vs CRDT**, presence, conflict-free merges |

### Tier 3 — Stretch (Weeks 21–22, only if ahead)
Twitter/X · Instagram · Airbnb · Zoom · Food Delivery (Swiggy) · Distributed Message Queue from
scratch · Distributed Transaction Coordinator · Feature Flag Service · A/B Testing Platform ·
Autocomplete for code (Copilot) · Recommendation System · Fraud Detection Pipeline

---

## 6. THE "USE YOUR OWN EXPERIENCE" ADVANTAGE

You've shipped **Microservices + Kafka + Redis + MongoDB + MySQL + K8s + AWS** for 5.3 years.
That is gold in this round — most candidates only have book knowledge.

**Prepare 6 "war stories" (one paragraph each, in `06_Trackers.md`):**
1. A production incident you debugged (latency spike / memory leak / cascading failure)
2. A service you designed end-to-end — and what you'd change now
3. A time you chose SQL over NoSQL (or vice-versa) and the trade-off
4. A Kafka consumer-lag / rebalancing / ordering problem you solved
5. A caching decision that backfired (or saved you)
6. A migration you ran with zero downtime

Drop these naturally: *"In production we hit exactly this — we saw consumer lag spike during
rebalances, so we…"* This instantly signals SDE2+ and shifts the interviewer to peer mode.

---

## 7. RESOURCES (fixed list — do not add more)

| Type | Resource | Use |
|------|----------|-----|
| Case studies | **System Design Interview Vol 1 & 2** — Alex Xu | Spine for the 24 cases |
| Depth | **DDIA** — Kleppmann (Ch 1,3,5,6,7,8,9,11) | Phase A only, chapter-targeted |
| Video | **Hello Interview** (YouTube) · **Jordan has no life** · **System Design Fight Club** | Watch AFTER your own attempt, never before |
| Practice | **Hello Interview** guided practice · **excalidraw.com** | Weekly |
| Real-world | Engineering blogs: Uber, Netflix, Discord, Cloudflare, Stripe, Slack | 1 blog/week, 20 min |
| Deep | **Grokking Advanced System Design** | Weeks 13+ for deep-dive round |
| Mock | interviewing.io (ex-FAANG designers) · meetapro.com | 1 design mock/fortnight from Wk 8 |

---

## 8. CHECKPOINTS

| Week | You must be able to |
|------|---------------------|
| 4 | Explain CAP/PACELC, consistent hashing, LSM vs B-Tree, Kafka ordering — out loud, no notes |
| 8 | Deliver 4 canonical designs in 45 min each with estimations |
| 12 | Deliver any of the 16 Tier-1 designs cold. Volunteer 2 deep dives unprompted. |
| 16 | Handle "now scale it 100×" and "what breaks first" without freezing |
| 20 | Design an unseen system using only the framework — the framework carries you, not memory |

> **The final test:** given a system you've never seen, can you produce a defensible design in
> 45 minutes? If yes, you're done. Memorising 24 designs is the *means*, not the goal.

