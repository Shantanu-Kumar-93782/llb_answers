# JAVA — Complete FAANG Interview Guide (5 YOE)

> Every Java concept from JVM internals to Java 21 features.
> Covers everything asked in FAANG Java/backend interviews.

---

# SECTION I: JVM ARCHITECTURE & INTERNALS

---

## 1.1 JVM Components
```
Source Code (.java)
    ↓ javac (Compiler)
Bytecode (.class)
    ↓
┌─────────────────────────────────────────────┐
│                   JVM                        │
│  ┌───────────────────────────────────────┐  │
│  │         Class Loader Subsystem         │  │
│  │  Bootstrap → Extension → Application   │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │         Runtime Memory Areas           │  │
│  │  Heap | Stack | Method Area | PC | NMS │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │          Execution Engine              │  │
│  │  Interpreter → JIT (C1/C2) → GC       │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │     Native Method Interface (JNI)      │  │
│  └───────────────────────────────────────┘  │
└─────────────────────────────────────────────┘
```

## 1.2 Class Loading
- **Loading:** Find and load `.class` file bytecode
- **Linking:**
  - Verification — bytecode is valid
  - Preparation — allocate memory for static fields (default values)
  - Resolution — symbolic references → direct references
- **Initialization:** Execute static blocks, initialize static fields

### ClassLoader Hierarchy
| ClassLoader | Loads From | Example |
|---|---|---|
| **Bootstrap** | `$JAVA_HOME/lib` | `java.lang.*`, `java.util.*` |
| **Extension/Platform** | `$JAVA_HOME/lib/ext` | `javax.*` extensions |
| **Application/System** | Classpath | Your application classes |
| **Custom** | Custom location | Plugin systems, app servers |

- **Delegation Model:** Child asks parent first → parent tries → if parent fails, child loads
- **Interview Q:** "Can two classes with same name coexist?" → Yes, if loaded by different classloaders

## 1.3 Runtime Memory Areas

### Heap (Shared across threads)
```
Heap
├── Young Generation
│   ├── Eden Space — new objects allocated here
│   ├── Survivor 0 (S0 / From)
│   └── Survivor 1 (S1 / To)
└── Old Generation (Tenured) — long-lived objects promoted here
```
- `-Xms` (initial heap), `-Xmx` (max heap)
- Objects move: Eden → Survivor (after Minor GC) → Old Gen (after threshold age)

### Stack (Per thread)
- Stack frames: local variables, operand stack, frame data
- `-Xss` to set stack size
- `StackOverflowError` — too deep recursion
- Primitive values and references stored here (objects on heap)

### Method Area / Metaspace
- Class metadata, static variables, constant pool, method bytecode
- **Metaspace** (Java 8+) — uses native memory, replaces PermGen
- `-XX:MaxMetaspaceSize` to limit
- `OutOfMemoryError: Metaspace` — too many classes loaded (common in app servers)

### PC Register (Per thread)
- Address of current executing instruction

### Native Method Stack (Per thread)
- For native (C/C++) method calls via JNI

## 1.4 JIT Compilation
- **Interpreter:** Executes bytecode line by line (slow, no startup cost)
- **JIT Compiler:** Compiles hot methods to native code at runtime
  - **C1 (Client):** Fast compilation, moderate optimization
  - **C2 (Server):** Slow compilation, aggressive optimization
  - **Tiered Compilation** (default): Start with C1 → promote to C2 for hot methods
- **HotSpot Detection:** Method invocation counter + back-edge counter
- **Optimizations:** Inlining, loop unrolling, dead code elimination, escape analysis
- **Escape Analysis:** If object doesn't escape method → allocate on stack (not heap)

## 1.5 Garbage Collection

### GC Basics
- **Reachability Analysis:** Start from GC roots, mark all reachable objects, sweep unreachable
- **GC Roots:** Local variables, static fields, active threads, JNI references, synchronized monitors
- **Stop-the-World (STW):** All application threads paused during GC

### GC Algorithms

| GC | Type | Best For | Pause | Throughput |
|---|---|---|---|---|
| **Serial** | Single-threaded | Small apps, client | High | Low |
| **Parallel** | Multi-threaded | Batch processing, throughput | Medium | High |
| **G1** | Region-based | General purpose (default ≥ Java 9) | Low | Good |
| **ZGC** | Concurrent, region | Ultra-low latency (Java 11+) | < 1ms | Good |
| **Shenandoah** | Concurrent | Low latency (OpenJDK) | < 10ms | Good |

### G1 GC Deep Dive (Most asked)
- Heap divided into equal-sized **regions** (~2048 regions)
- Regions classified: Eden, Survivor, Old, Humongous (objects > 50% region)
- **Concurrent Marking:** Identifies garbage regions concurrently
- **Mixed Collection:** Collects young + some old regions with most garbage
- **Pause Target:** `-XX:MaxGCPauseMillis=200` (default)
- **String Deduplication:** `-XX:+UseStringDeduplication`

### GC Tuning Parameters
```
-Xms512m                        # Initial heap
-Xmx2g                          # Max heap
-XX:+UseG1GC                    # Use G1
-XX:MaxGCPauseMillis=200         # Target pause
-XX:NewRatio=2                   # Old:Young ratio
-XX:SurvivorRatio=8              # Eden:Survivor ratio
-XX:+HeapDumpOnOutOfMemoryError  # Dump on OOM
-XX:+PrintGCDetails              # GC logging
-Xlog:gc*                        # Unified logging (Java 9+)
```

## 1.6 Memory Leaks
- **Causes:**
  - Unclosed resources (streams, connections, ResultSets)
  - Static collections growing unbounded
  - Listener/callback not deregistered
  - `ThreadLocal` not `remove()`-ed in thread pools
  - Inner class holding reference to outer class
  - Interned strings (before Java 7)
  - Custom classloader leaks
- **Detection:** VisualVM, JConsole, MAT (Memory Analyzer Tool), jmap, jstat
- **Prevention:** try-with-resources, WeakReference, proper lifecycle management

## 1.7 Reference Types
| Type | GC Behavior | Use Case |
|---|---|---|
| **Strong** | Never collected while reachable | Normal references |
| **Weak** (`WeakReference`) | Collected on next GC | WeakHashMap, caches |
| **Soft** (`SoftReference`) | Collected when memory low | Memory-sensitive caches |
| **Phantom** (`PhantomReference`) | Already finalized, enqueued | Post-mortem cleanup |

---

# SECTION II: JAVA CORE LANGUAGE DEEP DIVE

---

## 2.1 Data Types & Memory

### Primitives
| Type | Size | Default | Range |
|---|---|---|---|
| `byte` | 1B | 0 | -128 to 127 |
| `short` | 2B | 0 | -32,768 to 32,767 |
| `int` | 4B | 0 | -2^31 to 2^31-1 |
| `long` | 8B | 0L | -2^63 to 2^63-1 |
| `float` | 4B | 0.0f | IEEE 754 |
| `double` | 8B | 0.0d | IEEE 754 |
| `char` | 2B | '\u0000' | 0 to 65,535 (Unicode) |
| `boolean` | ~1B | false | true/false |

### Autoboxing / Unboxing
- `int` ↔ `Integer`, `double` ↔ `Double`, etc.
- **Integer Cache:** -128 to 127 cached → `Integer.valueOf(127) == Integer.valueOf(127)` is `true`
- **Pitfall:** `Integer a = null; int b = a;` → `NullPointerException`
- **Performance:** Avoid autoboxing in loops — use primitives

### Wrapper Class Caching
```java
Integer.valueOf(127) == Integer.valueOf(127)    // true (cached)
Integer.valueOf(128) == Integer.valueOf(128)    // false (new objects)
new Integer(127) == new Integer(127)            // false (always new)
```

## 2.2 String Internals

### Immutability
- `String` is `final` class, backed by `final char[]` (Java 8) / `final byte[]` (Java 9+, compact strings)
- **Why immutable?**
  1. Thread-safe without synchronization
  2. Hashcode cached (for HashMap keys)
  3. String pool possible (memory savings)
  4. Security (class loading, network connections use strings)

### String Pool
- Literal strings automatically interned: `"hello"` → pool
- `new String("hello")` → heap (separate from pool)
- `s.intern()` → returns pool reference (or adds to pool)
- **Java 7+:** String pool moved from PermGen to Heap

### String vs StringBuilder vs StringBuffer
| | String | StringBuilder | StringBuffer |
|---|---|---|---|
| Mutability | Immutable | Mutable | Mutable |
| Thread-safe | Yes (immutable) | No | Yes (synchronized) |
| Performance | Slow (creates new) | Fastest | Slower than SB |
| **Use when** | Few modifications | Single-threaded concat | Multi-threaded concat |

### String Concatenation Internals
- `+` operator: Compiler uses `StringBuilder.append()` (Java 8), `invokedynamic` + `StringConcatFactory` (Java 9+)
- **Pitfall:** In loops, `+` creates new StringBuilder per iteration → use explicit `StringBuilder`

## 2.3 Keywords Deep Dive

### `final`
- **Variable:** Cannot reassign (constant). For objects, reference is final, not object state.
- **Method:** Cannot override in subclass.
- **Class:** Cannot extend. e.g., `String`, `Integer`.
- **Effectively final:** Variable never reassigned after init → usable in lambdas.

### `static`
- **Field:** One copy per class (not per instance). Shared.
- **Method:** Called on class, not instance. Cannot access `this`.
- **Block:** Executed once when class loaded. For complex static initialization.
- **Inner Class:** Does NOT hold reference to outer class (unlike non-static inner class).
- **Import:** `import static java.lang.Math.PI;`

### `volatile`
- **Guarantees:** Visibility (reads from main memory, not thread cache)
- **Does NOT guarantee:** Atomicity (`volatile int count; count++` is still NOT atomic — read + modify + write)
- **Happens-before:** Write to volatile → happens-before subsequent read of same volatile
- **Use:** Flags (`volatile boolean running`), double-checked locking with `volatile` singleton
- **vs `synchronized`:** Volatile = visibility only, synchronized = visibility + atomicity + mutual exclusion

### `transient`
- Field excluded from serialization
- **Use:** Sensitive data (passwords), derived/computed fields, non-serializable references

### `synchronized`
- **Method level:** Locks `this` (instance method) or `Class` object (static method)
- **Block level:** `synchronized(lockObject) { ... }` — fine-grained
- **Reentrant:** Same thread can acquire same lock multiple times
- **Happens-before:** Unlock → happens-before subsequent lock of same monitor

### `native`
- Method implemented in C/C++ via JNI
- e.g., `System.currentTimeMillis()`, `Object.hashCode()`, `Thread.start()`

## 2.4 Object Class Methods (Every Java object inherits these)
- `equals(Object o)` — logical equality
- `hashCode()` — hash for HashMap/HashSet
- `toString()` — string representation
- `clone()` — create copy (needs `Cloneable`)
- `getClass()` — runtime class info
- `finalize()` — called before GC (deprecated Java 9+, removed concept Java 18+)
- `wait()`, `notify()`, `notifyAll()` — inter-thread communication
- **Contract:** `equals` + `hashCode` must be consistent

## 2.5 Equals & HashCode Contract
- **Rule 1:** If `a.equals(b)` then `a.hashCode() == b.hashCode()`
- **Rule 2:** If `a.hashCode() != b.hashCode()` then `!a.equals(b)`
- **Rule 3:** Same hashCode does NOT mean equals (hash collision)
- **Must override both or neither** — otherwise `HashMap`/`HashSet` breaks
- **Best practice:** Use `Objects.equals()`, `Objects.hash()`, or IDE/Lombok generation
- **Immutable fields only** in hashCode — mutable fields can change hash after insertion

## 2.6 Comparable vs Comparator
```java
// Comparable — natural ordering (single)
public class Employee implements Comparable<Employee> {
    public int compareTo(Employee o) { return this.name.compareTo(o.name); }
}

// Comparator — custom ordering (multiple)
Comparator<Employee> byAge = Comparator.comparing(Employee::getAge);
Comparator<Employee> bySalaryDesc = Comparator.comparing(Employee::getSalary).reversed();
Comparator<Employee> byAgeThenName = Comparator.comparing(Employee::getAge)
                                               .thenComparing(Employee::getName);
```

## 2.7 Generics

### Type Erasure
- Generics exist only at compile-time. At runtime, `List<String>` becomes `List<Object>`.
- **Cannot do:** `new T()`, `new T[]`, `instanceof T`, `catch T`, `T.class`
- **Can do:** `Class<T> clazz` parameter → `clazz.newInstance()` (workaround)

### Bounded Types
```java
<T extends Comparable<T>>          // Upper bound — T must implement Comparable
<T extends Number & Serializable>  // Multiple bounds (class first, then interfaces)
<? extends Number>                 // Upper bounded wildcard (producer/read)
<? super Integer>                  // Lower bounded wildcard (consumer/write)
<?>                                // Unbounded wildcard
```

### PECS — Producer Extends, Consumer Super
```java
// READING from collection → extends (producer gives data)
void printAll(List<? extends Number> list) { for (Number n : list) print(n); }

// WRITING to collection → super (consumer accepts data)
void addIntegers(List<? super Integer> list) { list.add(42); }
```

### Type Inference
- Diamond operator: `List<String> list = new ArrayList<>();` (Java 7+)
- `var list = new ArrayList<String>();` (Java 10+)

## 2.8 Exceptions

### Hierarchy
```
Throwable
├── Error (don't catch)
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   ├── VirtualMachineError
│   └── ClassFormatError
└── Exception
    ├── Checked (compile-time, must handle/declare)
    │   ├── IOException
    │   ├── SQLException
    │   ├── ClassNotFoundException
    │   ├── InterruptedException
    │   └── ReflectiveOperationException
    └── RuntimeException (unchecked)
        ├── NullPointerException
        ├── ArrayIndexOutOfBoundsException
        ├── IllegalArgumentException
        ├── IllegalStateException
        ├── ClassCastException
        ├── ArithmeticException
        ├── UnsupportedOperationException
        └── ConcurrentModificationException
```

### Best Practices
- Use specific exceptions, not `catch (Exception e)`
- Don't use exceptions for control flow
- Always log root cause: `throw new ServiceException("msg", cause)`
- Use `try-with-resources` for `AutoCloseable` resources
- Create custom exceptions for domain: `OrderNotFoundException extends RuntimeException`
- Don't catch `Error` — JVM is likely in unrecoverable state
- Prefer unchecked exceptions for programming errors; checked for recoverable conditions
- Multi-catch: `catch (IOException | SQLException e)`
- **Finally block:** Always executes (even with return in try), except `System.exit()`

## 2.9 Serialization

### `Serializable`
- Marker interface — no methods
- `serialVersionUID` — version control for compatibility
- `transient` fields excluded
- **Deserialization:** Does NOT call constructor — allocates memory directly
- `readResolve()` — return custom instance (protect singleton)
- `writeReplace()` — substitute object before serialization

### `Externalizable`
- Full custom control: `writeExternal(ObjectOutput)`, `readExternal(ObjectInput)`
- DOES call no-arg constructor during deserialization
- More performant than `Serializable` (no reflection)

### Security Risks
- Deserialization attacks — arbitrary code execution
- **Prevention:** Validation in `readObject()`, `ObjectInputFilter` (Java 9+), avoid deserializing untrusted data
- **Modern alternative:** JSON (Jackson, Gson), Protocol Buffers, Avro

## 2.10 Reflection
- Inspect/modify classes, fields, methods, constructors at runtime
- `Class.forName("com.example.User")` → load class
- `getDeclaredFields()`, `getDeclaredMethods()`, `getDeclaredConstructors()`
- `setAccessible(true)` → access private members
- **Used by:** Spring (DI, AOP), Hibernate (entity mapping), Jackson (JSON), JUnit (test discovery)
- **Performance:** 10-100x slower than direct access
- **Security:** Can break encapsulation, singleton, immutability
- **Module system (Java 9+):** `--add-opens` needed for deep reflection

## 2.11 Annotations
### Built-in
- `@Override`, `@Deprecated`, `@SuppressWarnings`, `@FunctionalInterface`
- `@SafeVarargs` — suppress heap pollution warning

### Meta-annotations (Annotations on annotations)
- `@Target` — where it can be applied (METHOD, FIELD, TYPE, PARAMETER, etc.)
- `@Retention` — how long it's kept (SOURCE, CLASS, RUNTIME)
- `@Documented` — include in Javadoc
- `@Inherited` — annotation inherited by subclasses
- `@Repeatable` — can be used multiple times on same element

### Custom Annotations
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface RateLimit {
    int maxRequests() default 100;
    int windowSeconds() default 60;
}
```

## 2.12 Enums
- Type-safe constants, can have fields, methods, constructors
- Implicitly `final`, extends `java.lang.Enum`
- **Enum with behavior:**
  ```java
  enum Operation {
      ADD { public int apply(int a, int b) { return a + b; } },
      SUB { public int apply(int a, int b) { return a - b; } };
      public abstract int apply(int a, int b);
  }
  ```
- **Enum Singleton:** Best singleton implementation (serialization + reflection safe)
- `values()`, `valueOf(String)`, `name()`, `ordinal()`
- **EnumSet / EnumMap:** Fastest set/map for enum keys (bit vector / array based)

## 2.13 Inner Classes
| Type | Has outer ref? | Can have static members? | Where declared |
|---|---|---|---|
| **Non-static inner** | Yes | No (Java 16+: yes) | Inside class |
| **Static nested** | No | Yes | Inside class with `static` |
| **Local** | Yes (if non-static method) | No | Inside method |
| **Anonymous** | Yes (if non-static context) | No | Inline |

- **Memory leak risk:** Non-static inner class holds reference to outer instance
- **Prefer static nested classes** unless outer reference is needed

## 2.14 Records (Java 14+)
```java
public record Point(int x, int y) {
    // Compact constructor for validation
    public Point {
        if (x < 0 || y < 0) throw new IllegalArgumentException("Negative");
    }
    // Custom method
    public double distanceTo(Point other) {
        return Math.sqrt(Math.pow(x - other.x, 2) + Math.pow(y - other.y, 2));
    }
}
```
- Auto-generates: `equals()`, `hashCode()`, `toString()`, accessors (`x()`, `y()`)
- Immutable, final class, final fields
- **Cannot:** Extend another class, be extended (implicitly final)
- **Can:** Implement interfaces, have static methods/fields, custom constructors

## 2.15 Sealed Classes (Java 17+)
```java
public sealed class Shape permits Circle, Square, Triangle {}
public final class Circle extends Shape {}           // cannot be extended
public non-sealed class Square extends Shape {}      // can be extended by anyone
public sealed class Triangle extends Shape permits RightTriangle {} // restricted
```
- **Exhaustive switch:** Compiler knows all subtypes → no default needed
- **Pattern matching for switch (Java 21):**
  ```java
  return switch (shape) {
      case Circle c -> Math.PI * c.radius() * c.radius();
      case Square s -> s.side() * s.side();
      case Triangle t -> 0.5 * t.base() * t.height();
  };
  ```

## 2.16 Pattern Matching
- **`instanceof` (Java 16+):**
  ```java
  if (obj instanceof String s && s.length() > 5) { use(s); }
  ```
- **Switch (Java 21):**
  ```java
  return switch (obj) {
      case Integer i -> "int: " + i;
      case String s when s.length() > 5 -> "long string";
      case String s -> "short string";
      case null -> "null";
      default -> "other";
  };
  ```
- **Guarded patterns:** `case String s when s.length() > 5` (replaces `&&` guard)
- **Record patterns (Java 21):**
  ```java
  if (obj instanceof Point(int x, int y)) { use(x, y); }
  ```

---

# SECTION III: JAVA COLLECTIONS FRAMEWORK

---

## 3.1 Collection Hierarchy
```
Iterable<T>
  └── Collection<T>
        ├── List<T> (ordered, duplicates allowed)
        │     ├── ArrayList     — dynamic array, O(1) get, O(n) insert
        │     ├── LinkedList    — doubly-linked, O(1) add/remove ends, O(n) get
        │     ├── Vector        — synchronized ArrayList (LEGACY)
        │     ├── Stack         — LIFO (LEGACY — use ArrayDeque)
        │     └── CopyOnWriteArrayList — snapshot iteration, thread-safe
        │
        ├── Set<T> (no duplicates)
        │     ├── HashSet       — O(1), unordered, backed by HashMap
        │     ├── LinkedHashSet — O(1), insertion order preserved
        │     ├── TreeSet       — O(log n), sorted (Red-Black Tree)
        │     ├── EnumSet       — bit-vector, fastest for enums
        │     └── CopyOnWriteArraySet — thread-safe set
        │
        └── Queue<T> / Deque<T>
              ├── PriorityQueue      — min-heap, O(log n) add/poll
              ├── ArrayDeque         — circular array, stack+queue
              ├── LinkedList         — also Deque
              ├── ConcurrentLinkedQueue — lock-free
              ├── LinkedBlockingQueue   — bounded, blocking
              ├── ArrayBlockingQueue    — bounded, blocking
              ├── PriorityBlockingQueue — priority + blocking
              ├── DelayQueue           — elements available after delay
              ├── SynchronousQueue     — zero-capacity, hand-off
              └── LinkedTransferQueue  — transfer + queue

Map<K,V> (NOT part of Collection)
  ├── HashMap         — O(1) avg, unordered, allows null key
  ├── LinkedHashMap   — O(1), insertion/access order
  ├── TreeMap         — O(log n), sorted keys (NavigableMap)
  ├── Hashtable       — synchronized (LEGACY)
  ├── ConcurrentHashMap — concurrent, segment/node locking
  ├── ConcurrentSkipListMap — concurrent + sorted
  ├── WeakHashMap     — weak key references, GC can reclaim
  ├── IdentityHashMap — uses == instead of equals
  └── EnumMap         — array-backed, fastest for enum keys
```

## 3.2 ArrayList Internals
- Backed by `Object[]` array
- Default capacity: 10
- Growth: 50% increase (`newCapacity = oldCapacity + (oldCapacity >> 1)`)
- `ensureCapacity(n)` — avoid repeated resizing
- `trimToSize()` — shrink to actual size
- **NOT thread-safe** — use `Collections.synchronizedList()` or `CopyOnWriteArrayList`
- **Random access:** O(1). **Insert/delete middle:** O(n) (shift elements)

## 3.3 LinkedList Internals
- Doubly-linked list, implements `List` + `Deque`
- No random access — O(n) for `get(i)`
- O(1) add/remove at head/tail
- More memory overhead (prev + next pointers per node)
- **When to use:** Frequent add/remove at ends, iterator-based removal

## 3.4 HashMap Internals (FAANG Favorite ★)
- **Structure:** `Node<K,V>[] table` — array of buckets
- **Bucket:** LinkedList (≤ 8 nodes) → Red-Black Tree (> 8 nodes, Java 8+)
  - **Treeify threshold:** 8. **Untreeify threshold:** 6.
  - Tree only if table capacity ≥ 64 (otherwise resize instead)
- **Hashing:**
  ```java
  static int hash(Object key) {
      int h;
      return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16); // spread high bits
  }
  int index = (n - 1) & hash;  // n = table.length (always power of 2)
  ```
- **Load Factor:** 0.75 (default) — trade-off between space and collision probability
- **Resize:** When `size > capacity * loadFactor`, double capacity, rehash all entries
- **Null handling:** One null key (bucket 0), multiple null values
- **NOT thread-safe** — race condition can cause infinite loop (Java 7 linked list) or data loss

### HashMap Put Flow
1. Calculate `hash(key)`
2. Find bucket: `(n-1) & hash`
3. If bucket empty → insert new node
4. If bucket has entries → compare keys (`hash == hash && (key == k || key.equals(k))`)
   - Found → replace value
   - Not found → append to list/tree
5. If size > threshold → resize (double)
6. If bucket list > 8 → treeify (if table ≥ 64)

## 3.5 ConcurrentHashMap Internals
- **Java 7:** 16 Segments, each with own lock → 16 concurrent writes
- **Java 8+:** Node-level locking
  - CAS for first node in empty bucket
  - `synchronized` on first node for non-empty bucket
  - No segment concept — finer granularity
- **No null keys or values** (unlike HashMap) — prevents ambiguity in `get()`
- **Size:** Uses `CounterCell[]` (like LongAdder) for concurrent counting
- **Weakly consistent iterators** — no `ConcurrentModificationException`
- **Atomic operations:** `putIfAbsent()`, `compute()`, `computeIfAbsent()`, `merge()`, `replace()`

## 3.6 TreeMap / TreeSet
- Based on **Red-Black Tree** (self-balancing BST)
- O(log n) for get, put, remove, containsKey
- Implements `NavigableMap` → `floorKey()`, `ceilingKey()`, `headMap()`, `tailMap()`, `subMap()`
- Keys must be `Comparable` OR provide `Comparator`
- **No null keys** (NPE on comparison)

## 3.7 LinkedHashMap — LRU Cache ★
```java
class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private final int capacity;
    public LRUCache(int capacity) {
        super(capacity, 0.75f, true); // true = access-order
        this.capacity = capacity;
    }
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > capacity;
    }
}
```
- `accessOrder = true` → move accessed entry to end
- `removeEldestEntry()` → called after every `put()`

## 3.8 Collections Utility Methods
- `Collections.unmodifiableList/Set/Map()` — read-only wrapper
- `Collections.synchronizedList/Set/Map()` — thread-safe wrapper
- `Collections.singletonList()`, `Collections.emptyList()`
- `List.of()`, `Set.of()`, `Map.of()` — immutable factories (Java 9+)
- `List.copyOf()`, `Set.copyOf()` — immutable copies (Java 10+)

## 3.9 When to Use What (Quick Reference)

| Need | Best Choice | Why |
|---|---|---|
| Random access by index | `ArrayList` | O(1) get |
| Frequent add/remove at ends | `ArrayDeque` | O(1) amortized |
| Unique elements, fast lookup | `HashSet` | O(1) |
| Unique elements, sorted | `TreeSet` | O(log n), NavigableSet |
| Unique elements, insertion order | `LinkedHashSet` | O(1) + order |
| Key-value, fast lookup | `HashMap` | O(1) |
| Key-value, sorted keys | `TreeMap` | O(log n), NavigableMap |
| Key-value, insertion order | `LinkedHashMap` | O(1) + order |
| Thread-safe map | `ConcurrentHashMap` | Concurrent reads/writes |
| Thread-safe list (read-heavy) | `CopyOnWriteArrayList` | Snapshot iteration |
| Priority processing | `PriorityQueue` | O(log n) add/poll |
| Stack (LIFO) | `ArrayDeque` | Faster than `Stack` |
| Queue (FIFO) | `ArrayDeque` or `LinkedList` | O(1) |
| Blocking queue | `LinkedBlockingQueue` | Producer-consumer |
| LRU Cache | `LinkedHashMap` (access-order) | Built-in eviction |
| Enum keys | `EnumMap` / `EnumSet` | Fastest (bit-vector/array) |

---

# SECTION IV: CONCURRENCY & MULTITHREADING

---

## 4.1 Thread Creation
```java
// 1. Extend Thread (bad — single inheritance)
class MyThread extends Thread { public void run() { } }

// 2. Implement Runnable (better — no return value)
Runnable task = () -> System.out.println("Running");
new Thread(task).start();

// 3. Implement Callable<V> (best — returns result, throws exception)
Callable<Integer> task = () -> { return 42; };
Future<Integer> future = executor.submit(task);
int result = future.get(); // blocking
```

## 4.2 Thread Lifecycle
```
NEW ──start()──→ RUNNABLE ──scheduler──→ RUNNING
                     ↑                       │
                     │         ┌─────────────┤
                     │         ↓             ↓
                 TIMED_WAITING  WAITING    BLOCKED
                 (sleep, wait   (wait,     (synchronized
                  with timeout,  join,      lock contention)
                  parkNanos)     park)
                     │         │             │
                     └─────────┴─────────────┘
                               ↓
                          TERMINATED
```

## 4.3 Synchronization Primitives

### `synchronized`
- Intrinsic lock / monitor lock
- Reentrant (same thread can acquire multiple times)
- Method-level: locks `this` (instance) or `Class` (static)
- Block-level: `synchronized(lockObj) { }` — finer control
- **Guarantees:** Atomicity + Visibility + Ordering

### `ReentrantLock`
- Explicit lock with more features than `synchronized`
- `lock()`, `unlock()`, `tryLock()`, `tryLock(timeout)`, `lockInterruptibly()`
- **Fairness:** `new ReentrantLock(true)` — longest-waiting thread gets lock
- **Condition:** `lock.newCondition()` → `await()`, `signal()`, `signalAll()` (replaces wait/notify)
- **ALWAYS unlock in finally block**

### `ReadWriteLock`
- Multiple concurrent readers OR one exclusive writer
- `rwLock.readLock().lock()` / `rwLock.writeLock().lock()`
- **When:** Read-heavy workloads (caches, config)

### `StampedLock` (Java 8+)
- Optimistic reading: `long stamp = lock.tryOptimisticRead()`
- If no write occurred: `lock.validate(stamp)` → proceed without locking
- **Faster** than ReadWriteLock for read-heavy scenarios
- **NOT reentrant**

### `Semaphore`
- Controls access to N resources
- `acquire()` (blocks if permits=0), `release()`
- **When:** Connection pool, rate limiting, bounded resource access
- `new Semaphore(5)` → 5 concurrent accessors

### `CountDownLatch`
- Wait for N events to complete
- `countDown()` from workers, `await()` from waiter
- **One-time use** — cannot reset
- **When:** Wait for all services to initialize, wait for N tasks to complete

### `CyclicBarrier`
- N threads wait for each other at a barrier point
- **Reusable** — resets after all threads arrive
- Optional barrier action (runs when all threads meet)
- **When:** Parallel computation phases, matrix processing rounds

### `Phaser`
- Flexible replacement for CyclicBarrier + CountDownLatch
- Dynamic registration/deregistration of parties
- Phase advancement

### `Exchanger<V>`
- Two threads exchange data at a synchronization point
- `V other = exchanger.exchange(myData);` — blocks until partner arrives

## 4.4 Atomic Variables & CAS
- `AtomicInteger`, `AtomicLong`, `AtomicBoolean`, `AtomicReference<V>`
- `AtomicIntegerArray`, `AtomicReferenceArray`
- **CAS (Compare-And-Swap):** `compareAndSet(expected, new)` — lock-free, non-blocking
- **ABA Problem:** Value changes A→B→A, CAS thinks no change. Fix: `AtomicStampedReference`
- **`LongAdder`** (Java 8+): Better than `AtomicLong` under high contention — uses striped cells
- **`LongAccumulator`:** Generalized LongAdder with custom accumulation function

## 4.5 Executor Framework
```
Executor (execute(Runnable))
  └── ExecutorService (submit, shutdown, invokeAll, invokeAny)
        ├── ThreadPoolExecutor
        │     Constructor: (corePoolSize, maxPoolSize, keepAliveTime, unit, workQueue, 
        │                   threadFactory, rejectionHandler)
        │     
        │     Work Queue Types:
        │     ├── LinkedBlockingQueue — unbounded (Fixed/Single pool)
        │     ├── SynchronousQueue — zero-capacity (Cached pool)
        │     ├── ArrayBlockingQueue — bounded
        │     └── PriorityBlockingQueue — priority-based
        │     
        │     Rejection Policies:
        │     ├── AbortPolicy (default) — throws RejectedExecutionException
        │     ├── CallerRunsPolicy — caller thread executes task
        │     ├── DiscardPolicy — silently discard
        │     └── DiscardOldestPolicy — discard oldest, retry
        │
        ├── ScheduledThreadPoolExecutor
        │     schedule(), scheduleAtFixedRate(), scheduleWithFixedDelay()
        │
        └── ForkJoinPool (Java 7+)
              Work-stealing: idle threads steal from busy threads' deques
              RecursiveTask<V> (returns result), RecursiveAction (no result)
              compute() → fork() + join()
```

### Factory Methods (Executors)
| Method | Core | Max | Queue | Use Case |
|---|---|---|---|---|
| `newFixedThreadPool(n)` | n | n | Unbounded | Known workload |
| `newCachedThreadPool()` | 0 | MAX | Synchronous | Bursty, short tasks |
| `newSingleThreadExecutor()` | 1 | 1 | Unbounded | Sequential tasks |
| `newScheduledThreadPool(n)` | n | MAX | Delayed | Periodic/delayed tasks |
| `newWorkStealingPool()` | CPU cores | CPU cores | ForkJoin | Parallel computation |

**⚠️ Avoid `newFixedThreadPool` and `newCachedThreadPool` in production:**
- Fixed: Unbounded queue → OOM
- Cached: Unbounded threads → OOM
- **Use `ThreadPoolExecutor` directly with bounded queue and proper rejection policy**

## 4.6 CompletableFuture (Java 8+)
```java
// Basic async
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> fetchData());

// Chaining
future.thenApply(data -> parse(data))        // transform (same thread)
      .thenApplyAsync(parsed -> enrich(parsed)) // transform (async)
      .thenAccept(result -> save(result))     // consume (no return)
      .exceptionally(ex -> handleError(ex));  // error handling

// Combining
CompletableFuture.allOf(f1, f2, f3).thenRun(() -> allDone());
CompletableFuture.anyOf(f1, f2, f3).thenAccept(first -> useFirst(first));

// Two futures
f1.thenCombine(f2, (r1, r2) -> combine(r1, r2));    // both complete
f1.thenCompose(r1 -> fetchMore(r1));                  // sequential (flatMap)

// Error handling
future.handle((result, ex) -> {                       // both cases
    if (ex != null) return fallback;
    return result;
});
```

## 4.7 ThreadLocal
- Per-thread storage — each thread has its own copy
- **Use:** User context, DB connection, transaction context, date formatters
- **⚠️ Memory leak in thread pools:** Thread survives, ThreadLocal value not cleaned
- **Always call `remove()`** in finally block or after use
- `InheritableThreadLocal` — child threads inherit parent's value
- **Java 21+:** `ScopedValue` — safer alternative for virtual threads

## 4.8 Common Concurrency Problems & Solutions

| Problem | Description | Solution |
|---|---|---|
| **Deadlock** | Threads waiting for each other's locks circularly | Lock ordering, tryLock with timeout, lock-free algorithms |
| **Livelock** | Threads keep changing state in response to each other | Add randomness/backoff |
| **Starvation** | Thread never gets CPU/lock | Fair locks, priority adjustment |
| **Race Condition** | Check-then-act without atomicity | synchronized, atomic ops, CAS |
| **Visibility** | Thread reads stale cached value | volatile, synchronized, happens-before |
| **False Sharing** | Different threads modify adjacent cache-line fields | `@Contended`, padding |
| **Priority Inversion** | Low-priority thread holds lock needed by high-priority | Priority inheritance |

## 4.9 Virtual Threads (Java 21+ / Project Loom) ★
```java
// Create
Thread.ofVirtual().start(() -> doWork());
Thread vt = Thread.ofVirtual().name("vt-", 0).factory().newThread(() -> doWork());

// Executor
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    IntStream.range(0, 100_000).forEach(i ->
        executor.submit(() -> blockingIOCall())
    );
}
```
- **Platform threads:** 1:1 with OS thread (~2MB stack, ~thousands max)
- **Virtual threads:** Many:1 with carrier (platform) thread (~KB stack, millions possible)
- **Best for:** I/O-bound tasks (HTTP calls, DB queries, file I/O)
- **NOT for:** CPU-bound tasks (use ForkJoinPool)
- **Pinning:** Virtual thread pinned to carrier if inside `synchronized` block or native call
  - **Fix:** Replace `synchronized` with `ReentrantLock`
- **ScopedValue** replaces ThreadLocal for virtual threads
- **Structured Concurrency** (preview): `StructuredTaskScope` — manage child tasks lifecycle

## 4.10 Concurrent Collections Summary

| Collection | Thread-Safety Mechanism | Iteration | Nulls |
|---|---|---|---|
| `ConcurrentHashMap` | CAS + node-level sync | Weakly consistent | No nulls |
| `CopyOnWriteArrayList` | Copy entire array on write | Snapshot | Allows null |
| `CopyOnWriteArraySet` | Backed by COWAL | Snapshot | Allows null |
| `ConcurrentLinkedQueue` | Lock-free (CAS) | Weakly consistent | No nulls |
| `LinkedBlockingQueue` | Two locks (put + take) | Weakly consistent | No nulls |
| `ArrayBlockingQueue` | Single lock | Weakly consistent | No nulls |
| `ConcurrentSkipListMap` | Lock-free | Weakly consistent | No nulls |
| `ConcurrentSkipListSet` | Lock-free | Weakly consistent | No nulls |

---

# SECTION V: JAVA 8+ FEATURES

---

## 5.1 Lambda Expressions
```java
// Before
Comparator<String> c = new Comparator<String>() {
    public int compare(String a, String b) { return a.compareTo(b); }
};

// After
Comparator<String> c = (a, b) -> a.compareTo(b);
Comparator<String> c = String::compareTo; // method reference
```
- **Effectively final:** Variables used in lambda must not be reassigned
- **`this` in lambda:** Refers to enclosing class (not the lambda itself)

## 5.2 Functional Interfaces

| Interface | Method | Signature | Use Case |
|---|---|---|---|
| `Predicate<T>` | `test(T)` | `T → boolean` | Filtering |
| `Function<T,R>` | `apply(T)` | `T → R` | Transformation |
| `Consumer<T>` | `accept(T)` | `T → void` | Side effects |
| `Supplier<T>` | `get()` | `() → T` | Factory/lazy |
| `BiFunction<T,U,R>` | `apply(T,U)` | `(T,U) → R` | Two-arg transform |
| `BiPredicate<T,U>` | `test(T,U)` | `(T,U) → boolean` | Two-arg filter |
| `BiConsumer<T,U>` | `accept(T,U)` | `(T,U) → void` | Two-arg consume |
| `UnaryOperator<T>` | `apply(T)` | `T → T` | Same type transform |
| `BinaryOperator<T>` | `apply(T,T)` | `(T,T) → T` | Reduction |

### Method References
| Type | Syntax | Equivalent Lambda |
|---|---|---|
| Static | `Math::abs` | `x -> Math.abs(x)` |
| Instance (bound) | `str::toUpperCase` | `() -> str.toUpperCase()` |
| Instance (unbound) | `String::toUpperCase` | `s -> s.toUpperCase()` |
| Constructor | `ArrayList::new` | `() -> new ArrayList<>()` |

## 5.3 Streams API

### Pipeline: Source → Intermediate (lazy) → Terminal (eager)

### Stream Sources
```java
list.stream()                          // from Collection
Arrays.stream(array)                   // from array
Stream.of(1, 2, 3)                     // explicit values
Stream.iterate(0, n -> n + 1)          // infinite
Stream.generate(Math::random)          // infinite
IntStream.range(1, 10)                 // primitive range
Files.lines(Path.of("file.txt"))       // from file
"hello".chars()                        // IntStream from String
Stream.concat(s1, s2)                  // concatenate
```

### Intermediate Operations (Lazy — nothing happens until terminal)
| Operation | Description |
|---|---|
| `filter(Predicate)` | Keep elements matching condition |
| `map(Function)` | Transform each element |
| `flatMap(Function)` | Transform + flatten (1-to-many) |
| `distinct()` | Remove duplicates (uses equals) |
| `sorted()` / `sorted(Comparator)` | Sort elements |
| `peek(Consumer)` | Side effect (debugging only) |
| `limit(n)` | Take first n elements |
| `skip(n)` | Skip first n elements |
| `takeWhile(Predicate)` | Take while true (Java 9+) |
| `dropWhile(Predicate)` | Drop while true (Java 9+) |
| `mapToInt/Long/Double` | Convert to primitive stream |

### Terminal Operations (Trigger execution)
| Operation | Description |
|---|---|
| `forEach(Consumer)` | Process each element |
| `collect(Collector)` | Accumulate into collection |
| `reduce(identity, BinaryOperator)` | Combine into single value |
| `count()` | Count elements |
| `findFirst()` / `findAny()` | Return Optional |
| `anyMatch` / `allMatch` / `noneMatch` | Boolean check |
| `min(Comparator)` / `max(Comparator)` | Min/max Optional |
| `toArray()` | Convert to array |
| `toList()` | Unmodifiable list (Java 16+) |

### Collectors (Most Important)
```java
.collect(Collectors.toList())                             // List
.collect(Collectors.toSet())                              // Set
.collect(Collectors.toMap(keyFn, valueFn))                 // Map
.collect(Collectors.groupingBy(classifier))               // Map<K, List<V>>
.collect(Collectors.groupingBy(classifier, counting()))   // Map<K, Long>
.collect(Collectors.partitioningBy(predicate))            // Map<Boolean, List<V>>
.collect(Collectors.joining(", "))                        // String
.collect(Collectors.summarizingInt(fn))                   // IntSummaryStatistics
.collect(Collectors.toUnmodifiableList())                 // Immutable list
.collect(Collectors.collectingAndThen(toList(), Collections::unmodifiableList))
```

### Parallel Streams
- `list.parallelStream()` or `stream.parallel()`
- Uses `ForkJoinPool.commonPool()` (CPU cores - 1 threads)
- **When to use:** CPU-intensive ops, large data, independent elements, no shared mutable state
- **When NOT to use:** I/O ops, small collections, ordered operations, shared state
- **Custom pool:** `new ForkJoinPool(4).submit(() -> stream.parallel()...)`

## 5.4 Optional
```java
Optional<User> opt = Optional.ofNullable(findUser(id));
String name = opt.map(User::getName)
                 .filter(n -> n.length() > 3)
                 .orElse("Unknown");

// Chaining
Optional<String> city = opt.flatMap(User::getAddress)    // avoids Optional<Optional<>>
                           .flatMap(Address::getCity);

// Java 9+
opt.ifPresentOrElse(user -> process(user), () -> handleEmpty());
opt.or(() -> Optional.of(defaultUser));         // lazy alternative
opt.stream();                                    // 0 or 1 element stream
```

**Anti-patterns:**
- Don't use as field type or method parameter
- Don't use `Optional.get()` without `isPresent()`
- Don't use for simple null checks (overhead)
- Don't use `Optional.of(null)` → NPE

## 5.5 Date/Time API (java.time)
| Class | Description | Example |
|---|---|---|
| `LocalDate` | Date without time | `2026-05-14` |
| `LocalTime` | Time without date | `14:30:00` |
| `LocalDateTime` | Date + time, no timezone | `2026-05-14T14:30:00` |
| `ZonedDateTime` | Date + time + timezone | `2026-05-14T14:30:00+05:30[Asia/Kolkata]` |
| `Instant` | Timestamp (epoch millis) | Machine time |
| `Duration` | Time-based amount | `PT2H30M` (2h 30m) |
| `Period` | Date-based amount | `P1Y2M3D` (1y 2m 3d) |
| `DateTimeFormatter` | Format/parse (thread-safe!) | `yyyy-MM-dd` |

## 5.6 Java Version Features Summary

| Version | Key Features |
|---|---|
| **Java 8** | Lambdas, Streams, Optional, `java.time`, default methods, `CompletableFuture` |
| **Java 9** | Modules (JPMS), `List.of()`, private interface methods, `Optional.ifPresentOrElse()`, JShell |
| **Java 10** | `var` (local variable type inference) |
| **Java 11** | `String` methods (isBlank, strip, lines, repeat), `HttpClient`, `var` in lambdas, single-file execution |
| **Java 12** | Switch expressions (preview), compact number formatting |
| **Java 13** | Text blocks (preview), switch expressions (second preview) |
| **Java 14** | Records (preview), `instanceof` pattern matching (preview), helpful NPEs, switch expressions (final) |
| **Java 15** | Text blocks (final), sealed classes (preview), hidden classes |
| **Java 16** | Records (final), `instanceof` pattern matching (final), `Stream.toList()` |
| **Java 17** | Sealed classes (final), pattern matching for switch (preview), always-strict FP |
| **Java 18** | Simple web server, code snippets in Javadoc |
| **Java 19** | Virtual threads (preview), structured concurrency (incubator) |
| **Java 20** | Scoped values (incubator), record patterns (preview) |
| **Java 21** | Virtual threads (final), sequenced collections, pattern matching switch (final), record patterns (final), string templates (preview) |

---

# SECTION VI: I/O AND NIO

---

## 6.1 Traditional I/O (java.io)
- **Byte Streams:** `InputStream` / `OutputStream` (binary data)
- **Character Streams:** `Reader` / `Writer` (text data, charset-aware)
- **Buffered Streams:** `BufferedReader`, `BufferedWriter`, `BufferedInputStream`
- **Object Streams:** `ObjectInputStream`, `ObjectOutputStream` (serialization)
- **Decorator Pattern:** Wrap streams for added functionality

## 6.2 NIO (java.nio) — Non-blocking I/O
- **Channels:** `FileChannel`, `SocketChannel`, `ServerSocketChannel` — bidirectional
- **Buffers:** `ByteBuffer`, `CharBuffer` — fixed-size, read/write modes, `flip()`, `compact()`
- **Selectors:** Multiplex multiple channels on single thread (event-driven)
- **Memory-mapped files:** `FileChannel.map()` — map file to memory (fast for large files)
- **Path + Files API (NIO.2):** `Path.of()`, `Files.readAllLines()`, `Files.walk()`, `Files.copy()`

## 6.3 try-with-resources
```java
try (var br = new BufferedReader(new FileReader("file.txt"));
     var bw = new BufferedWriter(new FileWriter("out.txt"))) {
    String line;
    while ((line = br.readLine()) != null) { bw.write(line); }
} // auto-closed in reverse order, even if exception thrown
```

---

# SECTION VII: DESIGN & ARCHITECTURE IN JAVA

---

## 7.1 Immutable Class Recipe
```java
public final class Money {                    // 1. final class
    private final BigDecimal amount;          // 2. final fields
    private final Currency currency;

    public Money(BigDecimal amount, Currency currency) {  // 3. constructor
        this.amount = amount;
        this.currency = currency;
    }

    public BigDecimal getAmount() { return amount; }     // 4. only getters
    public Currency getCurrency() { return currency; }
    // 5. No setters
    // 6. Defensive copy for mutable fields (Date, List, etc.)
    // 7. Don't provide methods that modify state
}
```
**Why?** Thread-safe, cacheable, great as Map keys, secure

## 7.2 Builder Pattern (Java Implementation)
```java
public class User {
    private final String name;     // required
    private final String email;    // required
    private final int age;         // optional
    private final String phone;    // optional

    private User(Builder builder) {
        this.name = builder.name;
        this.email = builder.email;
        this.age = builder.age;
        this.phone = builder.phone;
    }

    public static class Builder {
        private final String name;
        private final String email;
        private int age;
        private String phone;

        public Builder(String name, String email) {
            this.name = name;
            this.email = email;
        }
        public Builder age(int age) { this.age = age; return this; }
        public Builder phone(String phone) { this.phone = phone; return this; }
        public User build() { return new User(this); }
    }
}
// Usage: new User.Builder("John", "j@g.com").age(30).build();
```

## 7.3 Singleton Patterns (All Variants)
```java
// 1. Enum (BEST — thread-safe, serialization-safe, reflection-safe)
public enum Singleton { INSTANCE; public void doWork() {} }

// 2. Bill Pugh (lazy, thread-safe via class loading guarantee)
public class Singleton {
    private Singleton() {}
    private static class Holder { static final Singleton INSTANCE = new Singleton(); }
    public static Singleton getInstance() { return Holder.INSTANCE; }
}

// 3. Double-Checked Locking
public class Singleton {
    private static volatile Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) { instance = new Singleton(); }
            }
        }
        return instance;
    }
}
```

---

**Total: 7 Sections | JVM to Java 21 | Collections Internals | Full Concurrency | Streams Mastery**

**Study Plan for Java:**
1. JVM, Memory, GC — 2 days
2. Core Language (keywords, types, exceptions, generics) — 3 days
3. Collections internals (HashMap, ConcurrentHashMap, TreeMap) — 2 days
4. Concurrency (threads, locks, executors, CompletableFuture, virtual threads) — 3 days
5. Java 8+ features (lambdas, streams, Optional) — 2 days
6. I/O, NIO, design patterns in Java — 1 day
7. Practice: write code for all concepts — ongoing

**Total: ~2 weeks (2-3 hrs/day)** 🚀

