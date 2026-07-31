# 04 — CORE TECH DEEP DIVE (Java · Spring · Kafka · DB · Cloud)

> **Mission:** Convert 5.3 years of *doing* into 5.3 years of *articulating*.
> Reference: `../Java_MasterSheet.md` (1158 lines) · `../Spring_MasterSheet.md` (1769 lines) —
> **these are lookup references now, not reading material.**
> Slots: **Wed evening (Java/JVM/Concurrency)** + **Fri evening (Spring/Kafka/DB/Cloud)**, 90 min each.

---

## 1. THE METHOD — "Teach it in 5" (use for EVERY topic below)

For each weekly topic, produce exactly 3 artifacts:

1. **A 5-minute explanation** written in your own words as if teaching a junior.
   Rule: if you need the master sheet open to write it, you don't know it yet.
2. **10 Anki cards** — question on front, precise answer on back. Include at least 2
   "why" cards and 1 "when would this bite you in prod" card.
3. **One hands-on artifact** — a 20-line experiment, a benchmark, a `jstack` dump, a config
   change with observed effect. *Interviewers can smell theory-only knowledge instantly.*

**Then say it out loud, on camera, in 5 minutes, with no notes.** That's the pass condition.

---

## 2. WEDNESDAY TRACK — JAVA & JVM DEPTH

| Wk | Topic | Must be able to answer | Hands-on |
|----|-------|------------------------|----------|
| **1** | **JVM architecture & memory** — class loading (delegation model), heap/stack/metaspace/code cache, JIT (C1/C2, tiered, inlining, deopt) | "Walk me through what happens from `java Main` to `main()` executing" · "What is stored where?" | Run with `-XX:+PrintCompilation`, `-Xlog:gc*` |
| **2** | **GC & memory leaks** — Serial/Parallel/CMS/**G1/ZGC/Shenandoah**, generational hypothesis, minor vs major vs full GC, safepoints, reference types (strong/soft/weak/phantom) | "Prod latency spikes every 40 min — how do you debug?" · "How would you fix a memory leak in a static Map?" | Create an OOM, capture heap dump, analyse in **Eclipse MAT / VisualVM** |
| **3** | **Collections internals** — ArrayList growth, **HashMap (buckets, hash spreading, treeify at 8, resize)**, ConcurrentHashMap (CAS + bin locking, no segments since Java 8), TreeMap, LinkedHashMap-as-LRU, fail-fast vs fail-safe iterators | "Why treeify at 8?" · "Why must equals/hashCode be consistent?" · "How does CHM achieve concurrency without a global lock?" | Implement an LRU cache with LinkedHashMap **and** manually with HashMap+DLL |
| **4** | **Concurrency I** — thread lifecycle, `synchronized` vs `ReentrantLock`, `volatile` + **Java Memory Model / happens-before**, CAS & `AtomicX`, ABA problem, deadlock/livelock/starvation, false sharing | "What does volatile guarantee and NOT guarantee?" · "Write a deadlock, then fix it" · "Explain happens-before" | Write a deadlock, capture it with `jstack`, fix with lock ordering |
| **5** | **Concurrency II** — Executor framework, thread pool sizing (`N_cpu × (1 + W/C)`), queue choice, rejection policies, `Future` vs `CompletableFuture` (composition, `thenCombine`, `allOf`, exception handling), ForkJoin, `ThreadLocal` (+leak risk), `CountDownLatch/CyclicBarrier/Semaphore/Phaser` | "Size a pool for an IO-bound service" · "Chain 3 async calls with a timeout and fallback" | Build a `CompletableFuture` pipeline with timeouts + fallback + retry |
| **6** | **Java 8→21 features** — Streams (lazy, short-circuit, **parallel stream pitfalls**), Optional (right/wrong usage), functional interfaces, records, sealed classes, pattern matching, text blocks, **Virtual Threads (Loom)**, structured concurrency | "When is a parallel stream actually slower?" · "Virtual threads vs platform threads vs reactive — when what?" | Benchmark 10k tasks: platform threads vs virtual threads |
| **7** | **Java internals misc** — String pool & interning, immutability, `equals/hashCode` contracts, serialization + `serialVersionUID`, generics erasure + PECS, reflection cost, annotations & processors, exception design | "Design an immutable class with a mutable field" · "Explain PECS with an example" | Write a generic bounded API and break it deliberately |
| **8** | **Performance & profiling** — JMH microbenchmarking, async-profiler / JFR, flame graphs, allocation profiling, escape analysis, common latency killers | "p99 is 10× p50 — what are your top 5 hypotheses?" | Profile a slow method with JFR, produce a flame graph |

**Weeks 9+:** Wednesday becomes **revision + gap-filling** — re-record any 5-min explanation you
can't deliver cleanly. From Week 13, Wednesday flips to **company-specific tech prep**.

---

## 3. FRIDAY TRACK — SPRING · MESSAGING · DATA · CLOUD

| Wk | Topic | Must be able to answer | Hands-on |
|----|-------|------------------------|----------|
| **1** | **Spring Core** — IoC container, BeanFactory vs ApplicationContext, bean lifecycle (full order: instantiate → populate → aware → BPP-before → init → BPP-after → destroy), scopes, `@Autowired` resolution, circular dependency, `BeanPostProcessor` | "What exactly happens on `SpringApplication.run`?" · "How do you fix a circular dependency properly?" | Write a `BeanPostProcessor` + a custom scope |
| **2** | **Spring Boot internals** — auto-configuration (`@EnableAutoConfiguration`, `spring.factories`/`AutoConfiguration.imports`, `@Conditional*`), starters, externalised config precedence, profiles, Actuator, embedded server | "How would you write your own starter?" · "Config precedence order?" | Build a custom starter with `@ConditionalOnProperty` |
| **3** | **Spring AOP & Proxies** — JDK dynamic proxy vs CGLIB, pointcuts, why `@Transactional` fails on self-invocation, why it fails on private methods, order of advices | "Why does calling `this.save()` skip the transaction?" | Prove the self-invocation bug, fix it 2 ways |
| **4** | **Spring Data JPA & transactions** — repository proxies, JPQL vs native, **N+1 problem** (detect + fix with fetch join / `@EntityGraph` / batch size), lazy vs eager, first/second-level cache, dirty checking, propagation (all 7) + isolation levels, optimistic (`@Version`) vs pessimistic locking, pagination on large offsets | "Explain REQUIRES_NEW vs NESTED" · "How do you detect N+1 in prod?" | Enable SQL logging, reproduce N+1, fix it, measure |
| **5** | **Spring Web & Security** — DispatcherServlet flow, filters vs interceptors vs AOP, `@ControllerAdvice`, validation, content negotiation, Security filter chain, **OAuth2 flows**, JWT (validation, refresh, revocation problem), CORS, CSRF | "Draw the request path through the Security filter chain" · "Where do you store refresh tokens and why?" | Secure an endpoint with JWT + custom filter |
| **6** | **Microservices patterns** — service discovery, config server, **circuit breaker (Resilience4j)**, retry + exponential backoff + jitter, bulkhead, timeout budgets, API gateway, **Saga (choreography vs orchestration)**, **outbox pattern**, idempotency keys, distributed tracing, service mesh basics | "How do you keep a DB write and a Kafka publish atomic?" (→ outbox) · "Design a rollback across 3 services" | Implement Resilience4j circuit breaker + fallback |
| **7** | **Kafka deep dive** — topics/partitions/offsets, replication + **ISR + `acks`/`min.insync.replicas`**, leader election, consumer groups + **rebalancing (eager vs cooperative-sticky)**, ordering guarantees, exactly-once (idempotent producer + transactions), retention + compaction, consumer lag, backpressure, DLQ, when Kafka vs RabbitMQ vs SQS | "Guarantee ordering per user across partitions" · "Consumer lag is growing — 6 causes and fixes" · "acks=all + min.insync=2 — what does that buy?" | Run local Kafka: produce/consume, force a rebalance, watch lag |
| **8** | **Databases: MySQL** — B+Tree index internals, clustered vs secondary index, covering index, **EXPLAIN plan reading**, composite index leftmost-prefix rule, isolation levels + phenomena, MVCC in InnoDB, gap locks & deadlocks, replication lag, connection pooling (HikariCP sizing), query optimisation, schema migration with zero downtime | "This query is slow — walk me through your process" · "Why did adding an index make writes slower?" | Create 1M rows, run EXPLAIN before/after indexing |
| **9** | **NoSQL: MongoDB + Redis** — Mongo: document modelling (embed vs reference), indexes, aggregation pipeline, replica sets, sharding, read/write concerns. Redis: data structures (string/hash/list/set/zset/**hyperloglog/bitmap/stream**), persistence (RDB vs AOF), eviction policies, pipelining, Lua atomicity, Cluster + hash slots, distributed lock + **Redlock critique**, pub/sub vs streams | "Model a chat app in Mongo" · "Implement a distributed lock correctly" · "Rate limiter in Redis — which structure?" | Build a Lua-based atomic rate limiter |
| **10** | **Docker & Kubernetes** — image layers, multi-stage builds, JVM in containers (`-XX:MaxRAMPercentage`, cgroup awareness), Pods/Deployments/Services/Ingress/ConfigMap/Secret, liveness vs readiness vs startup probes, HPA, rolling update vs blue-green vs canary, resource requests/limits + **OOMKilled**, StatefulSets, PDB | "Your pod restarts every 5 min — debug it" · "Why did the JVM OOM inside a container with 2GB limit?" | Deploy a Spring Boot app to **kind/minikube** with probes + HPA |
| **11** | **AWS core** — EC2/ASG, ELB (ALB vs NLB), S3 (storage classes, lifecycle, presigned URLs, consistency), RDS vs Aurora vs DynamoDB (**partition key design, GSI/LSI, capacity modes**), SQS vs SNS vs EventBridge vs Kinesis, Lambda (cold starts, concurrency), ECS/EKS, VPC basics, IAM roles, CloudWatch, Route53, ElastiCache | "Design the AWS stack for the system you just designed" · "DynamoDB hot partition — how do you fix it?" | Design + cost-estimate one architecture on paper |
| **12** | **CI/CD & Observability** — Jenkins/GitHub Actions pipeline design, Maven/Gradle lifecycle & dependency resolution, SonarQube quality gates, JUnit5 + Mockito (mock vs spy, argument captors), Testcontainers, contract testing, **metrics vs logs vs traces**, RED/USE, SLI/SLO/error budgets, alert design, on-call runbooks | "Design a deployment pipeline with zero-downtime + rollback" · "What 5 dashboards would you build for your service?" | Write a GitHub Actions pipeline: build → test → scan → containerise → deploy |

**Weeks 13+:** Friday becomes **company-specific + rapid-fire revision**, plus mock deep-dive rounds.

---

## 4. THE 40 QUESTIONS YOU MUST BE ABLE TO ANSWER COLD

Print this. Tick each when you can answer in ≤3 minutes, out loud, no notes.

### Java (10)
- [ ] Walk through the full bean-to-object lifecycle in the JVM: classloading → GC eligibility
- [ ] G1 vs ZGC — when do you pick which, and what's the trade-off?
- [ ] `volatile` vs `synchronized` vs `AtomicInteger` — three-way comparison with a use case each
- [ ] Explain the Java Memory Model's happens-before with a concrete broken example
- [ ] HashMap internals end-to-end, including resize and treeify — and why CHM differs
- [ ] Size a thread pool for (a) CPU-bound, (b) IO-bound with 100ms downstream latency
- [ ] Virtual threads: what problem do they solve, what do they NOT solve?
- [ ] Debug: "service p99 jumped from 50ms to 900ms after a deploy" — your first 5 steps
- [ ] Design an immutable class that contains a `List` and a `Date`
- [ ] Why is `String` immutable, and what does that buy the JVM?

### Spring (8)
- [ ] What happens, in order, during `SpringApplication.run()`?
- [ ] How does `@Transactional` work under the hood, and give 3 ways it silently fails
- [ ] Explain all 7 propagation levels with a real scenario for REQUIRED vs REQUIRES_NEW
- [ ] Detect and fix an N+1 query — three different fixes and their trade-offs
- [ ] Draw the Spring Security filter chain and where you'd insert a custom JWT filter
- [ ] Write a custom auto-configuration + starter — the actual mechanics
- [ ] JDK proxy vs CGLIB — when does Spring choose which, and what breaks?
- [ ] How do you handle a distributed transaction across 3 Spring Boot services?

### Data & Messaging (12)
- [ ] Kafka: guarantee per-key ordering, and what happens when you increase partitions?
- [ ] Kafka: `acks`, ISR, `min.insync.replicas` — construct a config for "never lose a message"
- [ ] Kafka: consumer lag growing — enumerate 6 causes and their fixes
- [ ] Kafka vs RabbitMQ vs SQS vs Kinesis — pick for 3 different scenarios
- [ ] Exactly-once semantics: is it real? How do you actually achieve effective-once?
- [ ] The outbox pattern — what problem, what implementation, what's the catch?
- [ ] MySQL: read an EXPLAIN plan and explain leftmost-prefix indexing
- [ ] MySQL isolation levels: which phenomena does each prevent? Which does InnoDB default to?
- [ ] Design a Mongo schema for a feed; when would you denormalise, when reference?
- [ ] Redis: implement a correct distributed lock; explain Redlock's criticisms
- [ ] Redis: what happens when memory is full? Walk through eviction policies
- [ ] Sharding: pick a shard key for a chat app, and explain how you'd reshard live

### Cloud & Ops (10)
- [ ] Your pod is `OOMKilled` — full debugging path, including JVM container flags
- [ ] Liveness vs readiness vs startup probe — what breaks if you get them wrong?
- [ ] Design a zero-downtime deployment with automatic rollback
- [ ] DynamoDB: design a partition key for a time-series workload; fix a hot partition
- [ ] ALB vs NLB vs API Gateway — pick for 3 scenarios
- [ ] Blue-green vs canary — cost, risk, and when you'd choose each
- [ ] Zero-downtime schema migration (adding a NOT NULL column to a 500M-row table)
- [ ] Define SLI/SLO/error budget for your current service — with real numbers
- [ ] What 5 alerts would you page on at 3 AM, and what would you never page on?
- [ ] Multi-region: active-active vs active-passive — data consistency implications

---

## 5. YOUR PRODUCTION STORIES (the SDE2 multiplier)

Prepare **10 concrete stories from your 5.3 years**, one paragraph each, in `06_Trackers.md`.
Each must have: *context → problem → what you did → measurable outcome → what you'd do differently*.

1. A performance problem you diagnosed and fixed (with numbers: p99 X→Y ms)
2. A production incident + root cause + the permanent fix
3. A design decision you made and later regretted
4. A Kafka/messaging problem (lag, ordering, duplicates, rebalance storm)
5. A database problem (slow query, deadlock, migration, replication lag)
6. A time you disagreed with a senior/architect and how it resolved
7. A time you mentored or unblocked someone
8. A time you shipped under a hard deadline and what you traded off
9. A time you pushed back on scope or said no
10. Something you built that you're genuinely proud of

These serve **three rounds at once**: behavioral, system design ("in prod we saw…"), and the
deep-dive tech round. Highest-ROI writing in this entire plan. **Do 1 per week from Week 1.**

---

## 6. WEEKLY RAPID-FIRE DRILL (Fri, last 15 min)

Set a 15-minute timer. Answer 5 random questions from §4 out loud, on camera.
Any question you fumble → **it becomes next week's Anki priority + gets re-drilled Wednesday.**
By Week 12 all 40 must be green.

