# Low-Level Design (LLD) + Java + Spring — Complete FAANG Interview Guide (5 YOE)

> Every OOP concept, design pattern, SOLID principle, Java deep-dive, Spring internals,
> and classic LLD problems you need to crack any FAANG LLD/Machine Coding round.

---

# PART A: OBJECT-ORIENTED DESIGN & PRINCIPLES

---

## I. OOP FUNDAMENTALS

### 1.1 Four Pillars of OOP
- **Encapsulation** — Hide internal state, expose via methods. Use private fields + public getters/setters.
- **Abstraction** — Hide complexity, expose only what's necessary. Abstract classes vs Interfaces.
- **Inheritance** — IS-A relationship. When to use vs when to prefer composition.
- **Polymorphism**
  - Compile-time (Method Overloading)
  - Runtime (Method Overriding + dynamic dispatch)
  - Covariant return types

### 1.2 Association Relationships
- **Association** — A uses B (has a reference)
- **Aggregation** — A has B, but B can exist independently (e.g., Department has Professors)
- **Composition** — A owns B, B cannot exist without A (e.g., House has Rooms)
- **Dependency** — A temporarily uses B (method parameter)
- **IS-A vs HAS-A** — Inheritance vs Composition

### 1.3 Abstract Class vs Interface
| Feature | Abstract Class | Interface |
|---|---|---|
| Methods | Abstract + concrete | Abstract + default (Java 8+) + static |
| Fields | Instance variables | Only `public static final` |
| Constructor | Yes | No |
| Inheritance | Single | Multiple |
| **When to use** | Shared state + partial impl | Pure contract / capability |

### 1.4 Coupling & Cohesion
- **Tight Coupling** — Bad. Classes depend on concrete implementations.
- **Loose Coupling** — Good. Depend on abstractions (interfaces). Use DI.
- **High Cohesion** — Good. Class does one focused thing.
- **Low Cohesion** — Bad. God class doing everything.

---

## II. SOLID PRINCIPLES (Must explain with examples in interview)

### S — Single Responsibility Principle
- A class should have only ONE reason to change.
- **Violation example:** `UserService` handles registration + email sending + logging.
- **Fix:** Separate into `UserService`, `EmailService`, `AuditLogger`.

### O — Open/Closed Principle
- Open for extension, closed for modification.
- **Violation example:** `if-else` chain for every new payment type.
- **Fix:** Use Strategy pattern — `PaymentStrategy` interface + `CreditCardPayment`, `UPIPayment`, etc.

### L — Liskov Substitution Principle
- Subtypes must be substitutable for their base types without breaking behavior.
- **Violation example:** `Square extends Rectangle` where `setWidth()` breaks invariants.
- **Fix:** Don't force IS-A when behavior differs. Use composition or separate hierarchies.

### I — Interface Segregation Principle
- Clients should not be forced to depend on interfaces they don't use.
- **Violation example:** `Animal` interface with `fly()`, `swim()`, `walk()` — penguins can't fly.
- **Fix:** Split into `Flyable`, `Swimmable`, `Walkable`.

### D — Dependency Inversion Principle
- High-level modules should not depend on low-level modules. Both should depend on abstractions.
- **Violation example:** `OrderService` directly creates `MySQLRepository`.
- **Fix:** `OrderService` depends on `OrderRepository` interface. Inject implementation via DI.

---

## III. DESIGN PATTERNS (Gang of Four + Modern)

### 3.1 CREATIONAL PATTERNS

#### Singleton
- **What:** One instance globally. Private constructor, static method.
- **When:** Logger, Configuration, Connection Pool, Cache.
- **Java:** Enum singleton (best), double-checked locking, Bill Pugh (inner static class).
- **Thread safety:** `volatile` + `synchronized` for DCL, or use enum.
- **Interview trap:** "How to break singleton?" — Reflection, Serialization, Cloning. How to prevent each.

#### Factory Method
- **What:** Subclass decides which class to instantiate. Returns interface type.
- **When:** Object creation depends on input/config. `Document.createPage()` — `ResumeDocument` creates `ResumePage`.
- **Signals:** "Create objects without exposing creation logic to client."

#### Abstract Factory
- **What:** Factory of factories. Create families of related objects.
- **When:** Cross-platform UI — `WindowsFactory` creates `WindowsButton` + `WindowsCheckbox`. `MacFactory` creates `MacButton` + `MacCheckbox`.
- **Signals:** "Families of related objects", "platform-specific."

#### Builder
- **What:** Step-by-step construction of complex objects. Fluent API.
- **When:** Object with many optional parameters, immutable objects.
- **Java:** Lombok `@Builder`, or manual static inner class builder.
- **Signals:** "Too many constructor parameters", "telescoping constructor."

#### Prototype
- **What:** Clone existing objects instead of creating from scratch.
- **When:** Object creation is expensive. `Cloneable` interface.
- **Deep vs Shallow copy** — critical interview topic.

---

### 3.2 STRUCTURAL PATTERNS

#### Adapter
- **What:** Convert interface of a class into another interface clients expect.
- **When:** Integrate third-party library with incompatible interface. `XMLToJSONAdapter`.
- **Class Adapter** (inheritance) vs **Object Adapter** (composition — preferred).

#### Bridge
- **What:** Separate abstraction from implementation so both can vary independently.
- **When:** Shape (Circle, Square) × Color (Red, Blue) — avoid class explosion.
- **Signals:** "Two independent dimensions of variation."

#### Composite
- **What:** Tree structure where individual objects and compositions are treated uniformly.
- **When:** File system (File + Directory), UI components, organization hierarchy.
- **Signals:** "Part-whole hierarchy", "tree structure", "uniform treatment."

#### Decorator
- **What:** Add behavior dynamically by wrapping objects. Each decorator adds one responsibility.
- **When:** Java I/O Streams (`BufferedInputStream(FileInputStream(...))`), pizza toppings, notification channels.
- **Signals:** "Add features dynamically", "combine behaviors", "avoid subclass explosion."

#### Facade
- **What:** Simplified interface to a complex subsystem.
- **When:** `OrderFacade` hides `InventoryService`, `PaymentService`, `ShippingService`.
- **Signals:** "Simple interface to complex system."

#### Flyweight
- **What:** Share common state across many objects to save memory.
- **When:** String pool, integer cache (-128 to 127), game characters with shared sprites.
- **Intrinsic state** (shared) vs **Extrinsic state** (unique per instance).

#### Proxy
- **What:** Placeholder that controls access to another object.
- **Types:** Virtual (lazy init), Protection (access control), Remote (RMI), Caching, Logging.
- **When:** Lazy loading of heavy objects, access control, Spring AOP proxies.

---

### 3.3 BEHAVIORAL PATTERNS

#### Strategy
- **What:** Define a family of algorithms, encapsulate each, make them interchangeable.
- **When:** Payment methods, sorting algorithms, compression strategies, routing algorithms.
- **Java:** Functional interfaces + lambdas make this very clean.
- **Signals:** "Switch between algorithms at runtime."

#### Observer (Pub/Sub)
- **What:** One-to-many dependency. When subject changes, all observers are notified.
- **When:** Event systems, stock price listeners, notification systems, MVC.
- **Java:** `PropertyChangeListener`, custom `EventBus`, Spring `ApplicationEvent`.
- **Signals:** "Notify all subscribers", "event-driven."

#### Command
- **What:** Encapsulate request as an object. Supports undo, redo, queuing.
- **When:** Text editor undo/redo, task queues, transaction rollback, remote control.
- **Signals:** "Undo/redo", "queue commands", "log operations."

#### State
- **What:** Object behavior changes based on internal state. State transitions are explicit.
- **When:** Vending machine, order status (Placed→Confirmed→Shipped→Delivered), traffic light.
- **Signals:** "Behavior changes based on state", "state machine."
- **vs Strategy:** Strategy is chosen by client; State transitions are internal.

#### Template Method
- **What:** Define skeleton of algorithm in base class, let subclasses override specific steps.
- **When:** Data parsers (CSV, XML, JSON share parse-validate-process flow), game turn sequence.
- **Signals:** "Same algorithm structure, different details."

#### Chain of Responsibility
- **What:** Pass request along a chain of handlers. Each handler decides to process or pass on.
- **When:** Middleware/filters, logging levels, approval workflows, servlet filters, Spring Security filter chain.
- **Signals:** "Pipeline", "chain of handlers", "approval chain."

#### Iterator
- **What:** Access elements sequentially without exposing underlying structure.
- **When:** Custom collection traversal. Java `Iterable<T>` + `Iterator<T>`.
- **Fail-fast vs Fail-safe iterators** — `ConcurrentModificationException`.

#### Mediator
- **What:** Centralize complex communications between objects.
- **When:** Chat room (users don't talk directly, go through room), ATC for airplanes.
- **Signals:** "Reduce direct dependencies between objects."

#### Memento
- **What:** Capture and restore an object's internal state.
- **When:** Undo mechanism, game save/load, transaction rollback.
- **Signals:** "Save and restore state", "snapshot."

#### Visitor
- **What:** Add new operations to existing class hierarchy without modifying classes.
- **When:** Compiler AST traversal, tax calculation on different item types.
- **Signals:** "Operations on heterogeneous objects", "double dispatch."

#### Null Object
- **What:** Provide a no-op default instead of null checks everywhere.
- **When:** Default strategy, guest user with no permissions.
- **Signals:** "Avoid null checks", "default behavior."

---

### 3.4 MODERN / ENTERPRISE PATTERNS ★

#### Repository Pattern
- **What:** Abstract data access behind a collection-like interface.
- **When:** Always. `UserRepository.findById()`, `save()`, `delete()`.

#### DTO (Data Transfer Object)
- **What:** Simple object to transfer data between layers. No logic.
- **When:** API request/response objects, avoid exposing entities.

#### DAO (Data Access Object)
- **What:** Encapsulate all DB access. DAO = low-level CRUD, Repository = domain-oriented.

#### Service Layer Pattern
- **What:** Business logic in service classes, not in controllers or repositories.

#### Unit of Work
- **What:** Track changes to objects and commit them as a single transaction.
- **When:** JPA/Hibernate `EntityManager` is a Unit of Work.

#### Specification Pattern
- **What:** Combine business rules as composable predicates.
- **When:** Complex filtering — `PremiumUser AND ActiveUser AND AgeAbove18`.
- **Java:** Spring Data `Specification<T>`.

#### Event Sourcing
- **What:** Store state as sequence of events instead of current state.
- **When:** Audit trail, financial ledger, order history.

#### CQRS
- **What:** Separate read model from write model.
- **When:** Read-heavy systems with different read/write schemas.

---

## IV. UML DIAGRAMS (Know how to draw in interviews)

### 4.1 Class Diagram
- Classes, attributes, methods
- Relationships: Association (→), Aggregation (◇→), Composition (◆→), Inheritance (△), Interface Implementation (△ dashed)
- Multiplicity: 1..1, 1..*, 0..*, 0..1

### 4.2 Sequence Diagram
- Object interactions over time
- Synchronous vs Asynchronous calls
- Return messages
- Loops, conditionals (alt/opt fragments)

### 4.3 State Diagram
- States, transitions, events, guards
- Used for: Order lifecycle, vending machine, elevator

### 4.4 Activity Diagram
- Flowchart-like, parallel flows (fork/join)
- Used for: Business process, checkout flow

---

## V. KEY DESIGN PRINCIPLES BEYOND SOLID

### 5.1 DRY — Don't Repeat Yourself
### 5.2 KISS — Keep It Simple, Stupid
### 5.3 YAGNI — You Aren't Gonna Need It
### 5.4 Composition over Inheritance
- Prefer HAS-A over IS-A. More flexible, avoids fragile base class problem.
### 5.5 Program to an Interface, not an Implementation
### 5.6 Favor Immutability
- Immutable objects are thread-safe, cache-friendly, easier to reason about.
### 5.7 Law of Demeter (Principle of Least Knowledge)
- Don't chain: `a.getB().getC().doSomething()` → train wreck.
### 5.8 Tell, Don't Ask
- Tell objects what to do, don't ask for data and do it yourself.
### 5.9 Separation of Concerns

---

# PART B: JAVA DEEP DIVE

---

## VI. JAVA CORE CONCEPTS

### 6.1 JVM Architecture
- ClassLoader → Bytecode Verifier → JIT Compiler → Execution Engine
- **Memory Areas:** Heap, Stack, Method Area (Metaspace), PC Register, Native Method Stack
- **Class Loading:** Bootstrap → Extension → Application ClassLoader
- **JIT Compilation:** HotSpot, C1 (client), C2 (server) compilers
- **Garbage Collection Roots:** Local vars, static fields, active threads, JNI refs

### 6.2 Memory Model & Garbage Collection
- **Young Gen** (Eden + S0 + S1) → Minor GC
- **Old Gen** (Tenured) → Major GC / Full GC
- **Metaspace** (replaced PermGen in Java 8)
- **GC Algorithms:**
  - Serial GC — single-threaded, small apps
  - Parallel GC — multi-threaded, throughput-focused
  - G1 GC — region-based, default since Java 9, low-pause
  - ZGC — sub-millisecond pauses, Java 11+
  - Shenandoah — concurrent compaction
- **GC Tuning:** `-Xms`, `-Xmx`, `-XX:+UseG1GC`, `-XX:MaxGCPauseMillis`
- **Memory Leaks:** Unclosed resources, static collections, listener not deregistered, `ThreadLocal` not removed

### 6.3 Java Keywords Deep Dive
- **`final`** — variable (constant), method (no override), class (no extend)
- **`static`** — class-level, shared across instances, static block, static inner class
- **`volatile`** — visibility guarantee, no caching, happens-before, no atomicity
- **`transient`** — skip during serialization
- **`synchronized`** — monitor lock, method or block level
- **`native`** — JNI, implemented in C/C++
- **`strictfp`** — consistent floating-point across platforms

### 6.4 String Internals
- **String Pool** (intern pool) — literals go here, `new String()` goes to heap
- **Immutability** — why? Thread-safe, hashcode caching, security, string pool
- **`String` vs `StringBuilder` vs `StringBuffer`**
  - `String` — immutable
  - `StringBuilder` — mutable, NOT thread-safe, fast
  - `StringBuffer` — mutable, thread-safe (synchronized), slow
- **String concatenation:** Compiler converts `+` to `StringBuilder.append()` (but not in loops!)

### 6.5 Exceptions
- **Checked** (compile-time): `IOException`, `SQLException` — must handle/declare
- **Unchecked** (runtime): `NullPointerException`, `ArrayIndexOutOfBoundsException`
- **Error:** `OutOfMemoryError`, `StackOverflowError` — don't catch
- **Custom exceptions:** Extend `Exception` (checked) or `RuntimeException` (unchecked)
- **try-with-resources** — `AutoCloseable`, guaranteed cleanup
- **Exception chaining** — `throw new ServiceException("msg", cause)`
- **Best practices:** Don't catch `Exception`, don't use for control flow, always log root cause

### 6.6 Generics
- **Type Erasure** — generics removed at compile-time, replaced with `Object`
- **Bounded Types:** `<T extends Comparable<T>>`, `<? super Integer>`
- **PECS:** Producer Extends, Consumer Super
  - `List<? extends Number>` — read from (producer)
  - `List<? super Integer>` — write to (consumer)
- **Type inference:** Diamond operator `<>`, `var` (Java 10+)
- **Cannot:** `new T()`, `new T[]`, `instanceof T`, primitive generics

### 6.7 Equals, HashCode, Comparable, Comparator
- **Contract:** If `a.equals(b)` then `a.hashCode() == b.hashCode()` (not reverse)
- **Override both or neither** — broken `HashMap`/`HashSet` otherwise
- **`Comparable<T>`** — natural ordering, `compareTo()`, single sort order
- **`Comparator<T>`** — custom ordering, `compare()`, multiple sort orders
- **Java 8:** `Comparator.comparing(Person::getAge).thenComparing(Person::getName)`

### 6.8 Serialization
- `Serializable` marker interface
- `serialVersionUID` — version control
- `transient` fields — excluded
- `Externalizable` — full custom control (`readExternal`, `writeExternal`)
- **Security risks** — deserialization attacks, use allowlists

### 6.9 Reflection
- Inspect/modify classes, methods, fields at runtime
- `Class.forName()`, `getDeclaredFields()`, `setAccessible(true)`
- Used by: Spring (DI, AOP), Hibernate, Jackson, JUnit
- **Performance cost** — slow, breaks encapsulation
- **Break singleton, access private fields** — interview classic

### 6.10 Records (Java 14+), Sealed Classes (Java 17+)
- **Records:** Immutable data carriers. Auto-generate `equals`, `hashCode`, `toString`, getters.
  ```java
  public record Point(int x, int y) {}
  ```
- **Sealed Classes:** Restrict which classes can extend.
  ```java
  public sealed class Shape permits Circle, Square {}
  ```
- **Pattern Matching:** `instanceof` with variable binding, switch expressions.

---

## VII. JAVA COLLECTIONS FRAMEWORK (Deep Dive)

### 7.1 Collection Hierarchy
```
Iterable
  └── Collection
        ├── List (ordered, duplicates allowed)
        │     ├── ArrayList — O(1) get, O(n) insert middle, dynamic array
        │     ├── LinkedList — O(1) insert/delete at ends, O(n) get, doubly-linked
        │     ├── Vector — synchronized ArrayList (legacy)
        │     └── CopyOnWriteArrayList — snapshot iteration, write copies array
        ├── Set (no duplicates)
        │     ├── HashSet — O(1) add/remove/contains, backed by HashMap
        │     ├── LinkedHashSet — insertion order preserved
        │     ├── TreeSet — O(log n), sorted, backed by TreeMap (Red-Black Tree)
        │     ├── EnumSet — bit-vector, fastest for enums
        │     └── CopyOnWriteArraySet
        └── Queue
              ├── PriorityQueue — min-heap by default
              ├── ArrayDeque — resizable circular array, faster than LinkedList as stack/queue
              ├── LinkedList (also implements Deque)
              ├── ConcurrentLinkedQueue
              ├── LinkedBlockingQueue
              └── ArrayBlockingQueue

Map (not part of Collection)
  ├── HashMap — O(1) avg, null key allowed, NOT thread-safe
  │     └── (Java 8: bucket = LinkedList → TreeMap when size > 8)
  ├── LinkedHashMap — insertion/access order, good for LRU cache
  ├── TreeMap — O(log n), sorted keys, Red-Black Tree, NavigableMap
  ├── Hashtable — synchronized (legacy), no null keys/values
  ├── ConcurrentHashMap — segment-level locking (Java 7), CAS + synchronized (Java 8)
  ├── WeakHashMap — keys are weak references, GC can reclaim
  └── EnumMap — array-backed, fastest for enum keys
```

### 7.2 HashMap Internals (FAANG Favorite)
- **Structure:** Array of buckets → LinkedList → Red-Black Tree (when bucket > 8 nodes, Java 8+)
- **Hashing:** `hash(key)` → `(n-1) & hash` for bucket index
- **Load Factor:** 0.75 default. Resize (double) when `size > capacity * loadFactor`.
- **Rehashing:** All entries rehashed to new array — O(n).
- **Collision Resolution:** Separate chaining.
- **`null` key:** Always goes to bucket 0.
- **Thread-unsafe:** Use `ConcurrentHashMap` or `Collections.synchronizedMap()`.

### 7.3 ConcurrentHashMap Internals
- **Java 7:** Segment-based locking (16 segments default).
- **Java 8+:** CAS (Compare-And-Swap) + `synchronized` on individual bucket heads (node-level locking).
- **No null keys or values** (unlike HashMap).
- **Weakly consistent iterators** — no `ConcurrentModificationException`.
- **Atomic operations:** `putIfAbsent()`, `compute()`, `merge()`.

### 7.4 When to Use What

| Need | Use |
|---|---|
| Fast random access by index | `ArrayList` |
| Frequent insert/delete at ends | `ArrayDeque` or `LinkedList` |
| Unique elements, no order | `HashSet` |
| Unique elements, sorted | `TreeSet` |
| Key-value, fast lookup | `HashMap` |
| Key-value, sorted keys | `TreeMap` |
| Key-value, insertion order | `LinkedHashMap` |
| Thread-safe map | `ConcurrentHashMap` |
| Thread-safe list (read-heavy) | `CopyOnWriteArrayList` |
| Priority-based processing | `PriorityQueue` |
| Stack | `ArrayDeque` (not `Stack` class) |
| Queue | `ArrayDeque` or `LinkedList` |
| LRU Cache | `LinkedHashMap` with `removeEldestEntry()` |

---

## VIII. JAVA CONCURRENCY & MULTITHREADING

### 8.1 Thread Creation
- Extend `Thread` (bad — single inheritance)
- Implement `Runnable` (better — no return value)
- Implement `Callable<V>` (best — returns `Future<V>`, throws exceptions)

### 8.2 Thread Lifecycle
`NEW → RUNNABLE → RUNNING → BLOCKED/WAITING/TIMED_WAITING → TERMINATED`

### 8.3 Synchronization Primitives
- **`synchronized`** — intrinsic lock / monitor lock, reentrant
- **`volatile`** — visibility only, no atomicity
- **`ReentrantLock`** — explicit lock, tryLock, fairness, interruptible
- **`ReadWriteLock`** — multiple readers OR one writer
- **`StampedLock`** — optimistic reads (Java 8+)
- **`Semaphore`** — control access to N resources
- **`CountDownLatch`** — wait for N threads to complete (one-time use)
- **`CyclicBarrier`** — N threads wait for each other at barrier (reusable)
- **`Phaser`** — flexible CyclicBarrier with phases
- **`Exchanger`** — two threads swap data

### 8.4 Atomic Variables
- `AtomicInteger`, `AtomicLong`, `AtomicReference`, `AtomicBoolean`
- CAS (Compare-And-Swap) — lock-free, non-blocking
- `LongAdder` — better than `AtomicLong` under high contention

### 8.5 Executor Framework
```
Executor
  └── ExecutorService
        ├── ThreadPoolExecutor (core)
        │     Parameters: corePoolSize, maxPoolSize, keepAliveTime, workQueue, rejectionPolicy
        ├── ScheduledThreadPoolExecutor
        └── ForkJoinPool (work-stealing, Java 7+)

Executors Factory Methods:
  - newFixedThreadPool(n) — bounded
  - newCachedThreadPool() — unbounded, 60s idle timeout
  - newSingleThreadExecutor() — single thread, ordered
  - newScheduledThreadPool(n) — delayed/periodic tasks
  - newWorkStealingPool() — ForkJoinPool based
```

### 8.6 CompletableFuture (Java 8+)
- Async pipelines: `supplyAsync()`, `thenApply()`, `thenCompose()`, `thenCombine()`
- Error handling: `exceptionally()`, `handle()`
- Combine: `allOf()`, `anyOf()`
- **When:** Non-blocking async calls, parallel API calls, reactive-style pipelines.

### 8.7 Thread Safety Patterns
- **Immutability** — best thread safety (no shared mutable state)
- **ThreadLocal** — per-thread storage (user context, DB connection)
  - ⚠️ Memory leak in thread pools — always `remove()`
- **Confinement** — don't share at all
- **Synchronized collection wrappers** — `Collections.synchronizedList()`
- **Concurrent collections** — `ConcurrentHashMap`, `CopyOnWriteArrayList`, `BlockingQueue`

### 8.8 Common Concurrency Problems
- **Deadlock** — circular wait. Prevention: lock ordering, tryLock with timeout.
- **Livelock** — threads keep responding to each other, no progress.
- **Starvation** — thread never gets CPU. Use fair locks.
- **Race Condition** — check-then-act without atomicity.
- **Visibility** — thread caches stale value. Use `volatile` or `synchronized`.
- **Double-Checked Locking** — singleton pattern, needs `volatile`.

### 8.9 Virtual Threads (Java 21+ / Project Loom) ★
- Lightweight threads managed by JVM, not OS.
- Millions of virtual threads possible.
- `Thread.ofVirtual().start(runnable)` or `Executors.newVirtualThreadPerTaskExecutor()`
- **When:** I/O-bound tasks, replace reactive programming complexity.
- **Don't:** Use for CPU-bound tasks, use `synchronized` (prefer `ReentrantLock`).

---

## IX. JAVA 8+ FEATURES (Interviewers expect fluency)

### 9.1 Lambda Expressions & Functional Interfaces
- `@FunctionalInterface` — single abstract method
- Built-in: `Predicate<T>`, `Function<T,R>`, `Consumer<T>`, `Supplier<T>`, `BiFunction<T,U,R>`, `UnaryOperator<T>`
- Method references: `ClassName::method`, `instance::method`, `ClassName::new`

### 9.2 Streams API
- **Pipeline:** Source → Intermediate ops (lazy) → Terminal op (triggers execution)
- **Intermediate:** `filter`, `map`, `flatMap`, `distinct`, `sorted`, `peek`, `limit`, `skip`
- **Terminal:** `collect`, `forEach`, `reduce`, `count`, `findFirst`, `anyMatch`, `toArray`
- **Collectors:** `toList()`, `toSet()`, `toMap()`, `groupingBy()`, `partitioningBy()`, `joining()`
- **Parallel Streams:** `parallelStream()` — uses ForkJoinPool. Careful with shared state!
- **When NOT to use:** Side effects, small collections, ordered + parallel

### 9.3 Optional
- `Optional.of()`, `Optional.ofNullable()`, `Optional.empty()`
- `map()`, `flatMap()`, `filter()`, `orElse()`, `orElseGet()`, `orElseThrow()`
- **Don't:** Use as method parameter, use for fields, use `Optional.get()` without check.

### 9.4 Date/Time API (java.time)
- `LocalDate`, `LocalTime`, `LocalDateTime`, `ZonedDateTime`, `Instant`
- `Duration` (time-based), `Period` (date-based)
- `DateTimeFormatter` — thread-safe (unlike `SimpleDateFormat`)

### 9.5 Other Key Features
- **Java 10:** `var` (local variable type inference)
- **Java 14:** Records, helpful NPE messages, switch expressions
- **Java 15:** Text blocks (`"""..."""`)
- **Java 16:** Pattern matching for `instanceof`
- **Java 17:** Sealed classes, pattern matching switch (preview)
- **Java 21:** Virtual threads, sequenced collections, pattern matching for switch (final)

---

# PART C: SPRING FRAMEWORK & SPRING BOOT

---

## X. SPRING CORE

### 10.1 IoC (Inversion of Control) & DI (Dependency Injection)
- **IoC Container:** `ApplicationContext` (eager) vs `BeanFactory` (lazy)
- **DI Types:**
  - Constructor injection (preferred — immutable, testable)
  - Setter injection (optional dependencies)
  - Field injection (`@Autowired` on field — discouraged, untestable)
- **Why constructor injection is best:** Immutability, required deps enforced, no reflection needed, easy to test.

### 10.2 Bean Lifecycle
```
Instantiation → Populate Properties → BeanNameAware → BeanFactoryAware →
ApplicationContextAware → @PostConstruct → InitializingBean.afterPropertiesSet() →
Custom init-method → Bean Ready → @PreDestroy → DisposableBean.destroy() →
Custom destroy-method
```

### 10.3 Bean Scopes
| Scope | Description |
|---|---|
| `singleton` | One instance per container (default) |
| `prototype` | New instance per injection/request |
| `request` | One per HTTP request (web) |
| `session` | One per HTTP session (web) |
| `application` | One per ServletContext |
| `websocket` | One per WebSocket session |

- **Trap:** Injecting `prototype` into `singleton` — prototype becomes effectively singleton.
- **Fix:** Use `ObjectFactory<T>`, `Provider<T>`, or `@Lookup`.

### 10.4 Annotations Deep Dive
- **Stereotype:** `@Component`, `@Service`, `@Repository`, `@Controller`, `@RestController`
- **Configuration:** `@Configuration`, `@Bean`, `@Import`, `@PropertySource`
- **Injection:** `@Autowired`, `@Qualifier`, `@Primary`, `@Value`
- **Lifecycle:** `@PostConstruct`, `@PreDestroy`, `@Lazy`
- **Conditional:** `@Conditional`, `@ConditionalOnProperty`, `@Profile`

### 10.5 Spring AOP (Aspect-Oriented Programming)
- **Concepts:** Aspect, Advice, Pointcut, JoinPoint, Weaving
- **Advice Types:**
  - `@Before` — before method execution
  - `@After` — after method (regardless of outcome)
  - `@AfterReturning` — after successful return
  - `@AfterThrowing` — after exception
  - `@Around` — wraps method (most powerful)
- **Proxy Mechanism:**
  - JDK Dynamic Proxy — interface-based (default for interfaces)
  - CGLIB Proxy — subclass-based (default for classes)
- **Use Cases:** Logging, transaction management, security, caching, metrics

### 10.6 Spring Events
- `ApplicationEvent`, `ApplicationEventPublisher`
- `@EventListener`, `@Async` + `@EventListener`
- `@TransactionalEventListener` — fire after transaction commit
- **When:** Decouple components, async processing, audit logging

---

## XI. SPRING BOOT

### 11.1 Auto-Configuration
- `@SpringBootApplication` = `@Configuration` + `@EnableAutoConfiguration` + `@ComponentScan`
- `spring.factories` / `AutoConfiguration.imports` — lists auto-config classes
- `@ConditionalOnClass`, `@ConditionalOnMissingBean`, `@ConditionalOnProperty`
- **How it works:** Spring Boot scans classpath, finds libraries, auto-configures beans.

### 11.2 Externalized Configuration (Priority Order)
```
1. Command-line args
2. JNDI
3. System properties
4. OS environment variables
5. application-{profile}.properties/yml
6. application.properties/yml
7. @PropertySource
8. Default properties
```
- `@Value("${property}")`, `@ConfigurationProperties(prefix = "app")`

### 11.3 Profiles
- `@Profile("dev")`, `@Profile("!prod")`
- `spring.profiles.active=dev`
- Profile-specific config: `application-dev.yml`, `application-prod.yml`

### 11.4 Actuator
- Health checks: `/actuator/health`
- Metrics: `/actuator/metrics`
- Info: `/actuator/info`
- Custom health indicators: extend `HealthIndicator`
- **Production:** Expose only necessary endpoints, secure with Spring Security.

### 11.5 Embedded Server
- Tomcat (default), Jetty, Undertow, Netty (WebFlux)
- Customize: `server.port`, `server.servlet.context-path`

---

## XII. SPRING WEB / REST API DESIGN

### 12.1 Controller Layer
- `@RestController` = `@Controller` + `@ResponseBody`
- `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping`
- `@PathVariable`, `@RequestParam`, `@RequestBody`, `@RequestHeader`
- `@Valid` / `@Validated` for input validation

### 12.2 Exception Handling
- `@ExceptionHandler` — controller-level
- `@ControllerAdvice` / `@RestControllerAdvice` — global
- `ResponseStatusException` — inline
- **Best Practice:** Return consistent error response DTO:
  ```json
  { "timestamp": "...", "status": 400, "error": "Bad Request", "message": "...", "path": "/api/..." }
  ```

### 12.3 Request Validation
- Bean Validation (JSR 380): `@NotNull`, `@NotBlank`, `@Size`, `@Min`, `@Max`, `@Email`, `@Pattern`
- `@Valid` on `@RequestBody` parameters
- Custom validator: implement `ConstraintValidator<A, T>`
- Group validation: `@Validated(OnCreate.class)`

### 12.4 REST API Best Practices
- Use nouns, not verbs: `/api/users` not `/api/getUsers`
- Proper HTTP methods and status codes
- Versioning: URI (`/v1/users`), header, query param
- Pagination: `?page=0&size=20&sort=name,asc`
- HATEOAS (know what it is, rarely used in practice)
- Idempotency: GET, PUT, DELETE are idempotent; POST is not
- Rate limiting headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`

### 12.5 Filters, Interceptors, AOP
| Layer | Interface | Use Case |
|---|---|---|
| Servlet Filter | `Filter` / `OncePerRequestFilter` | Auth, CORS, logging, request wrapping |
| Handler Interceptor | `HandlerInterceptor` | Pre/post controller logic, timing |
| AOP | `@Aspect` | Cross-cutting concerns, method-level |
- **Order:** Filter → Interceptor → Controller → AOP (around the method)

---

## XIII. SPRING DATA & DATABASE

### 13.1 Spring Data JPA
- `JpaRepository<T, ID>` — CRUD + pagination + sorting
- **Derived queries:** `findByNameAndAge()`, `findByStatusOrderByCreatedAtDesc()`
- **`@Query`:** Custom JPQL or native SQL
- **Specification API:** Dynamic queries with `Specification<T>`
- **Projections:** Interface-based, class-based (DTO), dynamic

### 13.2 JPA / Hibernate Concepts
- **Entity States:** Transient → Persistent → Detached → Removed
- **Lazy vs Eager Loading:**
  - `@OneToMany` — LAZY by default (good)
  - `@ManyToOne` — EAGER by default (often bad)
  - `LazyInitializationException` — access lazy field outside session
  - Fix: `@EntityGraph`, `JOIN FETCH`, `@Transactional`
- **N+1 Problem:**
  - What: 1 query for parent + N queries for each child
  - Fix: `JOIN FETCH`, `@EntityGraph`, `@BatchSize`, DTO projection
- **Caching:**
  - L1 Cache — per session/EntityManager (always on)
  - L2 Cache — shared across sessions (EhCache, Hazelcast)
  - Query Cache — cache query results
- **Optimistic Locking:** `@Version` — throws `OptimisticLockException` on conflict
- **Pessimistic Locking:** `@Lock(LockModeType.PESSIMISTIC_WRITE)`
- **Dirty Checking:** Hibernate auto-detects changes to managed entities and flushes.
- **`@Transactional` Propagation:**
  - `REQUIRED` (default) — join existing or create new
  - `REQUIRES_NEW` — always new, suspend current
  - `NESTED` — savepoint within current
  - `SUPPORTS` — use if exists, else non-transactional
  - `MANDATORY` — must exist, else exception
  - `NOT_SUPPORTED` — suspend current, run non-transactional
  - `NEVER` — must NOT exist, else exception

### 13.3 Database Migration
- **Flyway** — SQL-based migrations, versioned (`V1__init.sql`, `V2__add_column.sql`)
- **Liquibase** — XML/YAML/JSON/SQL changelogs
- **Best Practice:** Never modify existing migrations. Always add new ones.

---

## XIV. SPRING SECURITY

### 14.1 Authentication & Authorization
- **Authentication:** Who are you? (login)
- **Authorization:** What can you do? (permissions)
- **SecurityFilterChain** — chain of filters processing every request
- **Key Filters:** `UsernamePasswordAuthenticationFilter`, `BasicAuthenticationFilter`, `BearerTokenAuthenticationFilter`

### 14.2 JWT Authentication Flow
```
1. Client sends credentials → /auth/login
2. Server validates → generates JWT (access + refresh token)
3. Client sends JWT in Authorization: Bearer <token>
4. JwtAuthenticationFilter extracts + validates token
5. Set SecurityContext → proceed to controller
```

### 14.3 Method-Level Security
- `@PreAuthorize("hasRole('ADMIN')")` — before method
- `@PostAuthorize("returnObject.owner == authentication.name")` — after method
- `@Secured("ROLE_ADMIN")` — simpler, role-only
- `@RolesAllowed("ADMIN")` — JSR-250

### 14.4 OAuth 2.0 / OpenID Connect
- **Grant Types:** Authorization Code (most secure), Client Credentials (machine-to-machine), Refresh Token
- **Spring Authorization Server** — build your own OAuth server
- **Resource Server** — validate JWT, protect APIs

### 14.5 CORS Configuration
- `@CrossOrigin` on controller
- Global: `WebMvcConfigurer.addCorsMappings()`
- Security: `http.cors(c -> c.configurationSource(...))`

---

## XV. SPRING MICROSERVICES PATTERNS

### 15.1 Service Communication
- **REST** — `RestTemplate` (legacy), `WebClient` (reactive, preferred), `RestClient` (Java 17+)
- **gRPC** — protobuf, high performance
- **Messaging** — Kafka, RabbitMQ via Spring Cloud Stream

### 15.2 Service Discovery
- **Eureka** — Netflix, client-side discovery
- **Consul** — HashiCorp, health checking
- **Kubernetes DNS** — built-in service discovery

### 15.3 API Gateway
- **Spring Cloud Gateway** — reactive, route + filter + predicate
- **Features:** Rate limiting, circuit breaker, load balancing, path rewriting

### 15.4 Circuit Breaker
- **Resilience4j** (replaced Hystrix)
- States: CLOSED → OPEN → HALF_OPEN
- `@CircuitBreaker(name = "service", fallbackMethod = "fallback")`
- Also: `@Retry`, `@RateLimiter`, `@Bulkhead`, `@TimeLimiter`

### 15.5 Distributed Configuration
- **Spring Cloud Config Server** — centralized config, Git-backed
- **Consul KV** / **Vault** — config + secrets

### 15.6 Distributed Tracing & Observability
- **Micrometer** — metrics facade (Prometheus, Datadog)
- **Spring Boot Actuator** — health, metrics, info
- **Micrometer Tracing** (replaced Sleuth) + **Zipkin/Jaeger** — distributed tracing
- **Trace ID / Span ID** propagation across services

### 15.7 Event-Driven with Spring
- **Spring Cloud Stream** — Kafka/RabbitMQ abstraction
- **Spring Kafka** — `@KafkaListener`, `KafkaTemplate`
- **Spring AMQP** — RabbitMQ integration
- **Transactional Outbox** — use `@TransactionalEventListener` + outbox table

---

## XVI. TESTING (Expected at 5 YOE)

### 16.1 Unit Testing
- **JUnit 5:** `@Test`, `@BeforeEach`, `@AfterEach`, `@DisplayName`, `@ParameterizedTest`
- **Mockito:** `@Mock`, `@InjectMocks`, `@Spy`, `when().thenReturn()`, `verify()`, `ArgumentCaptor`
- **AssertJ:** Fluent assertions `assertThat(result).isEqualTo(expected)`
- **Test Naming:** `should_ReturnUser_When_ValidIdProvided()`

### 16.2 Integration Testing
- `@SpringBootTest` — full context
- `@WebMvcTest` — controller layer only
- `@DataJpaTest` — JPA/repository layer only
- `@MockBean` — replace bean with mock in context
- **Testcontainers** — real DB/Kafka/Redis in Docker for tests

### 16.3 Test Patterns
- **AAA:** Arrange → Act → Assert
- **Builder pattern for test data** — `UserBuilder.aUser().withName("John").build()`
- **Test slicing** — load only relevant context
- **Contract testing** — Pact, Spring Cloud Contract

---

# PART D: CLASSIC LLD PROBLEMS (Machine Coding Round)

---

## XVII. MUST-DO LLD PROBLEMS

### 17.1 Structural / System Design
| # | Problem | Key Patterns |
|---|---|---|
| 1 | **Parking Lot** | Strategy, Factory, Observer, Enum |
| 2 | **Elevator System** | State, Strategy, Observer, Scheduler |
| 3 | **Library Management** | Repository, Observer (due dates), Strategy (search) |
| 4 | **Hotel Booking System** | Builder, Strategy (pricing), Observer (notifications) |
| 5 | **Movie Ticket Booking (BookMyShow)** | Strategy (seat selection), Observer, Concurrency (seat locking) |
| 6 | **Ride-Sharing (Uber/Ola)** | Strategy (matching, pricing), Observer, State (ride lifecycle) |
| 7 | **Food Delivery (Swiggy/Zomato)** | Strategy (restaurant ranking), Observer, State (order status) |
| 8 | **E-Commerce (Amazon)** | Strategy (payment, shipping), Observer, Cart as Composite |
| 9 | **ATM Machine** | State, Chain of Responsibility (dispense), Strategy |
| 10 | **Vending Machine** | State, Strategy (payment), Chain of Responsibility |

### 17.2 Game Design
| # | Problem | Key Patterns |
|---|---|---|
| 11 | **Chess** | Strategy (piece movement), Composite (board), Command (undo), Observer |
| 12 | **Tic-Tac-Toe** | Strategy (AI), State, Observer |
| 13 | **Snake & Ladder** | Template Method, Strategy (dice), Observer |
| 14 | **Card Game (Blackjack/Poker)** | Strategy, Factory (card/deck), Observer |
| 15 | **Minesweeper** | Observer, Composite (grid), Proxy (hidden cells) |
| 16 | **Sudoku Solver** | Backtracking, Strategy (solving technique) |
| 17 | **Battleship** | State, Strategy, Observer |

### 17.3 Application Design
| # | Problem | Key Patterns |
|---|---|---|
| 18 | **LRU/LFU Cache** | Decorator, Strategy (eviction), LinkedHashMap |
| 19 | **Logger (Log4j-like)** | Singleton, Chain of Responsibility, Strategy (appenders), Observer |
| 20 | **Pub/Sub Messaging System** | Observer, Mediator, Strategy (delivery) |
| 21 | **Task Scheduler (Cron)** | Command, Strategy (scheduling), Priority Queue |
| 22 | **Rate Limiter** | Strategy (token bucket, sliding window), Decorator |
| 23 | **File System (Linux-like)** | Composite, Iterator, Visitor |
| 24 | **Text Editor (Vim-like)** | Command (undo/redo), Memento (state), State (modes) |
| 25 | **Spreadsheet (Excel-like)** | Observer (cell dependencies), Composite, Strategy |
| 26 | **URL Shortener** | Factory, Strategy (hashing), Repository |
| 27 | **Social Media (Twitter)** | Observer (follow), Strategy (feed ranking), Decorator |
| 28 | **Stack Overflow** | Observer (notifications), Strategy (ranking), Composite (threads) |
| 29 | **Online Auction System** | Observer (bidding), State (auction lifecycle), Strategy (bidding rules) |
| 30 | **Chat Application** | Mediator, Observer, Command |

### 17.4 Infrastructure Design
| # | Problem | Key Patterns |
|---|---|---|
| 31 | **Connection Pool** | Singleton, Object Pool, Proxy |
| 32 | **Thread Pool** | Command, Strategy (rejection policy), Observer |
| 33 | **In-Memory Database** | Repository, Strategy (indexing), Observer (triggers) |
| 34 | **API Rate Limiter** | Strategy, Decorator, Proxy |
| 35 | **Circuit Breaker** | State, Proxy, Strategy |

---

## XVIII. LLD INTERVIEW FRAMEWORK (How to approach)

### Step 1: Clarify Requirements (2-3 min)
- What are the core use cases?
- Who are the actors?
- What are the constraints? (concurrency, scale)
- What features are out of scope?

### Step 2: Identify Core Objects (3-5 min)
- Nouns → Classes
- Verbs → Methods
- Relationships → Association, Composition, Inheritance

### Step 3: Draw Class Diagram (5 min)
- Classes with key attributes and methods
- Relationships and multiplicity
- Interfaces and abstract classes

### Step 4: Apply Design Patterns (5 min)
- Which patterns naturally fit?
- SOLID compliance check
- Extensibility points

### Step 5: Write Code (15-20 min)
- Start with interfaces/abstract classes
- Implement core logic
- Handle edge cases
- Show concurrency awareness if applicable

### Step 6: Discuss Trade-offs (2-3 min)
- Why this pattern over alternatives?
- How would this scale?
- What would change if requirement X was added?

---

## XIX. QUICK REFERENCE: PATTERN SELECTION FOR LLD

| If the problem has... | Use Pattern |
|---|---|
| Multiple algorithms/strategies | Strategy |
| Object creation based on input | Factory / Abstract Factory |
| Many optional parameters | Builder |
| State-dependent behavior | State |
| Undo/redo capability | Command + Memento |
| Event notification / listeners | Observer |
| Pipeline / chain of handlers | Chain of Responsibility |
| Part-whole hierarchy (tree) | Composite |
| Add features without modifying class | Decorator |
| Simplify complex subsystem | Facade |
| Control access to object | Proxy |
| One global instance | Singleton |
| Skeleton algorithm with custom steps | Template Method |
| Integrate incompatible interface | Adapter |
| Reduce many-to-many to many-to-one | Mediator |
| Operations on heterogeneous types | Visitor |

---

**Total Coverage: 19 Sections | 35 LLD Problems | 23+ Design Patterns | Full Java & Spring Deep Dive**

> At 5 YOE for FAANG, you're expected to write clean, extensible, SOLID-compliant code in 45 minutes.
> The difference between L4 and L5 in LLD is: **L4 makes it work. L5 makes it extensible, testable, and maintainable.**

**Study Plan:**
1. SOLID + OOP Fundamentals — 3 days
2. All 23 Design Patterns with Java code — 1 week
3. Java Core (Collections, Concurrency, JVM) — 1 week
4. Spring (Core, Boot, Data, Security) — 1 week
5. Practice LLD Problems (2/day) — 2-3 weeks
6. Mock Machine Coding Rounds — ongoing

**Total Prep Time: ~5-6 weeks (2-3 hrs/day)** 🚀

