# LOW-LEVEL DESIGN (LLD) — Complete FAANG Interview Guide (5 YOE)

> Every OOP concept, design pattern, SOLID principle, UML diagram, and classic LLD problem
> to ace any FAANG Machine Coding / LLD round.

---

# SECTION I: OOP FUNDAMENTALS

---

## 1.1 Four Pillars of OOP

### Encapsulation
- Hide internal state, expose via methods
- Use private fields + public getters/setters
- Access modifiers: `private` → `default` → `protected` → `public`
- **Information hiding** — expose behavior, not data
- **Interview Q:** "Why not make everything public?" → breaks invariants, tight coupling

### Abstraction
- Hide complexity, expose only what's necessary
- Abstract classes vs Interfaces
- Real-world analogy: Car steering wheel hides engine mechanics
- **Levels of abstraction:** High-level modules shouldn't know low-level details

### Inheritance
- IS-A relationship
- Method overriding (runtime polymorphism)
- `super` keyword — call parent constructor/methods
- Constructor chaining
- **Diamond Problem** — why Java doesn't allow multiple class inheritance
- **Fragile Base Class Problem** — changes in parent break children
- **When NOT to use:** When behavior differs (Square/Rectangle problem)
- **Prefer composition over inheritance** — almost always

### Polymorphism
- **Compile-time (Static):** Method Overloading — same name, different parameters
  - Return type alone doesn't count
  - Widening > Boxing > Varargs (resolution order)
- **Runtime (Dynamic):** Method Overriding — subclass provides specific implementation
  - Dynamic dispatch / virtual method table
  - `@Override` annotation — catch errors at compile time
- **Covariant return types** — overridden method can return subtype
- **Parametric polymorphism** — Generics (`List<T>`)
- **Ad-hoc polymorphism** — Overloading

---

## 1.2 Association Relationships

| Relationship | Strength | Lifecycle | Example |
|---|---|---|---|
| **Dependency** | Weakest | Temporary (method param) | `Car` uses `FuelStation` |
| **Association** | Weak | Independent | `Teacher` knows `Student` |
| **Aggregation** | Medium | Independent (HAS-A, shared) | `Department` has `Professors` |
| **Composition** | Strongest | Dependent (HAS-A, owned) | `House` has `Rooms` |
| **Inheritance** | Strong | IS-A | `Dog` is `Animal` |
| **Realization** | Contract | Implements | `ArrayList` implements `List` |

**Interview Tip:** Always identify relationships first when designing classes.

---

## 1.3 Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---|---|---|
| Methods | Abstract + concrete | Abstract + `default` (Java 8+) + `static` + `private` (Java 9+) |
| Fields | Instance variables, any modifier | Only `public static final` |
| Constructor | Yes | No |
| Inheritance | Single | Multiple |
| Access modifiers | Any | `public` (methods), `public static final` (fields) |
| **When to use** | Shared state + partial implementation | Pure contract / capability / mixin |

**When to use Abstract Class:**
- Common base implementation shared across subclasses
- Need instance variables or constructors
- Template Method pattern

**When to use Interface:**
- Define a capability/contract (e.g., `Serializable`, `Comparable`)
- Multiple inheritance of behavior needed
- Strategy pattern, Observer pattern, etc.

---

## 1.4 Coupling & Cohesion

### Coupling (aim for LOW)
- **Content Coupling** — one class modifies internals of another (worst)
- **Common Coupling** — shared global data
- **Control Coupling** — one class controls flow of another via flags
- **Stamp Coupling** — pass data structures, use only part
- **Data Coupling** — pass only needed data (best)
- **Message Coupling** — communicate via messages only (ideal)

### Cohesion (aim for HIGH)
- **Coincidental** — random grouping (worst)
- **Logical** — related by category, not function
- **Temporal** — things done at same time
- **Procedural** — things done in sequence
- **Communicational** — operate on same data
- **Sequential** — output of one is input of next
- **Functional** — everything contributes to single task (best)

---

## 1.5 Object-Oriented Analysis & Design (OOAD) Process

1. **Gather Requirements** — use cases, actors, constraints
2. **Identify Objects/Entities** — nouns → classes
3. **Identify Behaviors** — verbs → methods
4. **Define Relationships** — IS-A, HAS-A, uses
5. **Apply Design Principles** — SOLID, DRY, KISS
6. **Select Design Patterns** — fit patterns to problems
7. **Draw UML Diagrams** — class, sequence, state
8. **Implement & Iterate**

---

# SECTION II: SOLID PRINCIPLES

---

## S — Single Responsibility Principle
- A class should have only ONE reason to change
- **Violation:**
  ```
  UserService {
    registerUser()    // user logic
    sendEmail()       // email logic
    logActivity()     // logging logic
    generateReport()  // reporting logic
  }
  ```
- **Fix:** `UserService`, `EmailService`, `AuditLogger`, `ReportGenerator`
- **Test:** "If I describe what this class does, do I use the word 'AND'?"
- **Real-world:** Controller handles HTTP, Service handles logic, Repository handles data

---

## O — Open/Closed Principle
- Open for extension, closed for modification
- **Violation:** `if-else` / `switch` chain for every new type — must edit existing code
- **Fix:** Use Strategy / polymorphism — add new class implementing interface
- **Example:** `PaymentStrategy` interface → `CreditCardPayment`, `UPIPayment`, `CryptoPayment`
- Adding `CryptoPayment` requires ZERO changes to existing code

---

## L — Liskov Substitution Principle
- Subtypes must be substitutable for base types without breaking correctness
- **Violation:** `Square extends Rectangle` — `setWidth()` changes height too, breaks client expectations
- **Rules:**
  - Preconditions cannot be strengthened in subtype
  - Postconditions cannot be weakened in subtype
  - Invariants must be preserved
  - No new exceptions that parent doesn't throw (checked)
- **Fix:** Separate hierarchies, or use composition

---

## I — Interface Segregation Principle
- No client should be forced to depend on methods it doesn't use
- **Violation:**
  ```
  interface Worker {
    void work();
    void eat();
    void sleep();
  }
  // RobotWorker forced to implement eat() and sleep() — meaningless
  ```
- **Fix:** `Workable`, `Eatable`, `Sleepable` — Robot only implements `Workable`
- **Test:** "Does any implementer have empty/dummy methods?"

---

## D — Dependency Inversion Principle
- High-level modules should not depend on low-level modules. Both should depend on abstractions.
- Abstractions should not depend on details. Details should depend on abstractions.
- **Violation:** `OrderService` → `MySQLOrderRepository` (concrete dependency)
- **Fix:** `OrderService` → `OrderRepository` (interface) ← `MySQLOrderRepository`
- **Enables:** Easy swapping (MySQL → Postgres), easy testing (mock repository)

---

# SECTION III: DESIGN PATTERNS

---

## 3.1 CREATIONAL PATTERNS (5)

### Singleton
- **Intent:** Ensure one instance, provide global access
- **Implementations:**
  1. **Enum Singleton** (BEST — serialization-safe, reflection-safe, thread-safe)
  2. **Bill Pugh** (static inner class — lazy, thread-safe)
  3. **Double-Checked Locking** (`volatile` + `synchronized`)
  4. **Eager Initialization** (static field)
- **Breaking Singleton:** Reflection, Serialization (`readResolve()`), Cloning (`clone()`)
- **Prevention:** Enum (immune to all), throw exception in constructor if instance exists
- **When:** Logger, ConfigManager, ConnectionPool, CacheManager, Registry
- **Anti-pattern risk:** Hidden global state, makes testing hard

### Factory Method
- **Intent:** Define interface for creating objects, let subclasses decide which class
- **Structure:** `Creator.factoryMethod()` → returns `Product` interface
- **When:** Creation logic depends on subclass/input, decouple client from concrete classes
- **Example:** `LoggerFactory.getLogger()` — returns `FileLogger`, `ConsoleLogger`, `DBLogger`
- **vs Simple Factory:** Simple Factory = static method with if-else (not a GoF pattern but commonly used)

### Abstract Factory
- **Intent:** Create families of related objects without specifying concrete classes
- **When:** Cross-platform UI, database-specific DAOs, theme-based components
- **Example:** `UIFactory` → `WindowsUIFactory` (creates `WindowsButton`, `WindowsDialog`)
- **Key:** Ensures product compatibility within a family

### Builder
- **Intent:** Construct complex objects step by step, separate construction from representation
- **When:** Many optional params, immutable objects, fluent API
- **Structure:** `Product`, `Builder` (inner static class), `Director` (optional)
- **Example:** `User.builder().name("John").age(30).email("j@g.com").build()`
- **Lombok:** `@Builder` auto-generates
- **vs Telescoping Constructor:** Builder is cleaner, more readable

### Prototype
- **Intent:** Create objects by cloning existing instances
- **When:** Object creation is expensive (DB fetch, network call), need copies with slight variations
- **Shallow Copy:** References shared — `clone()` default
- **Deep Copy:** Everything duplicated — manual or serialization-based
- **Java:** `Cloneable` interface + `clone()` method
- **Registry variant:** Store prototypes in a map, clone by key

---

## 3.2 STRUCTURAL PATTERNS (7)

### Adapter
- **Intent:** Convert incompatible interface to expected interface
- **Object Adapter** (composition — preferred) vs **Class Adapter** (inheritance)
- **When:** Integrate legacy/third-party code, `XMLToJSONAdapter`, `SquarePegAdapter`
- **Real-world:** `Arrays.asList()` adapts array to List interface

### Bridge
- **Intent:** Decouple abstraction from implementation, both vary independently
- **When:** Two independent dimensions of variation would cause class explosion
- **Example:** `Shape` (Circle, Square) × `Renderer` (Vector, Raster) = 2+2 classes instead of 2×2
- **Key:** Abstraction HAS-A Implementation (composition)

### Composite
- **Intent:** Compose objects into tree structures, treat individual and composite uniformly
- **When:** Part-whole hierarchies, recursive structures
- **Example:** FileSystem (`File` + `Directory` both implement `FileSystemComponent`)
- **Operations:** `add()`, `remove()`, `getChild()`, `operation()`

### Decorator
- **Intent:** Attach additional responsibilities dynamically, alternative to subclassing
- **Structure:** Decorator wraps Component, both share same interface
- **When:** Add features without modifying existing code, combine behaviors
- **Example:** `BufferedInputStream(new FileInputStream(new File("f.txt")))`
- **Example:** `BasicPizza` → `CheeseDecorator` → `OliveDecorator` → price accumulates
- **vs Inheritance:** Decorator composes at runtime; inheritance is compile-time

### Facade
- **Intent:** Provide simplified interface to complex subsystem
- **When:** Shield clients from subsystem complexity
- **Example:** `OrderFacade.placeOrder()` internally calls `InventoryService`, `PaymentService`, `NotificationService`, `ShippingService`
- **Not about hiding:** Subsystem still accessible directly if needed

### Flyweight
- **Intent:** Share common state to support large numbers of fine-grained objects
- **Intrinsic State:** Shared, immutable (stored in flyweight)
- **Extrinsic State:** Unique per context (passed by client)
- **When:** Many similar objects consuming too much memory
- **Example:** `String.intern()`, `Integer.valueOf(-128..127)`, game character sprites
- **Key:** Flyweight objects must be immutable

### Proxy
- **Intent:** Provide surrogate/placeholder to control access
- **Types:**
  - **Virtual Proxy** — lazy initialization of heavy objects
  - **Protection Proxy** — access control / permissions
  - **Remote Proxy** — represent remote object locally (RMI)
  - **Caching Proxy** — cache results of expensive operations
  - **Logging Proxy** — log access/operations
  - **Smart Reference** — reference counting, cleanup
- **Java:** `java.lang.reflect.Proxy` (JDK dynamic proxy), CGLIB
- **Spring:** AOP proxies, `@Transactional`, `@Cacheable` — all proxy-based

---

## 3.3 BEHAVIORAL PATTERNS (11)

### Strategy
- **Intent:** Define algorithm family, encapsulate each, make interchangeable
- **When:** Multiple algorithms for same task, switch at runtime
- **Example:** `SortStrategy` → `QuickSort`, `MergeSort`, `HeapSort`
- **Example:** `CompressionStrategy` → `ZipCompression`, `GzipCompression`
- **Java 8+:** Lambda/functional interface eliminates class explosion
  ```java
  paymentService.pay(amount, order, (a, o) -> processViaUPI(a, o));
  ```

### Observer (Pub/Sub)
- **Intent:** One-to-many dependency, notify all dependents on state change
- **Components:** Subject (Observable), Observer (Listener), Event
- **When:** Event systems, UI listeners, notification systems, stock tickers
- **Push vs Pull:** Push = subject sends data; Pull = observer asks subject
- **Java:** `PropertyChangeSupport`, Spring `ApplicationEvent` + `@EventListener`
- **Pitfall:** Memory leak if observer not deregistered, ordering issues

### Command
- **Intent:** Encapsulate request as object, support undo/redo, queuing, logging
- **Components:** Command (interface), ConcreteCommand, Receiver, Invoker
- **When:** Undo/redo (text editor), macro recording, task queues, transaction systems
- **Example:** `LightOnCommand` wraps `Light.turnOn()`, `RemoteControl` invokes
- **Undo:** Each command stores previous state, `undo()` method reverses

### State
- **Intent:** Object alters behavior when internal state changes
- **When:** Clear state transitions, behavior varies by state
- **Example:** VendingMachine states: `IdleState`, `HasMoneyState`, `DispensingState`
- **Example:** Order: `Created` → `Paid` → `Shipped` → `Delivered` → `Returned`
- **vs Strategy:** State transitions internally; Strategy chosen externally by client
- **Implementation:** State interface + concrete state classes, context delegates to current state

### Template Method
- **Intent:** Define algorithm skeleton, defer steps to subclasses
- **When:** Same structure, different details across implementations
- **Example:** `DataParser` → `parseFile()` is template → `readData()`, `processData()`, `validate()` are abstract hooks
- **Hook methods:** Optional override points with default behavior
- **Hollywood Principle:** "Don't call us, we'll call you"

### Chain of Responsibility
- **Intent:** Pass request along chain of handlers, each decides to process or forward
- **When:** Multiple handlers, processing order matters, decouple sender from receiver
- **Example:** Logging: `DebugLogger` → `InfoLogger` → `ErrorLogger`
- **Example:** Approval: `Manager($1K)` → `Director($10K)` → `VP($100K)` → `CEO(unlimited)`
- **Example:** Spring Security filter chain, Servlet filters, middleware pipeline
- **Variant:** Each handler can process AND forward (vs process OR forward)

### Iterator
- **Intent:** Sequential access without exposing underlying representation
- **Java:** `Iterable<T>` (has `iterator()`) + `Iterator<T>` (has `hasNext()`, `next()`, `remove()`)
- **Fail-fast:** `ArrayList` iterator throws `ConcurrentModificationException` if modified during iteration
- **Fail-safe:** `CopyOnWriteArrayList` iterator works on snapshot — no exception
- **Enhanced for-loop:** Uses iterator internally
- **Custom Iterator:** Implement `Iterator<T>` for custom data structures (tree traversal, etc.)

### Mediator
- **Intent:** Centralize complex communications, reduce direct dependencies
- **When:** Many objects communicate, creating spaghetti dependencies
- **Example:** Chat Room (mediator) — users send messages to room, room distributes
- **Example:** ATC (mediator) — planes don't communicate directly
- **Benefit:** N×N dependencies → N×1 dependencies

### Memento
- **Intent:** Capture and externalize internal state for later restoration
- **Components:** Originator (creates memento), Memento (stores state), Caretaker (manages mementos)
- **When:** Undo, checkpoints, game save/load, transaction rollback
- **Key:** Memento should not expose state to anyone except originator
- **Implementation:** Memento as inner class of Originator (access private fields)

### Visitor
- **Intent:** Define new operations on object structure without modifying classes
- **Double Dispatch:** `element.accept(visitor)` → `visitor.visit(element)` — correct method called based on BOTH types
- **When:** Operations on heterogeneous collections, AST traversal, tax/discount calculation
- **Trade-off:** Easy to add new operations; hard to add new element types
- **Example:** `TaxVisitor.visit(Book)`, `TaxVisitor.visit(Electronics)` — different tax rates

### Null Object
- **Intent:** Provide default no-op behavior instead of null checks
- **When:** Avoid `if (x != null)` everywhere
- **Example:** `NullLogger` implements `Logger` but does nothing — no NPE risk
- **Example:** `GuestUser` implements `User` with no permissions — no null checks

---

## 3.4 MODERN / ENTERPRISE PATTERNS

### Repository Pattern
- Abstract data access behind collection-like interface
- `UserRepository.findById()`, `save()`, `delete()`, `findByEmail()`
- Domain-oriented (vs DAO which is table-oriented)

### DTO (Data Transfer Object)
- Simple data carrier between layers — NO business logic
- `UserRequestDTO`, `UserResponseDTO`
- Prevent entity exposure to API layer

### DAO (Data Access Object)
- Encapsulate all DB access for an entity
- DAO = low-level CRUD mapping, Repository = domain-oriented with business query methods

### Service Layer
- Business logic in service classes, not controllers or repositories
- Orchestrates multiple repositories, applies business rules

### Unit of Work
- Track changes to objects, commit as single transaction
- JPA `EntityManager` is a Unit of Work

### Specification Pattern
- Composable business rules as predicates
- `PremiumUserSpec.and(ActiveUserSpec).and(AgeAbove18Spec)`
- Spring Data `Specification<T>` interface

### Event Sourcing
- Store state as sequence of events, not current state
- Reconstruct state by replaying events
- **When:** Audit trail, financial systems, order history, undo everything

### CQRS (Command Query Responsibility Segregation)
- Separate read model (query) from write model (command)
- Different schemas/databases for reads and writes
- **When:** Read-heavy systems, different read/write patterns

### Object Pool
- Reuse expensive objects instead of creating/destroying
- **When:** DB connections, threads, socket connections
- Pool manages lifecycle: borrow → use → return

### Intercepting Filter
- Chain of filters processing request/response
- **When:** Authentication, logging, compression, encoding
- **Java:** Servlet `Filter`, Spring `OncePerRequestFilter`

---

# SECTION IV: UML DIAGRAMS

---

## 4.1 Class Diagram (Most Important for LLD)
- **Classes:** Name, Attributes (- private, + public, # protected), Methods
- **Relationships:**
  - Association: `A ——→ B` (A knows B)
  - Aggregation: `A ◇——→ B` (A has B, B exists independently)
  - Composition: `A ◆——→ B` (A owns B, B dies with A)
  - Inheritance: `A △——→ B` (A is-a B)
  - Interface Impl: `A △- - -→ B` (A implements B)
  - Dependency: `A - - -→ B` (A uses B temporarily)
- **Multiplicity:** `1`, `0..1`, `1..*`, `0..*`, `n..m`
- **Stereotypes:** `<<interface>>`, `<<abstract>>`, `<<enum>>`, `<<singleton>>`

## 4.2 Sequence Diagram
- Objects as vertical lifelines
- Messages: synchronous (solid arrow), asynchronous (open arrow), return (dashed)
- Activation bars (method execution duration)
- Fragments: `alt` (if-else), `opt` (optional), `loop`, `par` (parallel), `ref` (reference)

## 4.3 State Diagram
- States (rounded rectangles), transitions (arrows with events/guards/actions)
- Initial state (filled circle), final state (circled dot)
- **Use for:** Order lifecycle, elevator, vending machine, traffic light, connection states

## 4.4 Activity Diagram
- Actions, decisions (diamonds), fork/join (parallel), swim lanes (actors)
- **Use for:** Business processes, checkout flows, approval workflows

## 4.5 Use Case Diagram
- Actors, use cases (ovals), system boundary
- `<<include>>` (always), `<<extend>>` (optional)
- **Use for:** Requirements gathering, scope definition

---

# SECTION V: DESIGN PRINCIPLES BEYOND SOLID

---

| Principle | Description |
|---|---|
| **DRY** | Don't Repeat Yourself — extract common logic |
| **KISS** | Keep It Simple, Stupid — simplest solution that works |
| **YAGNI** | You Aren't Gonna Need It — don't build for imaginary future |
| **Composition > Inheritance** | Prefer HAS-A over IS-A — more flexible |
| **Program to Interface** | Depend on abstractions, not concrete classes |
| **Favor Immutability** | Immutable = thread-safe, cacheable, predictable |
| **Law of Demeter** | Don't chain: `a.getB().getC().doX()` — train wreck |
| **Tell, Don't Ask** | Tell objects what to do; don't query state and act on it |
| **Separation of Concerns** | Each module handles one concern |
| **Principle of Least Surprise** | Behavior should match expectations |
| **Fail Fast** | Detect errors early, report immediately |
| **Convention over Configuration** | Sensible defaults, override when needed |
| **Single Level of Abstraction** | Each method should be at one abstraction level |
| **Boy Scout Rule** | Leave code cleaner than you found it |
| **Encapsulate What Varies** | Identify what changes, encapsulate it behind interface |

---

# SECTION VI: CONCURRENCY IN LLD

---

## 6.1 Thread Safety in Design
- **Immutable objects** — best thread safety
- **Synchronized access** — `synchronized`, `ReentrantLock`
- **Thread-safe collections** — `ConcurrentHashMap`, `CopyOnWriteArrayList`
- **Atomic operations** — `AtomicInteger`, `AtomicReference`, CAS
- **ThreadLocal** — per-thread state (user context)

## 6.2 Common Concurrency Patterns in LLD
- **Producer-Consumer** — BlockingQueue between producer and consumer threads
- **Reader-Writer** — `ReadWriteLock` for read-heavy scenarios
- **Double-Checked Locking** — Singleton with `volatile` + `synchronized`
- **Object Pool** — Synchronized borrow/return of reusable objects
- **Semaphore-based Rate Limiting** — control concurrent access count
- **Optimistic Locking** — version field, retry on conflict
- **Pessimistic Locking** — lock resource before modification

## 6.3 Concurrency Questions in LLD Interviews
- "How will you handle concurrent seat booking?" → Optimistic locking + retry
- "Two users editing same document?" → OT (Operational Transform) or CRDT
- "Thread-safe singleton?" → Enum or Bill Pugh
- "Multiple users bidding simultaneously?" → `synchronized` on auction, or CAS
- "Connection pool thread safety?" → `Semaphore` + `BlockingQueue`

---

# SECTION VII: CLASSIC LLD PROBLEMS (Machine Coding)

---

## 7.1 Structural / Real-World Systems

| # | Problem | Key Patterns | Key Classes |
|---|---|---|---|
| 1 | **Parking Lot** | Strategy, Factory, Observer | ParkingLot, Level, Spot, Vehicle, Ticket |
| 2 | **Elevator System** | State, Strategy, Observer | Elevator, ElevatorController, Request, Direction |
| 3 | **Library Management** | Repository, Observer, Strategy | Library, Book, Member, Loan, Fine |
| 4 | **Hotel Booking** | Builder, Strategy, Observer | Hotel, Room, Reservation, Guest, Payment |
| 5 | **Movie Ticket Booking** | Strategy, Observer, Concurrency | Theater, Screen, Show, Seat, Booking |
| 6 | **Ride-Sharing (Uber)** | Strategy, State, Observer | Ride, Driver, Rider, Trip, PricingStrategy |
| 7 | **Food Delivery** | Strategy, State, Observer | Restaurant, Menu, Order, DeliveryAgent, Customer |
| 8 | **E-Commerce (Amazon)** | Strategy, Observer, Composite | Product, Cart, Order, Payment, Shipment |
| 9 | **ATM Machine** | State, Chain of Resp, Strategy | ATM, Account, Transaction, CashDispenser |
| 10 | **Vending Machine** | State, Strategy | VendingMachine, Product, Coin, State |
| 11 | **Traffic Signal System** | State, Observer, Mediator | TrafficLight, Intersection, Signal, Timer |
| 12 | **Airline Management** | Strategy, Observer, Builder | Flight, Aircraft, Booking, Passenger, Seat |
| 13 | **Car Rental System** | Strategy, State, Observer | Vehicle, Rental, Customer, Branch, Invoice |
| 14 | **Hospital Management** | Observer, Strategy, State | Patient, Doctor, Appointment, Ward, Bill |

## 7.2 Game Design

| # | Problem | Key Patterns | Key Classes |
|---|---|---|---|
| 15 | **Chess** | Strategy, Command, Observer | Board, Piece (King, Queen...), Move, Player, Game |
| 16 | **Tic-Tac-Toe** | Strategy, State | Board, Player, Cell, Game, WinStrategy |
| 17 | **Snake & Ladder** | Template Method, Strategy | Board, Player, Snake, Ladder, Dice, Game |
| 18 | **Card Game (Blackjack)** | Strategy, Factory, Observer | Deck, Card, Hand, Player, Dealer, Game |
| 19 | **Minesweeper** | Observer, Composite, Proxy | Board, Cell, Game, MineGenerator |
| 20 | **Sudoku Solver** | Backtracking, Strategy | Board, Cell, Solver, Validator |
| 21 | **Battleship** | State, Strategy, Observer | Board, Ship, Cell, Player, Game |
| 22 | **Ludo** | Template Method, State | Board, Player, Token, Dice, Game |
| 23 | **Connect Four** | Strategy, Observer | Board, Player, Disc, WinChecker |
| 24 | **Bowling Alley** | Strategy, State | Lane, Player, Frame, ScoreCalculator |

## 7.3 Application / Platform Design

| # | Problem | Key Patterns | Key Classes |
|---|---|---|---|
| 25 | **LRU/LFU Cache** | Strategy, Decorator | Cache, Node, DoublyLinkedList, HashMap |
| 26 | **Logger Framework** | Singleton, Chain of Resp, Strategy | Logger, LogLevel, Appender, Formatter |
| 27 | **Pub/Sub System** | Observer, Mediator | Topic, Publisher, Subscriber, Message, Broker |
| 28 | **Task Scheduler (Cron)** | Command, Strategy, Observer | Scheduler, Task, Trigger, TimeExpression |
| 29 | **Rate Limiter** | Strategy, Decorator | RateLimiter, TokenBucket, SlidingWindow, FixedWindow |
| 30 | **File System** | Composite, Iterator, Visitor | FileSystem, File, Directory, Path |
| 31 | **Text Editor** | Command, Memento, State | Editor, Document, Cursor, UndoManager |
| 32 | **Spreadsheet (Excel)** | Observer, Composite, Strategy | Spreadsheet, Cell, Formula, CellReference |
| 33 | **URL Shortener** | Factory, Strategy, Repository | URLService, Encoder, URLMapping |
| 34 | **Social Media (Twitter)** | Observer, Strategy, Decorator | User, Tweet, Feed, Follow, NotificationService |
| 35 | **Stack Overflow** | Observer, Strategy, Composite | User, Question, Answer, Comment, Vote, Tag |
| 36 | **Online Auction** | Observer, State, Strategy | Auction, Bid, Item, User, Timer |
| 37 | **Chat Application** | Mediator, Observer, Command | ChatRoom, User, Message, GroupChat |
| 38 | **Notification System** | Observer, Decorator, Strategy | Notification, Channel (Email, SMS, Push), Template |
| 39 | **Calendar System** | Observer, Builder, Strategy | Calendar, Event, Reminder, RecurrenceRule |
| 40 | **Splitwise (Expense Sharing)** | Strategy, Observer | User, Group, Expense, Balance, Settlement |

## 7.4 Infrastructure / System Design

| # | Problem | Key Patterns | Key Classes |
|---|---|---|---|
| 41 | **Connection Pool** | Singleton, Object Pool, Proxy | ConnectionPool, Connection, PoolConfig |
| 42 | **Thread Pool** | Command, Strategy | ThreadPool, Worker, Task, RejectionPolicy |
| 43 | **In-Memory Database** | Repository, Strategy | Database, Table, Row, Index, Query |
| 44 | **API Rate Limiter** | Strategy, Decorator, Proxy | RateLimiter, Rule, Client, Window |
| 45 | **Circuit Breaker** | State, Proxy | CircuitBreaker, State (Open/Closed/HalfOpen), Metrics |
| 46 | **In-Memory Queue** | Observer, Strategy | Queue, Producer, Consumer, Message |
| 47 | **Key-Value Store** | Strategy, Repository | Store, Entry, TTL, EvictionPolicy |
| 48 | **Plugin System** | Strategy, Factory, Observer | PluginManager, Plugin, PluginLoader, Hook |

---

# SECTION VIII: LLD INTERVIEW FRAMEWORK

---

## Step 1: Clarify Requirements (2-3 min)
- What are the core use cases? (list top 3-5)
- Who are the actors?
- What are the constraints? (concurrency, real-time, scale)
- What features are OUT of scope?
- Any specific design patterns expected?

## Step 2: Identify Core Objects (3-5 min)
- **Nouns → Classes** (User, Order, Payment, Product)
- **Verbs → Methods** (placeOrder, makePayment, addToCart)
- **Adjectives → Enums/States** (OrderStatus: PLACED, CONFIRMED, SHIPPED)
- **Relationships** → Association, Composition, Inheritance

## Step 3: Draw Class Diagram (5 min)
- Classes with key attributes and methods
- Relationships and multiplicity
- Interfaces and abstract classes
- Enums for fixed categories

## Step 4: Apply Design Patterns (5 min)
- Which patterns naturally fit?
- SOLID compliance check
- Extensibility: "What if we add a new payment method?" — should require NO change to existing code

## Step 5: Write Code (15-20 min)
- Start with interfaces/abstract classes (contracts first)
- Implement core business logic
- Handle edge cases
- Show concurrency awareness if applicable
- Use enums for type safety

## Step 6: Discuss Trade-offs (2-3 min)
- Why this pattern over alternatives?
- How would this scale?
- What would change if requirement X was added?
- What's the time/space complexity of key operations?

---

## QUICK REFERENCE: PATTERN SELECTION

| If the problem has... | Use Pattern |
|---|---|
| Multiple algorithms/strategies | **Strategy** |
| Object creation based on input | **Factory / Abstract Factory** |
| Many optional parameters | **Builder** |
| State-dependent behavior | **State** |
| Undo/redo capability | **Command + Memento** |
| Event notification / listeners | **Observer** |
| Pipeline / chain of handlers | **Chain of Responsibility** |
| Part-whole hierarchy (tree) | **Composite** |
| Add features without modifying class | **Decorator** |
| Simplify complex subsystem | **Facade** |
| Control access to object | **Proxy** |
| One global instance | **Singleton** |
| Skeleton algorithm with custom steps | **Template Method** |
| Integrate incompatible interface | **Adapter** |
| Two independent dimensions of variation | **Bridge** |
| Reduce many-to-many to many-to-one | **Mediator** |
| Operations on heterogeneous types | **Visitor** |
| Save/restore state | **Memento** |
| Reuse expensive objects | **Object Pool / Flyweight** |
| Default no-op behavior | **Null Object** |

---

## ANTI-PATTERNS TO AVOID (Mention in interview)

| Anti-Pattern | Problem | Fix |
|---|---|---|
| **God Class** | One class does everything | SRP — split responsibilities |
| **Spaghetti Code** | No structure, tangled dependencies | Layered architecture, patterns |
| **Shotgun Surgery** | One change requires editing many classes | High cohesion, encapsulation |
| **Feature Envy** | Method uses another class's data more than its own | Move method to that class |
| **Primitive Obsession** | Use primitives instead of small objects | Value Objects (`Money`, `Email`, `PhoneNumber`) |
| **Data Clumps** | Same group of fields appears everywhere | Extract into class |
| **Refused Bequest** | Subclass doesn't use inherited methods | Composition over inheritance |
| **Singleton Overuse** | Everything is a singleton | Use DI, limit to true singletons |
| **Leaky Abstraction** | Implementation details leak through interface | Better encapsulation |
| **Circular Dependency** | A depends on B depends on A | Introduce interface, mediator |

---

**Total: 8 Sections | 48 LLD Problems | 23+ Design Patterns | All Principles & Anti-Patterns**

**Study Plan for LLD:**
1. SOLID + OOP + Principles — 3 days
2. All Design Patterns with Java code — 1 week (2-3 patterns/day)
3. UML Diagrams — 1 day
4. Practice LLD Problems — 2-3 weeks (2 problems/day)
5. Concurrency in design — 2 days
6. Mock Machine Coding — ongoing

**Total: ~4-5 weeks (2-3 hrs/day)** 🚀

