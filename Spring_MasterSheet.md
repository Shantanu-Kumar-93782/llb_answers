# SPRING FRAMEWORK & SPRING BOOT — Complete FAANG Interview Guide (5 YOE)

> Every Spring concept from IoC/DI to microservices patterns.
> Covers Spring Core, Boot, Web, Data JPA, Security, Cloud, and Testing.

---

# SECTION I: SPRING CORE (IoC & DI)

---

## 1.1 Inversion of Control (IoC)
- **Without IoC:** Class creates its own dependencies → tight coupling
- **With IoC:** Container creates and injects dependencies → loose coupling
- **IoC Container:** Manages object lifecycle, wiring, configuration
  - `BeanFactory` — lazy initialization, basic DI
  - `ApplicationContext` — eager initialization, adds events, i18n, AOP, environment
    - `AnnotationConfigApplicationContext` — Java config
    - `ClassPathXmlApplicationContext` — XML config
    - `WebApplicationContext` — web apps

## 1.2 Dependency Injection Types

### Constructor Injection (PREFERRED ★)
```java
@Service
public class OrderService {
    private final OrderRepository orderRepo;
    private final PaymentService paymentService;

    public OrderService(OrderRepository orderRepo, PaymentService paymentService) {
        this.orderRepo = orderRepo;           // immutable
        this.paymentService = paymentService;  // required deps enforced
    }
}
// @Autowired not needed if single constructor (Spring 4.3+)
```
**Why best:** Immutability (`final`), required deps enforced (compile-time), no reflection, easy to unit test, no circular dependency hiding

### Setter Injection
```java
@Service
public class NotificationService {
    private EmailSender emailSender;

    @Autowired(required = false)  // optional dependency
    public void setEmailSender(EmailSender emailSender) {
        this.emailSender = emailSender;
    }
}
```
**When:** Optional dependencies, reconfigurable at runtime

### Field Injection (AVOID ★)
```java
@Service
public class UserService {
    @Autowired  // bad — untestable, hides dependencies
    private UserRepository userRepo;
}
```
**Why avoid:** Can't instantiate without Spring, hides dependencies, not immutable, hard to test

## 1.3 Bean Lifecycle (Complete)
```
1. Instantiation (constructor)
2. Populate properties (DI)
3. BeanNameAware.setBeanName()
4. BeanFactoryAware.setBeanFactory()
5. ApplicationContextAware.setApplicationContext()
6. BeanPostProcessor.postProcessBeforeInitialization()
7. @PostConstruct
8. InitializingBean.afterPropertiesSet()
9. Custom init-method (@Bean(initMethod="init"))
10. BeanPostProcessor.postProcessAfterInitialization()
11. ═══ BEAN READY FOR USE ═══
12. @PreDestroy
13. DisposableBean.destroy()
14. Custom destroy-method (@Bean(destroyMethod="cleanup"))
```

### BeanPostProcessor
- Intercept bean creation — modify/wrap ANY bean
- `postProcessBeforeInitialization()` — before init
- `postProcessAfterInitialization()` — after init (AOP proxies created here)
- **Used by Spring internally:** AOP, `@Autowired`, `@Value`, `@Scheduled`

### BeanFactoryPostProcessor
- Modify bean DEFINITIONS before any beans are created
- `PropertySourcesPlaceholderConfigurer` — resolves `${...}` placeholders

## 1.4 Bean Scopes

| Scope | Lifecycle | Use Case |
|---|---|---|
| `singleton` (default) | One per container, lives until container shutdown | Services, repos, config |
| `prototype` | New instance per injection/getBean() | Stateful objects, commands |
| `request` | One per HTTP request | Request-scoped data |
| `session` | One per HTTP session | User session data |
| `application` | One per ServletContext | Shared across app |
| `websocket` | One per WebSocket session | WebSocket state |

### Prototype-in-Singleton Problem ★
```java
@Service // singleton
public class OrderService {
    @Autowired
    private ShoppingCart cart; // prototype — BUT gets injected ONCE!
}
```
**Problem:** Prototype bean injected once into singleton → becomes effectively singleton

**Solutions:**
1. `ObjectFactory<ShoppingCart>` or `Provider<ShoppingCart>` — call `.getObject()` / `.get()` each time
2. `@Lookup` method — Spring overrides method to return new prototype
3. `ApplicationContext.getBean()` — direct lookup (not recommended)
4. Scoped proxy: `@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)`

## 1.5 Annotations

### Stereotype Annotations
| Annotation | Purpose | Special Behavior |
|---|---|---|
| `@Component` | Generic Spring-managed bean | Base annotation |
| `@Service` | Business logic layer | Semantic only (no extra behavior) |
| `@Repository` | Data access layer | Exception translation (SQL → Spring DataAccessException) |
| `@Controller` | Web controller (MVC) | Returns view names |
| `@RestController` | REST API controller | = `@Controller` + `@ResponseBody` |
| `@Configuration` | Java-based configuration | CGLIB proxied, `@Bean` methods are singleton by default |

### Configuration Annotations
| Annotation | Purpose |
|---|---|
| `@Bean` | Declare bean in `@Configuration` class |
| `@ComponentScan` | Auto-detect `@Component` classes |
| `@Import` | Import other config classes |
| `@PropertySource` | Load properties file |
| `@ConfigurationProperties` | Bind properties to POJO |
| `@Value("${key}")` | Inject single property value |
| `@Profile("dev")` | Activate bean for specific profile |
| `@Conditional` | Conditional bean registration |
| `@Lazy` | Lazy initialization |
| `@DependsOn` | Explicit ordering |
| `@Order` / `@Priority` | Bean ordering |

### Injection Annotations
| Annotation | Source | By |
|---|---|---|
| `@Autowired` | Spring | Type (then qualifier) |
| `@Qualifier("name")` | Spring | Disambiguate by name |
| `@Primary` | Spring | Default when multiple candidates |
| `@Resource` | JSR-250 | Name (then type) |
| `@Inject` | JSR-330 | Type (like @Autowired) |

## 1.6 Spring AOP (Aspect-Oriented Programming)

### Concepts
| Term | Description |
|---|---|
| **Aspect** | Cross-cutting concern module (e.g., logging aspect) |
| **Advice** | Action taken at a joinpoint (the actual code) |
| **JoinPoint** | Point during execution (method call, field access) |
| **Pointcut** | Expression that selects joinpoints |
| **Weaving** | Linking aspects with target objects |
| **Target** | Object being advised |
| **Proxy** | Object created by AOP (wraps target) |

### Advice Types
```java
@Aspect
@Component
public class LoggingAspect {
    
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint jp) {
        log.info("Calling: {}", jp.getSignature());
    }
    
    @AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", returning = "result")
    public void logAfterReturn(JoinPoint jp, Object result) {
        log.info("Returned: {}", result);
    }
    
    @AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", throwing = "ex")
    public void logException(JoinPoint jp, Exception ex) {
        log.error("Exception in {}: {}", jp.getSignature(), ex.getMessage());
    }
    
    @After("execution(* com.example.service.*.*(..))")
    public void logAfter(JoinPoint jp) {
        log.info("Finished: {}", jp.getSignature()); // runs regardless of outcome
    }
    
    @Around("execution(* com.example.service.*.*(..))")
    public Object measureTime(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = pjp.proceed();  // call actual method
        long time = System.currentTimeMillis() - start;
        log.info("{} took {}ms", pjp.getSignature(), time);
        return result;
    }
}
```

### Pointcut Expressions
```java
execution(* com.example.service.*.*(..))    // any method in service package
execution(public * *(..))                    // any public method
execution(* save*(..))                       // methods starting with "save"
@annotation(com.example.Loggable)            // methods with @Loggable annotation
within(com.example.service.*)                // any method within package
bean(orderService)                           // specific bean
args(String, ..)                             // first arg is String
```

### Proxy Mechanism
| Type | When Used | How |
|---|---|---|
| **JDK Dynamic Proxy** | Target implements interface | Creates proxy implementing same interface |
| **CGLIB Proxy** | Target has no interface | Creates subclass of target (cannot proxy `final` classes/methods) |

- **Spring Boot default:** CGLIB (even for interfaces, since Spring Boot 2.0)
- **`@Transactional` trap:** Self-invocation bypasses proxy → transaction not applied
  ```java
  @Service
  public class OrderService {
      @Transactional
      public void placeOrder() { ... }
      
      public void process() {
          placeOrder(); // ⚠️ Direct call — NO proxy — NO transaction!
      }
  }
  ```
  **Fix:** Inject self, use `AopContext.currentProxy()`, or extract to another service

## 1.7 Spring Events
```java
// Define event
public class OrderPlacedEvent extends ApplicationEvent {
    private final Order order;
    public OrderPlacedEvent(Object source, Order order) {
        super(source);
        this.order = order;
    }
}

// Publish
@Service
public class OrderService {
    @Autowired private ApplicationEventPublisher publisher;
    
    public void placeOrder(Order order) {
        // ... save order
        publisher.publishEvent(new OrderPlacedEvent(this, order));
    }
}

// Listen
@Component
public class NotificationListener {
    @EventListener
    public void onOrderPlaced(OrderPlacedEvent event) {
        sendNotification(event.getOrder());
    }
    
    @Async @EventListener  // async processing
    public void onOrderPlacedAsync(OrderPlacedEvent event) { ... }
    
    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void afterCommit(OrderPlacedEvent event) { ... } // only if tx commits
}
```

## 1.8 SpEL (Spring Expression Language)
```java
@Value("#{systemProperties['user.home']}")        // system property
@Value("#{T(java.lang.Math).PI}")                  // static field
@Value("#{orderService.getDiscount()}")             // bean method call
@Value("#{2 * T(java.lang.Math).PI * 10}")          // computation
@Value("#{user.name ?: 'Anonymous'}")               // Elvis operator (null-safe)
@PreAuthorize("#userId == authentication.principal.id")  // security
```

---

# SECTION II: SPRING BOOT

---

## 2.1 Auto-Configuration
- `@SpringBootApplication` = `@Configuration` + `@EnableAutoConfiguration` + `@ComponentScan`
- **How it works:**
  1. Spring Boot scans `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`
  2. Evaluates `@Conditional*` annotations on each auto-config class
  3. If conditions met → registers beans automatically
- **Conditions:**
  - `@ConditionalOnClass` — class exists on classpath
  - `@ConditionalOnMissingBean` — no user-defined bean of this type
  - `@ConditionalOnProperty` — property has specific value
  - `@ConditionalOnWebApplication` — web app context
- **Customizing:** Define your own bean → auto-config backs off (`@ConditionalOnMissingBean`)
- **Debug:** `--debug` flag or `debug=true` in properties → prints auto-config report

## 2.2 Externalized Configuration

### Priority Order (highest to lowest)
```
1. Command-line arguments (--server.port=9090)
2. SPRING_APPLICATION_JSON (inline JSON)
3. ServletConfig/ServletContext parameters
4. JNDI attributes
5. System properties (System.getProperties())
6. OS environment variables
7. application-{profile}.properties/yml (profile-specific)
8. application.properties/yml
9. @PropertySource on @Configuration classes
10. Default properties (SpringApplication.setDefaultProperties())
```

### @ConfigurationProperties (Type-Safe)
```java
@ConfigurationProperties(prefix = "app.mail")
@Validated
public class MailProperties {
    @NotBlank private String host;
    @Min(1) @Max(65535) private int port;
    private boolean ssl = false;
    private List<String> recipients = new ArrayList<>();
    private Map<String, String> headers = new HashMap<>();
    // getters + setters
}

// application.yml
app:
  mail:
    host: smtp.gmail.com
    port: 587
    ssl: true
    recipients:
      - admin@example.com
      - support@example.com
    headers:
      X-Priority: "1"
```
- Enable: `@EnableConfigurationProperties(MailProperties.class)` or `@ConfigurationPropertiesScan`
- **Relaxed binding:** `app.mail-host`, `APP_MAIL_HOST`, `app.mailHost` all map to `mailHost`

## 2.3 Profiles
- Activate: `spring.profiles.active=dev,local`
- Profile-specific files: `application-dev.yml`, `application-prod.yml`
- `@Profile("dev")` — bean only in dev
- `@Profile("!prod")` — bean in everything except prod
- **Profile groups (2.4+):** `spring.profiles.group.local=dev,debug`

## 2.4 Spring Boot Actuator

### Endpoints
| Endpoint | Description |
|---|---|
| `/actuator/health` | Application health (UP/DOWN) |
| `/actuator/info` | Application info (git, build) |
| `/actuator/metrics` | Application metrics |
| `/actuator/metrics/{name}` | Specific metric |
| `/actuator/env` | Environment properties |
| `/actuator/beans` | All beans in context |
| `/actuator/mappings` | All request mappings |
| `/actuator/loggers` | View/change log levels at runtime |
| `/actuator/threaddump` | Thread dump |
| `/actuator/heapdump` | Heap dump (download) |
| `/actuator/scheduledtasks` | Scheduled tasks |
| `/actuator/caches` | Cache info |
| `/actuator/prometheus` | Prometheus-format metrics |

### Custom Health Indicator
```java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    @Override
    public Health health() {
        if (isDatabaseUp()) {
            return Health.up().withDetail("database", "PostgreSQL 15").build();
        }
        return Health.down().withDetail("error", "Connection refused").build();
    }
}
```

### Custom Metrics
```java
@Service
public class OrderService {
    private final Counter orderCounter;
    private final Timer orderTimer;
    
    public OrderService(MeterRegistry registry) {
        this.orderCounter = Counter.builder("orders.placed")
                .tag("type", "online").register(registry);
        this.orderTimer = Timer.builder("orders.processing.time")
                .register(registry);
    }
    
    public void placeOrder(Order order) {
        orderTimer.record(() -> {
            // process order
            orderCounter.increment();
        });
    }
}
```

### Production Configuration
```yaml
management:
  endpoints:
    web:
      exposure:
        include: health, info, metrics, prometheus  # expose only needed
  endpoint:
    health:
      show-details: when-authorized  # hide details from public
  server:
    port: 8081  # separate management port
```

## 2.5 Embedded Server
- **Tomcat** (default), **Jetty**, **Undertow**, **Netty** (WebFlux)
- Switch: Exclude `spring-boot-starter-tomcat`, add `spring-boot-starter-jetty`
- Customize:
  ```yaml
  server:
    port: 8080
    servlet:
      context-path: /api
    tomcat:
      max-threads: 200
      max-connections: 10000
      accept-count: 100
    compression:
      enabled: true
      min-response-size: 1024
  ```

## 2.6 Startup & Initialization
- `CommandLineRunner` — `run(String... args)` after context loaded
- `ApplicationRunner` — `run(ApplicationArguments args)` after context loaded
- `@EventListener(ApplicationReadyEvent.class)` — app fully ready
- `ApplicationStartedEvent` → `ApplicationReadyEvent` → `ContextClosedEvent`
- `@PostConstruct` — per-bean initialization

## 2.7 Logging
- Default: **Logback** (SLF4J facade)
- Alternatives: Log4j2, JUL
- Configuration: `logback-spring.xml` (Spring-aware) or `application.yml`
```yaml
logging:
  level:
    root: INFO
    com.example.service: DEBUG
    org.hibernate.SQL: DEBUG
  pattern:
    console: "%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"
  file:
    name: app.log
    max-size: 10MB
    max-history: 30
```

---

# SECTION III: SPRING WEB / REST API

---

## 3.1 Controller Layer
```java
@RestController
@RequestMapping("/api/v1/users")
@Validated
public class UserController {

    private final UserService userService;
    
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping
    public ResponseEntity<Page<UserResponse>> getAll(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size,
            @RequestParam(defaultValue = "name,asc") String sort) {
        return ResponseEntity.ok(userService.findAll(PageRequest.of(page, size)));
    }

    @GetMapping("/{id}")
    public ResponseEntity<UserResponse> getById(@PathVariable Long id) {
        return ResponseEntity.ok(userService.findById(id));
    }

    @PostMapping
    public ResponseEntity<UserResponse> create(@Valid @RequestBody CreateUserRequest request) {
        UserResponse created = userService.create(request);
        URI location = URI.create("/api/v1/users/" + created.getId());
        return ResponseEntity.created(location).body(created);
    }

    @PutMapping("/{id}")
    public ResponseEntity<UserResponse> update(
            @PathVariable Long id, @Valid @RequestBody UpdateUserRequest request) {
        return ResponseEntity.ok(userService.update(id, request));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> delete(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
}
```

## 3.2 Request/Response Handling

### Parameter Binding
| Annotation | Source | Example |
|---|---|---|
| `@PathVariable` | URL path | `/users/{id}` |
| `@RequestParam` | Query string | `?name=John&age=30` |
| `@RequestBody` | Request body (JSON) | POST/PUT payload |
| `@RequestHeader` | HTTP header | `Authorization`, `Content-Type` |
| `@CookieValue` | Cookie | Session cookie |
| `@ModelAttribute` | Form data / query to object | Form submissions |
| `@MatrixVariable` | Matrix params | `/users;role=admin` |

### Response Control
```java
// ResponseEntity — full control
return ResponseEntity.status(HttpStatus.CREATED)
        .header("X-Custom", "value")
        .body(responseObj);

// @ResponseStatus on method or exception
@ResponseStatus(HttpStatus.NO_CONTENT)
@DeleteMapping("/{id}")
public void delete(@PathVariable Long id) { ... }
```

### HTTP Status Codes (Know These)
| Code | Meaning | When |
|---|---|---|
| 200 | OK | Successful GET, PUT |
| 201 | Created | Successful POST (with Location header) |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Validation failure |
| 401 | Unauthorized | Not authenticated |
| 403 | Forbidden | Authenticated but no permission |
| 404 | Not Found | Resource doesn't exist |
| 405 | Method Not Allowed | Wrong HTTP method |
| 409 | Conflict | Duplicate, version conflict |
| 422 | Unprocessable Entity | Semantic error |
| 429 | Too Many Requests | Rate limited |
| 500 | Internal Server Error | Unhandled exception |
| 502 | Bad Gateway | Upstream failure |
| 503 | Service Unavailable | Server overloaded |

## 3.3 Exception Handling

### Global Exception Handler
```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(
            LocalDateTime.now(), 404, "Not Found", ex.getMessage());
        return ResponseEntity.status(404).body(error);
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidation(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(e -> 
            errors.put(e.getField(), e.getDefaultMessage()));
        ErrorResponse error = new ErrorResponse(
            LocalDateTime.now(), 400, "Validation Failed", errors.toString());
        return ResponseEntity.badRequest().body(error);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneral(Exception ex) {
        log.error("Unhandled exception", ex);
        ErrorResponse error = new ErrorResponse(
            LocalDateTime.now(), 500, "Internal Error", "Something went wrong");
        return ResponseEntity.internalServerError().body(error);
    }
}

record ErrorResponse(LocalDateTime timestamp, int status, String error, String message) {}
```

## 3.4 Validation
```java
public record CreateUserRequest(
    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 100) String name,

    @NotBlank @Email String email,

    @NotNull @Min(18) @Max(120) Integer age,

    @Pattern(regexp = "^\\+?[1-9]\\d{1,14}$", message = "Invalid phone")
    String phone,

    @NotEmpty List<@NotBlank String> roles
) {}

// Custom Validator
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = UniqueEmailValidator.class)
public @interface UniqueEmail {
    String message() default "Email already exists";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}

public class UniqueEmailValidator implements ConstraintValidator<UniqueEmail, String> {
    @Autowired private UserRepository userRepo;
    
    @Override
    public boolean isValid(String email, ConstraintValidatorContext ctx) {
        return email != null && !userRepo.existsByEmail(email);
    }
}
```

### Group Validation
```java
public interface OnCreate {}
public interface OnUpdate {}

public class UserRequest {
    @Null(groups = OnCreate.class)
    @NotNull(groups = OnUpdate.class)
    private Long id;
    
    @NotBlank(groups = {OnCreate.class, OnUpdate.class})
    private String name;
}

@PostMapping
public void create(@Validated(OnCreate.class) @RequestBody UserRequest req) { }
```

## 3.5 Filters, Interceptors, AOP — Execution Order
```
HTTP Request
    ↓
┌─ Servlet Filter (OncePerRequestFilter) ─────────────┐
│  Security, CORS, Logging, Request wrapping           │
│    ↓                                                  │
│  ┌─ HandlerInterceptor ────────────────────────────┐ │
│  │  preHandle() — auth, timing, MDC context         │ │
│  │    ↓                                             │ │
│  │  ┌─ Controller Method ────────────────────────┐ │ │
│  │  │    ↓                                       │ │ │
│  │  │  ┌─ AOP @Around ────────────────────────┐ │ │ │
│  │  │  │  Service method execution             │ │ │ │
│  │  │  └───────────────────────────────────────┘ │ │ │
│  │  └────────────────────────────────────────────┘ │ │
│  │  postHandle() — modify ModelAndView              │ │
│  │  afterCompletion() — cleanup, timing end         │ │
│  └──────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────┘
    ↓
HTTP Response
```

### Custom Filter
```java
@Component
@Order(1)
public class RequestLoggingFilter extends OncePerRequestFilter {
    @Override
    protected void doFilterInternal(HttpServletRequest request,
            HttpServletResponse response, FilterChain chain) throws ... {
        String requestId = UUID.randomUUID().toString();
        MDC.put("requestId", requestId);
        log.info("Request: {} {}", request.getMethod(), request.getRequestURI());
        long start = System.currentTimeMillis();
        
        chain.doFilter(request, response);  // proceed
        
        log.info("Response: {} in {}ms", response.getStatus(), 
                 System.currentTimeMillis() - start);
        MDC.clear();
    }
}
```

### Custom Interceptor
```java
@Component
public class AuthInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest req, HttpServletResponse res, Object handler) {
        String token = req.getHeader("Authorization");
        if (!isValid(token)) {
            res.setStatus(401);
            return false;  // stop chain
        }
        return true;  // continue
    }
}

@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new AuthInterceptor())
                .addPathPatterns("/api/**")
                .excludePathPatterns("/api/auth/**");
    }
}
```

## 3.6 REST API Best Practices

| Practice | Do | Don't |
|---|---|---|
| URLs | `/api/v1/users/{id}/orders` | `/api/getUser`, `/api/deleteOrder` |
| HTTP methods | GET=read, POST=create, PUT=replace, PATCH=partial, DELETE=remove | POST for everything |
| Status codes | 201 for created, 204 for delete | 200 for everything |
| Pagination | `?page=0&size=20&sort=name,asc` | Return all records |
| Filtering | `?status=active&minAge=18` | Complex query in body for GET |
| Versioning | URI (`/v1/`), header, or media type | No versioning |
| Error format | Consistent `{timestamp, status, error, message, path}` | Stack traces in response |
| Idempotency | GET, PUT, DELETE are idempotent | Non-idempotent PUT |
| HATEOAS | Links to related resources (if adopted) | Hard-coded client URLs |

## 3.7 Content Negotiation & Jackson
```yaml
spring:
  jackson:
    serialization:
      write-dates-as-timestamps: false  # ISO format
      indent-output: true               # pretty print (dev only)
    deserialization:
      fail-on-unknown-properties: false  # ignore extra fields
    default-property-inclusion: non_null  # skip nulls
    date-format: "yyyy-MM-dd'T'HH:mm:ss"
```

### Jackson Annotations
```java
@JsonProperty("user_name")              // rename field
@JsonIgnore                             // exclude from JSON
@JsonIgnoreProperties({"password"})     // class-level ignore
@JsonInclude(Include.NON_NULL)          // skip nulls
@JsonFormat(pattern = "yyyy-MM-dd")     // date format
@JsonCreator                            // custom deserialization constructor
@JsonSerialize(using = CustomSerializer.class)
@JsonDeserialize(using = CustomDeserializer.class)
```

## 3.8 Async REST
```java
@GetMapping("/async")
public CompletableFuture<ResponseEntity<Data>> getAsync() {
    return CompletableFuture.supplyAsync(() -> fetchData())
            .thenApply(ResponseEntity::ok);
}

@GetMapping("/sse")
public SseEmitter streamEvents() {
    SseEmitter emitter = new SseEmitter(60_000L);
    executor.execute(() -> {
        for (int i = 0; i < 10; i++) {
            emitter.send(SseEmitter.event().data("Event " + i));
            Thread.sleep(1000);
        }
        emitter.complete();
    });
    return emitter;
}
```

## 3.9 WebClient (Reactive HTTP Client)
```java
@Service
public class ExternalApiService {
    private final WebClient webClient;
    
    public ExternalApiService(WebClient.Builder builder) {
        this.webClient = builder.baseUrl("https://api.example.com")
                .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                .filter(ExchangeFilterFunction.ofRequestProcessor(
                    req -> { log.info("Request: {}", req.url()); return Mono.just(req); }))
                .build();
    }
    
    public Mono<User> getUser(Long id) {
        return webClient.get()
                .uri("/users/{id}", id)
                .retrieve()
                .onStatus(HttpStatusCode::is4xxClientError, 
                    resp -> Mono.error(new NotFoundException("User not found")))
                .bodyToMono(User.class)
                .timeout(Duration.ofSeconds(5))
                .retry(3);
    }
}
```

---

# SECTION IV: SPRING DATA JPA

---

## 4.1 Repository Hierarchy
```
Repository<T, ID> (marker)
  └── CrudRepository<T, ID> (CRUD)
        └── ListCrudRepository<T, ID> (returns List)
              └── JpaRepository<T, ID> (JPA-specific: flush, batch, example)
                    └── JpaSpecificationExecutor<T> (dynamic queries)
```

## 4.2 Query Methods
```java
public interface UserRepository extends JpaRepository<User, Long> {
    // Derived queries (method name parsing)
    List<User> findByName(String name);
    List<User> findByNameAndAge(String name, int age);
    List<User> findByAgeBetween(int min, int max);
    List<User> findByNameContainingIgnoreCase(String name);
    List<User> findByStatusOrderByCreatedAtDesc(Status status);
    Optional<User> findByEmail(String email);
    boolean existsByEmail(String email);
    long countByStatus(Status status);
    List<User> findTop5ByOrderByCreatedAtDesc();
    
    // JPQL
    @Query("SELECT u FROM User u WHERE u.age > :age AND u.status = :status")
    List<User> findActiveUsersAboveAge(@Param("age") int age, @Param("status") Status status);
    
    // Native SQL
    @Query(value = "SELECT * FROM users WHERE email LIKE %:domain", nativeQuery = true)
    List<User> findByEmailDomain(@Param("domain") String domain);
    
    // Modifying
    @Modifying
    @Query("UPDATE User u SET u.status = :status WHERE u.lastLogin < :date")
    int deactivateInactiveUsers(@Param("status") Status status, @Param("date") LocalDate date);
    
    // Projections
    @Query("SELECT u.name as name, u.email as email FROM User u")
    List<UserSummary> findAllSummaries();
}

// Interface projection
interface UserSummary {
    String getName();
    String getEmail();
}
```

## 4.3 Entity Mapping
```java
@Entity
@Table(name = "users", indexes = {
    @Index(name = "idx_email", columnList = "email", unique = true),
    @Index(name = "idx_status_created", columnList = "status, created_at")
})
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)  // or SEQUENCE for batch inserts
    private Long id;
    
    @Column(nullable = false, length = 100)
    private String name;
    
    @Column(unique = true, nullable = false)
    private String email;
    
    @Enumerated(EnumType.STRING)  // never use ORDINAL — breaks on reorder
    private Status status;
    
    @Embedded
    private Address address;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Order> orders = new ArrayList<>();
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "department_id")
    private Department department;
    
    @ManyToMany
    @JoinTable(name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id"))
    private Set<Role> roles = new HashSet<>();
    
    @CreatedDate
    private LocalDateTime createdAt;
    
    @LastModifiedDate
    private LocalDateTime updatedAt;
    
    @Version  // optimistic locking
    private Long version;
}
```

### Relationship Defaults & Best Practices
| Relationship | Default Fetch | Best Practice |
|---|---|---|
| `@OneToOne` | EAGER | Change to LAZY |
| `@ManyToOne` | EAGER | Change to LAZY |
| `@OneToMany` | LAZY | Keep LAZY ✓ |
| `@ManyToMany` | LAZY | Keep LAZY ✓ |

**Rule:** Always use LAZY. Fetch eagerly only when needed via `JOIN FETCH` or `@EntityGraph`.

## 4.4 N+1 Problem ★

### Problem
```java
List<User> users = userRepo.findAll();       // 1 query: SELECT * FROM users
for (User u : users) {
    u.getDepartment().getName();             // N queries: SELECT * FROM department WHERE id=?
}
// Total: 1 + N queries!
```

### Solutions
```java
// 1. JOIN FETCH (JPQL)
@Query("SELECT u FROM User u JOIN FETCH u.department")
List<User> findAllWithDepartment();

// 2. @EntityGraph
@EntityGraph(attributePaths = {"department", "roles"})
List<User> findAll();

// 3. @BatchSize (Hibernate)
@BatchSize(size = 20)
@OneToMany(mappedBy = "user")
private List<Order> orders;
// Fetches 20 users' orders in one query instead of 20 separate queries

// 4. DTO Projection (best for read-only)
@Query("SELECT new com.example.dto.UserDTO(u.name, u.email, d.name) " +
       "FROM User u JOIN u.department d")
List<UserDTO> findAllUserDTOs();
```

## 4.5 JPA Entity Lifecycle
```
     new User()          persist()         flush()
Transient ────────→ Persistent ────────→ Database
                        ↑    │
                  merge()│    │ detach()/clear()
                        │    ↓
                      Detached
                        │
                  remove()
                        ↓
                      Removed
```

- **Persistent:** Managed by EntityManager, changes auto-detected (dirty checking)
- **Detached:** No longer managed, changes NOT auto-saved
- **Dirty Checking:** Hibernate compares current state with snapshot at flush time

## 4.6 Transactions (`@Transactional`)

### Propagation
| Type | Behavior |
|---|---|
| `REQUIRED` (default) | Join existing tx, or create new |
| `REQUIRES_NEW` | Always create new, suspend current |
| `NESTED` | Savepoint within current tx |
| `SUPPORTS` | Join if exists, else non-transactional |
| `MANDATORY` | Must exist, else throw exception |
| `NOT_SUPPORTED` | Suspend current, run non-transactional |
| `NEVER` | Must NOT exist, else throw exception |

### Isolation Levels
| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|---|---|---|---|
| `READ_UNCOMMITTED` | ✓ | ✓ | ✓ |
| `READ_COMMITTED` | ✗ | ✓ | ✓ |
| `REPEATABLE_READ` | ✗ | ✗ | ✓ |
| `SERIALIZABLE` | ✗ | ✗ | ✗ |

### Common Pitfalls
1. **Self-invocation:** Calling `@Transactional` method from same class bypasses proxy
2. **Private methods:** `@Transactional` on private methods is ignored
3. **Checked exceptions:** By default, tx only rolls back on unchecked exceptions
   - Fix: `@Transactional(rollbackFor = Exception.class)`
4. **Read-only:** `@Transactional(readOnly = true)` — optimization hint, no dirty checking

### Locking
```java
// Optimistic (field-level)
@Version
private Long version;  // auto-incremented on update, throws OptimisticLockException on conflict

// Pessimistic (query-level)
@Lock(LockModeType.PESSIMISTIC_WRITE)
@Query("SELECT u FROM User u WHERE u.id = :id")
Optional<User> findByIdForUpdate(@Param("id") Long id);
```

## 4.7 Caching

### L1 Cache (First-Level) — Always ON
- Per `EntityManager` / Session
- Same entity fetched twice in same transaction → returns cached instance
- Cleared on `clear()` or session close

### L2 Cache (Second-Level) — Optional
- Shared across sessions/transactions
- Enable: `@Cacheable` on entity, configure provider (EhCache, Hazelcast, Caffeine)
```java
@Entity
@Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
public class Product { ... }
```

### Spring Cache Abstraction
```java
@Service
public class ProductService {
    @Cacheable(value = "products", key = "#id")
    public Product findById(Long id) { return repo.findById(id).orElseThrow(); }
    
    @CachePut(value = "products", key = "#product.id")
    public Product update(Product product) { return repo.save(product); }
    
    @CacheEvict(value = "products", key = "#id")
    public void delete(Long id) { repo.deleteById(id); }
    
    @CacheEvict(value = "products", allEntries = true)
    public void clearCache() { }
}
```

## 4.8 Auditing
```java
@Configuration
@EnableJpaAuditing
public class AuditConfig {
    @Bean
    public AuditorAware<String> auditorProvider() {
        return () -> Optional.of(SecurityContextHolder.getContext()
                .getAuthentication().getName());
    }
}

@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseEntity {
    @CreatedDate private LocalDateTime createdAt;
    @LastModifiedDate private LocalDateTime updatedAt;
    @CreatedBy private String createdBy;
    @LastModifiedBy private String updatedBy;
}
```

## 4.9 Database Migration

### Flyway
```
src/main/resources/db/migration/
  V1__create_users_table.sql
  V2__add_email_column.sql
  V3__create_orders_table.sql
  R__refresh_views.sql          (repeatable migration)
```
- **Convention:** `V{version}__{description}.sql`
- **Rules:** Never modify existing migrations. Always add new.
- **Rollback:** `U{version}__{description}.sql` (Flyway Teams)

### Liquibase
```yaml
# changelog-master.yaml
databaseChangeLog:
  - changeSet:
      id: 1
      author: dev
      changes:
        - createTable:
            tableName: users
            columns:
              - column: { name: id, type: bigint, autoIncrement: true, constraints: { primaryKey: true } }
              - column: { name: name, type: varchar(100), constraints: { nullable: false } }
```

---

# SECTION V: SPRING SECURITY

---

## 5.1 Security Filter Chain
```
HTTP Request
    ↓
SecurityFilterChain (ordered list of filters):
    ↓
1. DisableEncodeUrlFilter
2. WebAsyncManagerIntegrationFilter
3. SecurityContextHolderFilter
4. HeaderWriterFilter
5. CorsFilter
6. CsrfFilter
7. LogoutFilter
8. UsernamePasswordAuthenticationFilter / BearerTokenAuthenticationFilter
9. DefaultLoginPageGeneratingFilter
10. RequestCacheAwareFilter
11. SecurityContextHolderAwareRequestFilter
12. AnonymousAuthenticationFilter
13. ExceptionTranslationFilter
14. AuthorizationFilter (was FilterSecurityInterceptor)
    ↓
Controller
```

## 5.2 Configuration (Spring Security 6+)
```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
            .csrf(csrf -> csrf.disable())  // disable for REST APIs
            .cors(cors -> cors.configurationSource(corsConfig()))
            .sessionManagement(sm -> sm.sessionCreationPolicy(STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**", "/actuator/health").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .requestMatchers(HttpMethod.GET, "/api/products/**").permitAll()
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class)
            .exceptionHandling(ex -> ex
                .authenticationEntryPoint((req, res, authEx) -> 
                    res.sendError(401, "Unauthorized"))
                .accessDeniedHandler((req, res, authEx) -> 
                    res.sendError(403, "Forbidden"))
            )
            .build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(12);
    }

    @Bean
    public AuthenticationManager authManager(AuthenticationConfiguration config) throws Exception {
        return config.getAuthenticationManager();
    }
}
```

## 5.3 JWT Authentication Flow
```
1. POST /api/auth/login {email, password}
2. Server validates credentials via AuthenticationManager
3. Server generates JWT:
   - Access Token (short-lived: 15min-1hr)
   - Refresh Token (long-lived: 7-30 days)
4. Client stores tokens (HttpOnly cookie or secure storage)
5. Client sends: Authorization: Bearer <accessToken>
6. JwtAuthenticationFilter:
   a. Extract token from header
   b. Validate signature + expiry
   c. Extract claims (userId, roles)
   d. Create Authentication object
   e. Set SecurityContextHolder
7. Request proceeds to controller
8. On 401 → client uses refresh token to get new access token
```

### JWT Filter Implementation
```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    private final JwtService jwtService;
    private final UserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(HttpServletRequest request,
            HttpServletResponse response, FilterChain chain) throws ... {
        
        String header = request.getHeader("Authorization");
        if (header == null || !header.startsWith("Bearer ")) {
            chain.doFilter(request, response);
            return;
        }
        
        String token = header.substring(7);
        String username = jwtService.extractUsername(token);
        
        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);
            if (jwtService.isTokenValid(token, userDetails)) {
                var auth = new UsernamePasswordAuthenticationToken(
                        userDetails, null, userDetails.getAuthorities());
                auth.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(auth);
            }
        }
        chain.doFilter(request, response);
    }
}
```

## 5.4 Method-Level Security
```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long id) { }

@PreAuthorize("hasAnyRole('ADMIN', 'MANAGER')")
public List<User> getAllUsers() { }

@PreAuthorize("#userId == authentication.principal.id or hasRole('ADMIN')")
public User getUser(Long userId) { }

@PostAuthorize("returnObject.owner == authentication.name")
public Document getDocument(Long id) { }

@PreFilter("filterObject.owner == authentication.name")
public void deleteDocuments(List<Document> docs) { }

@PostFilter("filterObject.department == authentication.principal.department")
public List<User> getUsers() { }
```

## 5.5 OAuth 2.0 / OpenID Connect

### Grant Types
| Grant | Use Case |
|---|---|
| **Authorization Code + PKCE** | Web/mobile apps (most secure) |
| **Client Credentials** | Machine-to-machine (no user) |
| **Refresh Token** | Get new access token without re-login |
| ~~Implicit~~ | Deprecated (use Auth Code + PKCE) |
| ~~Resource Owner Password~~ | Deprecated |

### Spring as Resource Server
```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    return http
        .oauth2ResourceServer(oauth2 -> oauth2
            .jwt(jwt -> jwt.decoder(jwtDecoder()))
        )
        .build();
}

@Bean
public JwtDecoder jwtDecoder() {
    return NimbusJwtDecoder.withJwkSetUri("https://auth-server/.well-known/jwks.json").build();
}
```

## 5.6 CORS Configuration
```java
@Bean
public CorsConfigurationSource corsConfig() {
    CorsConfiguration config = new CorsConfiguration();
    config.setAllowedOrigins(List.of("https://myapp.com"));
    config.setAllowedMethods(List.of("GET", "POST", "PUT", "DELETE"));
    config.setAllowedHeaders(List.of("*"));
    config.setAllowCredentials(true);
    config.setMaxAge(3600L);
    
    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/api/**", config);
    return source;
}
```

---

# SECTION VI: SPRING MICROSERVICES

---

## 6.1 Service Communication

| Method | When | Library |
|---|---|---|
| **REST (sync)** | Simple CRUD, low latency tolerance | `WebClient`, `RestClient`, `Feign` |
| **gRPC (sync)** | High performance, inter-service, schema enforcement | `grpc-spring-boot-starter` |
| **Messaging (async)** | Decoupled, event-driven, fire-and-forget | Spring Cloud Stream, Spring Kafka |

### Feign Client (Declarative REST)
```java
@FeignClient(name = "user-service", url = "${user-service.url}",
    fallbackFactory = UserClientFallbackFactory.class)
public interface UserClient {
    @GetMapping("/api/users/{id}")
    UserResponse getUser(@PathVariable Long id);
}
```

## 6.2 Service Discovery
| Technology | Type | Features |
|---|---|---|
| **Eureka** | Client-side | Heartbeat, self-preservation, Netflix ecosystem |
| **Consul** | Server-side | Health checks, KV store, service mesh |
| **K8s DNS** | Server-side | Native, no extra infrastructure |

## 6.3 API Gateway (Spring Cloud Gateway)
```java
@Bean
public RouteLocator routes(RouteLocatorBuilder builder) {
    return builder.routes()
        .route("user-service", r -> r
            .path("/api/users/**")
            .filters(f -> f
                .stripPrefix(1)
                .circuitBreaker(c -> c.setFallbackUri("/fallback"))
                .requestRateLimiter(rl -> rl.setRateLimiter(redisRateLimiter()))
                .retry(3)
                .addRequestHeader("X-Request-Id", UUID.randomUUID().toString()))
            .uri("lb://user-service"))
        .build();
}
```

## 6.4 Circuit Breaker (Resilience4j)
```java
@Service
public class OrderService {
    @CircuitBreaker(name = "inventory", fallbackMethod = "inventoryFallback")
    @Retry(name = "inventory", fallbackMethod = "inventoryFallback")
    @TimeLimiter(name = "inventory")
    @Bulkhead(name = "inventory")
    public CompletableFuture<InventoryResponse> checkInventory(Long productId) {
        return CompletableFuture.supplyAsync(() -> inventoryClient.check(productId));
    }
    
    private CompletableFuture<InventoryResponse> inventoryFallback(Long productId, Throwable t) {
        log.warn("Inventory service down, using fallback", t);
        return CompletableFuture.completedFuture(InventoryResponse.unknown());
    }
}
```

```yaml
resilience4j:
  circuitbreaker:
    instances:
      inventory:
        sliding-window-type: COUNT_BASED
        sliding-window-size: 10
        failure-rate-threshold: 50
        wait-duration-in-open-state: 10s
        permitted-number-of-calls-in-half-open-state: 3
        minimum-number-of-calls: 5
  retry:
    instances:
      inventory:
        max-attempts: 3
        wait-duration: 500ms
        exponential-backoff-multiplier: 2
  ratelimiter:
    instances:
      inventory:
        limit-for-period: 100
        limit-refresh-period: 1s
        timeout-duration: 0
```

## 6.5 Distributed Tracing
```yaml
# Micrometer Tracing + Zipkin
management:
  tracing:
    sampling:
      probability: 1.0  # 100% sampling (dev). Use 0.1 in prod
  zipkin:
    tracing:
      endpoint: http://zipkin:9411/api/v2/spans
```
- **Trace ID:** Unique per request, propagated across services
- **Span ID:** Unique per operation within a trace
- Headers: `traceparent` (W3C), `X-B3-TraceId` (Zipkin B3)

## 6.6 Event-Driven Architecture

### Spring Kafka
```java
// Producer
@Service
public class OrderEventPublisher {
    private final KafkaTemplate<String, OrderEvent> kafkaTemplate;
    
    public void publish(OrderEvent event) {
        kafkaTemplate.send("order-events", event.getOrderId(), event)
                .whenComplete((result, ex) -> {
                    if (ex != null) log.error("Failed to publish", ex);
                    else log.info("Published to partition {}", result.getRecordMetadata().partition());
                });
    }
}

// Consumer
@Service
public class InventoryEventConsumer {
    @KafkaListener(topics = "order-events", groupId = "inventory-service",
                   containerFactory = "kafkaListenerContainerFactory")
    public void onOrderPlaced(OrderEvent event, Acknowledgment ack) {
        try {
            inventoryService.reserve(event.getItems());
            ack.acknowledge();  // manual ack
        } catch (Exception e) {
            log.error("Failed to process", e);
            // don't ack → will be retried
        }
    }
}
```

### Transactional Outbox Pattern
```java
@Service
@Transactional
public class OrderService {
    public Order placeOrder(OrderRequest request) {
        Order order = orderRepo.save(mapToOrder(request));
        
        // Save event in same transaction as business data
        outboxRepo.save(new OutboxEvent(
            UUID.randomUUID(), "OrderPlaced", 
            objectMapper.writeValueAsString(order)));
        
        return order;
    }
}

// Separate process polls outbox table and publishes to Kafka
@Scheduled(fixedDelay = 1000)
public void publishOutboxEvents() {
    List<OutboxEvent> events = outboxRepo.findUnpublished();
    events.forEach(event -> {
        kafkaTemplate.send("order-events", event.getPayload());
        event.markPublished();
        outboxRepo.save(event);
    });
}
```

## 6.7 Distributed Configuration
- **Spring Cloud Config Server:** Git-backed centralized config
- **Consul KV:** Key-value store for config
- **Vault:** Secrets management (DB passwords, API keys, certificates)
- **Kubernetes ConfigMap / Secrets:** Native K8s config

## 6.8 Spring Cloud Stream (Binder Abstraction)
```java
@Bean
public Function<Flux<OrderEvent>, Flux<InventoryCommand>> processOrders() {
    return orderEvents -> orderEvents
            .filter(event -> event.getType() == EventType.PLACED)
            .map(event -> new InventoryCommand(event.getItems(), CommandType.RESERVE));
}
```
```yaml
spring:
  cloud:
    stream:
      bindings:
        processOrders-in-0:
          destination: order-events
          group: inventory-service
        processOrders-out-0:
          destination: inventory-commands
```

---

# SECTION VII: TESTING

---

## 7.1 Test Pyramid
```
         /  E2E Tests  \        — Few (Selenium, Playwright)
        / Integration    \      — Some (@SpringBootTest, Testcontainers)
       / Unit Tests        \    — Many (JUnit, Mockito)
      /____________________\
```

## 7.2 Unit Testing (JUnit 5 + Mockito)
```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {
    @Mock private OrderRepository orderRepo;
    @Mock private PaymentService paymentService;
    @InjectMocks private OrderService orderService;
    @Captor private ArgumentCaptor<Order> orderCaptor;

    @Test
    @DisplayName("should place order and process payment")
    void shouldPlaceOrder() {
        // Arrange
        var request = new OrderRequest("item-1", 2, BigDecimal.TEN);
        when(paymentService.charge(any())).thenReturn(PaymentResult.SUCCESS);
        when(orderRepo.save(any())).thenAnswer(inv -> {
            Order o = inv.getArgument(0);
            o.setId(1L);
            return o;
        });

        // Act
        OrderResponse response = orderService.placeOrder(request);

        // Assert
        assertThat(response).isNotNull();
        assertThat(response.getId()).isEqualTo(1L);
        assertThat(response.getStatus()).isEqualTo(OrderStatus.CONFIRMED);

        verify(paymentService).charge(any(PaymentRequest.class));
        verify(orderRepo).save(orderCaptor.capture());
        assertThat(orderCaptor.getValue().getItemId()).isEqualTo("item-1");
        verifyNoMoreInteractions(paymentService);
    }

    @ParameterizedTest
    @CsvSource({"1, 10.00", "5, 50.00", "10, 100.00"})
    void shouldCalculateTotal(int quantity, BigDecimal expectedTotal) {
        assertThat(orderService.calculateTotal(quantity, BigDecimal.TEN))
                .isEqualByComparingTo(expectedTotal);
    }

    @Test
    void shouldThrowWhenPaymentFails() {
        when(paymentService.charge(any())).thenReturn(PaymentResult.DECLINED);
        
        assertThatThrownBy(() -> orderService.placeOrder(request))
                .isInstanceOf(PaymentFailedException.class)
                .hasMessageContaining("Payment declined");
    }
}
```

## 7.3 Integration Testing

### Controller Layer (`@WebMvcTest`)
```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    @Autowired private MockMvc mockMvc;
    @MockBean private UserService userService;
    @Autowired private ObjectMapper objectMapper;

    @Test
    void shouldCreateUser() throws Exception {
        var request = new CreateUserRequest("John", "john@test.com", 25);
        var response = new UserResponse(1L, "John", "john@test.com");
        when(userService.create(any())).thenReturn(response);

        mockMvc.perform(post("/api/v1/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(request)))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.id").value(1))
            .andExpect(jsonPath("$.name").value("John"))
            .andExpect(header().exists("Location"));
    }

    @Test
    void shouldReturn400ForInvalidRequest() throws Exception {
        var request = new CreateUserRequest("", "invalid-email", -1);

        mockMvc.perform(post("/api/v1/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(request)))
            .andExpect(status().isBadRequest());
    }
}
```

### Repository Layer (`@DataJpaTest`)
```java
@DataJpaTest
@AutoConfigureTestDatabase(replace = Replace.NONE)  // use real DB
@Testcontainers
class UserRepositoryTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15");
    
    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Autowired private UserRepository userRepo;
    @Autowired private TestEntityManager em;

    @Test
    void shouldFindByEmail() {
        em.persist(new User("John", "john@test.com", 25));
        em.flush();

        Optional<User> user = userRepo.findByEmail("john@test.com");
        
        assertThat(user).isPresent()
                .get().extracting(User::getName).isEqualTo("John");
    }
}
```

### Full Integration (`@SpringBootTest`)
```java
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@Testcontainers
@AutoConfigureMockMvc
class OrderIntegrationTest {
    @Container static PostgreSQLContainer<?> postgres = ...;
    @Container static KafkaContainer kafka = ...;
    
    @Autowired private MockMvc mockMvc;
    @Autowired private OrderRepository orderRepo;
    
    @Test
    void shouldPlaceOrderEndToEnd() throws Exception {
        mockMvc.perform(post("/api/v1/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{...}"))
            .andExpect(status().isCreated());
        
        assertThat(orderRepo.count()).isEqualTo(1);
    }
}
```

## 7.4 Test Slices Summary

| Annotation | Loads | Use For |
|---|---|---|
| `@WebMvcTest` | Controller + filters + converters | REST API testing |
| `@DataJpaTest` | JPA + repositories + EntityManager | Repository/query testing |
| `@DataMongoTest` | Mongo repositories | MongoDB testing |
| `@WebFluxTest` | WebFlux controllers | Reactive API testing |
| `@JsonTest` | Jackson ObjectMapper | JSON serialization |
| `@RestClientTest` | RestTemplate/WebClient | External API mocking |
| `@SpringBootTest` | Full application context | End-to-end testing |

## 7.5 Testing Best Practices
- **Test naming:** `should_ExpectedBehavior_When_Condition()`
- **AAA pattern:** Arrange → Act → Assert
- **One assertion concept per test** (can have multiple `assertThat` for same concept)
- **Test data builder:** `UserBuilder.aUser().withName("John").withAge(30).build()`
- **Don't test frameworks** — test YOUR logic
- **Testcontainers** for real databases/queues in CI
- **Contract testing** (Spring Cloud Contract / Pact) for inter-service contracts

---

# SECTION VIII: PERFORMANCE & PRODUCTION

---

## 8.1 Connection Pooling (HikariCP)
```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20        # max connections
      minimum-idle: 5               # min idle connections
      idle-timeout: 300000          # 5 min idle before removal
      connection-timeout: 30000     # 30s wait for connection
      max-lifetime: 1800000         # 30 min max connection age
      leak-detection-threshold: 60000  # detect leaked connections
```

## 8.2 Caching Strategy
```yaml
spring:
  cache:
    type: redis  # or caffeine, ehcache
    redis:
      time-to-live: 3600000  # 1 hour
      cache-null-values: false
```
- **L1 (Local):** Caffeine — in-process, fastest, per-instance
- **L2 (Distributed):** Redis — shared across instances, network hop
- **Strategy:** Caffeine (L1) + Redis (L2) = near-cache pattern

## 8.3 Async Processing
```java
@Configuration
@EnableAsync
public class AsyncConfig {
    @Bean
    public Executor asyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10);
        executor.setMaxPoolSize(50);
        executor.setQueueCapacity(500);
        executor.setThreadNamePrefix("async-");
        executor.setRejectedExecutionHandler(new CallerRunsPolicy());
        executor.initialize();
        return executor;
    }
}

@Service
public class EmailService {
    @Async
    public CompletableFuture<Void> sendEmail(String to, String body) {
        // send email asynchronously
        return CompletableFuture.completedFuture(null);
    }
}
```

## 8.4 Scheduling
```java
@Configuration
@EnableScheduling
public class SchedulerConfig { }

@Service
public class CleanupService {
    @Scheduled(cron = "0 0 2 * * ?")      // 2 AM daily
    public void cleanupExpiredSessions() { }
    
    @Scheduled(fixedRate = 60000)           // every 60 seconds
    public void healthCheck() { }
    
    @Scheduled(fixedDelay = 30000)          // 30s after last completion
    public void processQueue() { }
}
```

---

**Total: 8 Sections | Spring Core to Microservices | Security | Data JPA | Testing | Production Patterns**

**Study Plan for Spring:**
1. Spring Core (IoC, DI, AOP, Events) — 3 days
2. Spring Boot (Auto-config, Actuator, Properties) — 2 days
3. Spring Web (REST, Validation, Exception Handling, Filters) — 3 days
4. Spring Data JPA (Queries, N+1, Transactions, Locking) — 3 days
5. Spring Security (JWT, OAuth2, Method Security) — 2 days
6. Microservices (Gateway, Circuit Breaker, Kafka, Tracing) — 3 days
7. Testing (JUnit5, Mockito, WebMvcTest, Testcontainers) — 2 days
8. Build mini-project applying all concepts — 3 days

**Total: ~3 weeks (2-3 hrs/day)** 🚀

