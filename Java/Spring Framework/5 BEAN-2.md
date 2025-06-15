# Spring Bean Management – Part 2

This section covers important annotations for controlling bean lifecycle and dependency behavior in Spring.

---

## `@DependsOn`

Use `@DependsOn` when a bean depends on **static initialization or side effects** from another bean, even if it doesn’t directly reference it via `@Autowired`.

### 🔹 Example:

```java
@Component
public class LoggerInitializer {
    static {
        System.out.println("Logger initialized");
    }
}
```

```java
@Component
@DependsOn("loggerInitializer")
public class AuditService {
    public AuditService() {
        System.out.println("AuditService depends on LoggerInitializer");
    }
}
```

📝 **Use Case:** Forces Spring to initialize `LoggerInitializer` before `AuditService`.

---

## `@Lazy`

By default, Spring initializes all beans eagerly during context startup. Use `@Lazy` to delay initialization **until the bean is first requested**.

### 🔹 Lazy Bean Example:

```java
@Component
@Lazy
public class ExpensiveService {
    public ExpensiveService() {
        System.out.println("ExpensiveService loaded");
    }
}
```

```java
@Component
public class AppRunner {
    @Autowired
    private ExpensiveService service;

    public void run() {
        service.toString(); // triggers initialization
    }
}
```

> 🔸 Note: If a lazy bean is injected into a non-lazy bean, it will still be initialized eagerly.

### 🔹 Lazy All Beans via Configuration:

```java
@Configuration
@Lazy
@ComponentScan("com.example")
public class AppConfig {
    // all beans in this config will be lazy by default
}
```

---

## `@Autowired`

Injects a dependency automatically by **type**. It can be used on constructors, fields, or setters.

### 🔹 Field Injection:

```java
@Component
public class OrderService {

    @Autowired
    private PaymentProcessor paymentProcessor;
}
```

### 🔹 Constructor Injection (Recommended):

```java
@Component
public class OrderService {

    private final PaymentProcessor paymentProcessor;

    @Autowired
    public OrderService(PaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }
}
```

---

## `@Lookup`

Use `@Lookup` when you need a **new instance** of a prototype bean every time it is needed inside a singleton bean.

### 🔹 Example:

```java
@Component
public abstract class NotificationSender {

    @Lookup
    public abstract Notification createNotification();

    public void send(String message) {
        Notification notification = createNotification();
        notification.setMessage(message);
        notification.dispatch();
    }
}
```

```java
@Component
@Scope("prototype")
public class Notification {
    private String message;

    public void setMessage(String message) {
        this.message = message;
    }

    public void dispatch() {
        System.out.println("Sending: " + message);
    }
}
```

📝 Spring overrides the `@Lookup` method at runtime to return a fresh bean instance.

---

## ❌ When Autowiring Is Not Recommended

Autowiring is convenient, but should be avoided in certain cases:

| Scenario                                             | Recommendation                                      |
|------------------------------------------------------|-----------------------------------------------------|
| Multiple beans of same type causing ambiguity        | Use `@Qualifier` or define one as `@Primary`        |
| You need full visibility of dependencies             | Use explicit wiring via constructor or XML          |
| For primitive types (e.g., `int`, `String`)          | Set via configuration or properties file            |
| You want to generate documentation or diagrams       | Avoid autowiring; use explicit, named wiring        |
| Large, complex applications needing clear contracts  | Prefer constructor injection with clear interfaces  |

> ✅ Best practice: Use constructor injection with explicit parameters to make dependencies clear and testable.

---

## Summary Table

| Annotation     | Purpose                                                              |
|----------------|----------------------------------------------------------------------|
| `@DependsOn`   | Ensures a bean initializes after specific other beans                |
| `@Lazy`        | Delays bean creation until first use                                 |
| `@Autowired`   | Automatically injects dependencies by type                           |
| `@Lookup`      | Retrieves a new instance of a prototype bean from a singleton bean   |

