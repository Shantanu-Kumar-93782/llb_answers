# System Design (HLD) — Complete Topic List for FAANG (5 YOE)

---

## 1. FUNDAMENTALS & BUILDING BLOCKS

### 1.1 Networking Basics
- Client-Server Model
- HTTP/HTTPS, HTTP/2, HTTP/3 (QUIC)
- TCP vs UDP
- WebSockets & Server-Sent Events (SSE)
- Long Polling vs Short Polling
- gRPC & Protocol Buffers
- DNS & DNS Resolution
- CDN (Content Delivery Network)
- REST vs GraphQL vs RPC
- API Gateway
- Reverse Proxy vs Forward Proxy
- Service Discovery (Client-Side vs Server-Side)
- API Versioning Strategies
- Webhooks & Callback Patterns
- Content-Based Routing
- Sticky Sessions / Session Affinity

### 1.2 Storage & Data
- SQL vs NoSQL (when to use what)
- ACID Properties
- BASE Properties
- CAP Theorem
- PACELC Theorem
- Data Partitioning / Sharding (horizontal, vertical, directory-based, hash-based, range-based)
- Data Replication (single-leader, multi-leader, leaderless)
- Consistent Hashing (with Virtual Nodes)
- Database Indexing (B-Tree, LSM Tree, Hash Index, Bitmap Index)
- Denormalization vs Normalization
- Data Lakes vs Data Warehouses
- Time-Series Databases (InfluxDB, TimescaleDB)
- Graph Databases (Neo4j, Amazon Neptune)
- Blob/Object Storage (S3)
- Column-Oriented vs Row-Oriented Databases
- Database Federation
- Write-Ahead Log (WAL)
- SSTable & Memtable (LSM Tree internals)
- Compaction Strategies (Size-Tiered, Leveled)
- MVCC (Multi-Version Concurrency Control)
- Optimistic vs Pessimistic Locking
- Hot/Cold Data Storage & Data Tiering
- Tombstones in Distributed Systems
- Database Connection Pooling
- Read/Write Splitting
- Hot Key / Hot Partition Problem
- Multi-Tenancy (Shared DB vs DB per Tenant)

### 1.3 Caching
- Cache-Aside (Lazy Loading)
- Read-Through / Write-Through
- Write-Behind (Write-Back)
- Write-Around
- Cache Eviction Policies (LRU, LFU, FIFO, TTL)
- Distributed Cache (Redis, Memcached)
- CDN Caching
- Browser/Client-Side Caching
- Cache Invalidation Strategies
- Cache Stampede / Thundering Herd
- Bloom Filters (probabilistic caching)

### 1.4 Message Queues & Streaming
- Message Queues (RabbitMQ, SQS)
- Event Streaming (Kafka, Pulsar, Kinesis)
- Pub/Sub Model
- Point-to-Point Model
- Dead Letter Queues
- Message Ordering & Exactly-Once Delivery
- Backpressure Handling
- Event Sourcing
- CQRS (Command Query Responsibility Segregation)

### 1.5 Load Balancing
- Layer 4 vs Layer 7 Load Balancing
- Algorithms: Round Robin, Weighted Round Robin, Least Connections, IP Hash, Consistent Hashing
- Health Checks
- Global Server Load Balancing (GSLB)
- Service Mesh (Istio, Envoy)

### 1.6 Data Structures for System Design
- Bloom Filters (membership testing)
- HyperLogLog (cardinality estimation)
- Count-Min Sketch (frequency estimation)
- Trie / Prefix Tree (autocomplete, IP routing)
- Quadtree / R-Tree (spatial indexing)
- Geohash / S2 Geometry / H3 (geo-proximity)
- Merkle Trees (data integrity verification)
- Skip Lists (ordered data, used in Redis)
- Inverted Index (search engines)
- B+ Trees (database indexing)
- Consistent Hashing Ring

---

## 2. SCALABILITY & PERFORMANCE

### 2.1 Scaling Patterns
- Vertical Scaling vs Horizontal Scaling
- Stateless vs Stateful Services
- Database Read Replicas
- Sharding Strategies
- Auto-Scaling
- Connection Pooling
- Microservices vs Monolith vs Modular Monolith

### 2.2 Performance Optimization
- Latency vs Throughput
- P99/P95/P50 Latency
- Tail Latency Amplification
- Batch Processing vs Stream Processing
- Lazy Loading / Eager Loading
- Pagination (Offset vs Cursor-based)
- Data Compression
- Async Processing
- Pre-computation & Materialized Views

---

## 3. RELIABILITY & FAULT TOLERANCE

### 3.1 Availability & Redundancy
- SLA, SLO, SLI
- Availability (99.9%, 99.99% — nines)
- Active-Passive vs Active-Active
- Failover Strategies
- Redundancy (data, service, geographic)
- Disaster Recovery (RPO, RTO)
- Multi-Region / Multi-AZ Deployment

### 3.2 Fault Tolerance Patterns
- Circuit Breaker Pattern
- Bulkhead Pattern
- Retry with Exponential Backoff & Jitter
- Timeouts & Deadline Propagation
- Graceful Degradation
- Rate Limiting & Throttling (Token Bucket, Leaky Bucket, Sliding Window, Fixed Window)
- Idempotency & Idempotency Keys
- Saga Pattern (Choreography vs Orchestration)
- Two-Phase Commit (2PC)
- Three-Phase Commit (3PC)
- Heartbeat / Health Checks
- Leader Election (Bully, Raft, Paxos, ZAB)
- Sloppy Quorum & Hinted Handoff
- Read Repair & Anti-Entropy Repair
- Phi Accrual Failure Detector
- Lease-Based Locking
- Fencing Tokens
- Distributed Snapshots (Chandy-Lamport Algorithm)
- Poison Pill / Dead Letter Queue Handling
- Blue-Green Deployment
- Canary Deployment / Rolling Updates
- Feature Flags / Dark Launching
- Shadow Traffic / Dark Traffic Testing
- Chaos Engineering (Netflix Simian Army)

---

## 4. CONSISTENCY & CONSENSUS

- Strong Consistency vs Eventual Consistency
- Linearizability vs Serializability
- Quorum-based Reads/Writes (R + W > N)
- Vector Clocks & Lamport Timestamps
- Conflict Resolution (LWW, CRDTs)
- Consensus Algorithms: Paxos, Raft, ZAB
- Gossip Protocol
- Split-Brain Problem & Fencing Tokens
- Read-Your-Own-Writes Consistency
- Causal Consistency
- Session Consistency

---

## 5. SECURITY

- Authentication vs Authorization
- OAuth 2.0 / OpenID Connect
- JWT & Token-Based Auth
- API Key Management
- Role-Based Access Control (RBAC)
- Rate Limiting for DDoS Protection
- Data Encryption (at rest, in transit, end-to-end)
- TLS/SSL
- CORS
- Input Validation & SQL Injection Prevention
- Secrets Management (Vault)

---

## 6. OBSERVABILITY & MONITORING

- Logging (Structured Logging, ELK Stack)
- Metrics (Prometheus, Grafana, CloudWatch)
- Distributed Tracing (Jaeger, Zipkin, OpenTelemetry)
- Alerting & On-Call
- Dashboards & SLO Monitoring
- Log Aggregation
- Health Check Endpoints
- Chaos Engineering (fault injection)

---

## 7. SYSTEM DESIGN PATTERNS & ARCHITECTURES

### 7.1 Architecture Styles
- Monolithic Architecture
- Microservices Architecture
- Service-Oriented Architecture (SOA)
- Event-Driven Architecture
- Serverless / FaaS
- Peer-to-Peer Architecture
- Lambda Architecture (batch + speed layers)
- Kappa Architecture (streaming only)

### 7.2 Design Patterns
- API Gateway Pattern
- Backend for Frontend (BFF)
- Strangler Fig Pattern
- Sidecar Pattern
- Ambassador Pattern
- Anti-Corruption Layer
- Database per Service
- Shared Database
- Outbox Pattern (Transactional Outbox)
- Change Data Capture (CDC) — Debezium
- Fan-Out / Fan-In (Fan-Out on Write vs Fan-Out on Read)
- Scatter-Gather
- Bulkhead
- Competing Consumers
- Saga Pattern (Choreography vs Orchestration) ★
- Two-Phase Commit (2PC) ★
- Transactional Inbox/Outbox ★
- Dual Write Problem & Solutions ★
- Event-Carried State Transfer
- Polling Publisher
- Transaction Log Tailing
- Choreography vs Orchestration (in-depth)
- Cell-Based Architecture (Blast Radius Reduction)
- Shard-per-Tenant Pattern
- Throttle Pattern
- Queue-Based Load Leveling
- Priority Queue Pattern
- Claim Check Pattern (large message handling)
- Pipes and Filters Pattern
- Gateway Aggregation / Gateway Offloading
- Valet Key Pattern (direct client-to-storage access)
- Materialized View Pattern
- Index Table Pattern
- CQRS + Event Sourcing (combined) ★

---

## 8. DATA PROCESSING & ANALYTICS

- Batch Processing (MapReduce, Spark)
- Stream Processing (Kafka Streams, Flink, Storm)
- ETL Pipelines
- Data Pipelines & Workflow Orchestration (Airflow)
- Real-Time Analytics
- OLAP vs OLTP
- Data Warehousing (Redshift, BigQuery, Snowflake)
- Hadoop Ecosystem Basics
- Search Engines (Elasticsearch, Solr) — Inverted Index

---

## 9. UNIQUE ID GENERATION & COORDINATION

- UUID
- Snowflake ID (Twitter)
- Database Auto-Increment
- ULID
- Zookeeper / etcd for Coordination
- Distributed Locking (Redlock)

### 9.1 Distributed Transactions (Deep Dive) ★
- Two-Phase Commit (2PC) — coordinator, participant, prepare/commit
- Three-Phase Commit (3PC) — added pre-commit phase
- Saga Pattern — long-lived transactions with compensating actions
  - Choreography-based Saga (event-driven)
  - Orchestration-based Saga (central coordinator)
  - Compensating Transactions
  - Semantic Locks / Countermeasures
- TCC (Try-Confirm-Cancel) Pattern
- Outbox Pattern for Reliable Event Publishing
- Dual Write Problem & Why It's Dangerous
- Idempotent Consumer Pattern
- At-Least-Once vs At-Most-Once vs Exactly-Once Delivery

---

## 10. ESTIMATION & MATH (Back-of-the-Envelope)

- QPS / TPS Estimation
- Storage Estimation
- Bandwidth Estimation
- Memory Estimation
- Number of Servers Estimation
- Powers of 2 Table
- Latency Numbers Every Programmer Should Know
- Read-to-Write Ratio Analysis

---

## 11. CLASSIC SYSTEM DESIGN PROBLEMS (Must-Do)

### 11.1 Social / Feed
- Design Twitter / X
- Design Facebook News Feed
- Design Instagram
- Design Reddit
- Design TikTok (short-video feed)

### 11.2 Messaging / Communication
- Design WhatsApp / Facebook Messenger
- Design Slack
- Design a Notification System (Push, SMS, Email)
- Design Email Service (Gmail)

### 11.3 Storage / File Systems
- Design Google Drive / Dropbox
- Design a Distributed File System (GFS)
- Design an Image/Video Upload & Processing Service
- Design a Key-Value Store (DynamoDB)
- Design a Distributed Cache (Redis)
- Design an Object Storage (S3)

### 11.4 Search & Discovery
- Design Google Search / Web Crawler
- Design Typeahead / Autocomplete
- Design a Search Engine (Elasticsearch)
- Design Yelp / Nearby Places (Proximity Service)

### 11.5 Video / Streaming
- Design YouTube
- Design Netflix
- Design a Live Streaming Service (Twitch)
- Design a Video Transcoding Pipeline

### 11.6 E-Commerce / Payments
- Design Amazon / E-commerce Platform
- Design a Payment System (Stripe)
- Design an Online Ticketing System (BookMyShow)
- Design a Hotel Booking System (Airbnb)
- Design a Food Delivery System (Uber Eats / DoorDash)

### 11.7 Infrastructure / Platform
- Design a URL Shortener (TinyURL / Bit.ly)
- Design Pastebin
- Design a Rate Limiter
- Design a Load Balancer
- Design a Task Scheduler / Job Queue
- Design a Distributed Message Queue (Kafka)
- Design a Logging / Monitoring System
- Design a Metrics Collection System
- Design a Distributed Locking Service
- Design an API Rate Limiter
- Design a CDN
- Design DNS

### 11.8 Ride-Sharing / Location
- Design Uber / Lyft
- Design Google Maps
- Design a Location-Based Service

### 11.9 Gaming / Real-Time
- Design an Online Multiplayer Game (Leaderboard)
- Design a Real-Time Collaborative Editor (Google Docs)
- Design a Voting / Polling System

### 11.10 Auth / Identity
- Design a Single Sign-On (SSO) System
- Design an OAuth / Authentication Service
- Design a Permission System (RBAC)

### 11.11 Miscellaneous
- Design a Ticketing System (JIRA)
- Design a Calendar System (Google Calendar)
- Design a Parking Lot System (with scale)
- Design an Ad Click Aggregation System
- Design a Stock Exchange / Trading Platform
- Design a Coupon / Voucher System
- Design a Content Moderation System
- Design a Fraud Detection System
- Design a Recommendation Engine
- Design a Web Analytics System (Google Analytics)
- Design a Code Deployment System (CI/CD)

### 11.12 Recently Asked at FAANG (2024-2026 Trends) ★
- Design a Distributed Rate Limiter (across multiple data centers)
- Design a Feature Flag System (LaunchDarkly)
- Design a Privacy/GDPR Compliance System (data deletion at scale)
- Design a Multi-Region Active-Active Database
- Design a Real-Time Fraud Detection System
- Design an ML Model Serving Platform (A/B testing, canary)
- Design a Distributed Workflow Engine (Temporal / Cadence)
- Design a Change Data Capture Pipeline
- Design a Service Mesh
- Design a Secrets Management System
- Design a Distributed Configuration Service
- Design an Event-Driven Microservices Platform
- Design a Data Pipeline with Exactly-Once Semantics
- Design a Global Unique ID Generator at Scale

---

## 12. HOW TO STRUCTURE YOUR ANSWER (Framework)

1. **Clarify Requirements** — Functional & Non-Functional
2. **Back-of-the-Envelope Estimation** — QPS, Storage, Bandwidth
3. **High-Level Design** — API Design, Data Flow, Component Diagram
4. **Data Model** — Schema, DB choice
5. **Deep Dive** — Core components, trade-offs
6. **Bottlenecks & Scaling** — Identify & resolve
7. **Monitoring & Observability**
8. **Trade-offs Discussion**

---

## 13. KEY TECHNOLOGIES TO KNOW (Not in-depth, but know when to use)

| Category | Technologies |
|---|---|
| Databases | MySQL, PostgreSQL, MongoDB, Cassandra, DynamoDB, CockroachDB, TiDB, ScyllaDB |
| Cache | Redis, Memcached, Caffeine (local) |
| Queue/Stream | Kafka, RabbitMQ, SQS, Pulsar, Kinesis, NATS |
| Search | Elasticsearch, Solr, Meilisearch |
| Object Storage | S3, GCS, Azure Blob, MinIO |
| CDN | CloudFront, Akamai, Cloudflare, Fastly |
| Coordination | ZooKeeper, etcd, Consul |
| Load Balancer | Nginx, HAProxy, AWS ALB/NLB, Envoy |
| Container/Orchestration | Docker, Kubernetes, ECS |
| Monitoring | Prometheus, Grafana, Datadog, ELK, Jaeger, OpenTelemetry |
| API Gateway | Kong, AWS API Gateway, Zuul, Envoy |
| Workflow | Temporal, Cadence, Airflow, Step Functions |
| CDC | Debezium, Maxwell, AWS DMS |
| Service Mesh | Istio, Linkerd, Envoy |
| Feature Flags | LaunchDarkly, Unleash |

---

## 14. COMMON TRADE-OFFS TO DISCUSS ★ (Interviewers LOVE this)

| Trade-off | When to Discuss |
|---|---|
| Consistency vs Availability | Any distributed system |
| Latency vs Throughput | Streaming, real-time systems |
| SQL vs NoSQL | Data model discussion |
| Push vs Pull | Feed, notifications |
| Fan-Out on Write vs Fan-Out on Read | Twitter/Feed design |
| Sync vs Async Processing | Payment, order processing |
| Strong vs Eventual Consistency | Chat, social media, banking |
| Monolith vs Microservices | Architecture discussion |
| Normalization vs Denormalization | Schema design |
| Polling vs WebSocket vs SSE | Real-time communication |
| Batch vs Stream Processing | Analytics, data pipelines |
| CP vs AP Systems | Database selection |
| Pre-computation vs On-the-fly | Leaderboards, analytics |
| Replication vs Sharding | Scaling databases |
| Pessimistic vs Optimistic Locking | Concurrency control |
| 2PC vs Saga | Distributed transactions |
| In-memory vs Disk-based | Cache/DB selection |
| HTTP vs gRPC | Inter-service communication |
| Centralized vs Decentralized | Architecture decisions |

---

> **Verdict: This has MASSIVE potential.** System design is THE differentiator for L5/E5+ roles at FAANG. A 5-YOE candidate is *expected* to ace HLD. Mastering all the above topics puts you in the top 5% of candidates. This is not optional — it's the single highest-ROI investment for senior engineering interviews.

**Recommended Study Order:**
1. Fundamentals (Sections 1-6) — 2-3 weeks
2. Patterns & Architectures (Section 7-9) — 1-2 weeks
3. Distributed Transactions Deep Dive (Section 9.1) — 3-4 days
4. Estimation (Section 10) — 2-3 days
5. Trade-offs (Section 14) — review continuously
6. Practice Problems (Section 11) — 4-6 weeks (2-3 problems/week)
7. Mock Interviews — ongoing

**Total Prep Time: ~10-12 weeks (2-3 hrs/day)**
