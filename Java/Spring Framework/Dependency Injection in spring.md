# 🌱 Dependency Injection in Spring

**Dependency Injection (DI)** is a design pattern used to achieve **loose coupling** between software components. Instead of creating dependencies within a class, they are provided (injected) from the outside. This improves code maintainability, testability, and scalability.

---

## 🤔 Why Avoid Tight Coupling?

Tightly coupled classes mean:
- 🔄 Changes in one class may require changes in others
- 🧪 Harder to test
- ♻️ Reduced reusability
- 🛠️ Difficult to maintain

---

## 💡 Types of Dependency Injection

Spring supports three main types of dependency injection:

- ✅ **Constructor Injection** (Recommended)
- 🔧 **Setter Injection**
- 🚫 **Field Injection** (Less preferred)

### 🏗️ Constructor Injection

Use when the dependency is **required** for the class to function.

    @Component
    public class OrderService {
        private final PaymentService paymentService;

        public OrderService(PaymentService paymentService) {
            this.paymentService = paymentService;
        }
    }

### ⚙️ Setter Injection

Use when the dependency is **optional** or can be changed post-creation.

    @Component
    public class OrderService {
        private PaymentService paymentService;

        @Autowired
        public void setPaymentService(PaymentService paymentService) {
            this.paymentService = paymentService;
        }
    }

---

## 🌟 How Spring Helps with DI

Imagine a class with 3 dependencies, each having 3 more. Manual instantiation becomes complex.

### 🧠 Enter Spring IoC

- **IoC (Inversion of Control):** Spring creates and manages objects for you.
- **Beans:** Java objects managed by Spring.

This simplifies object creation and wiring using Spring’s **IoC container**.

---

## 🛠️ Creating and Registering Beans

### 1. 🧩 Using Annotations

#### `@Component` 🔹

Marks a class as a Spring-managed component.

    @Component
    public class EmailService {
        // ...
    }

#### `@Service` 🟢

Specialized `@Component` for service-layer logic.

    @Service
    public class PaymentService {
        // ...
    }

#### `@Repository` 🟡

Specialized `@Component` for data access layer.

    @Repository
    public class OrderRepository {
        // ...
    }

#### `@Controller` 🟠

Marks web-layer controller classes in Spring MVC.

    @Controller
    public class HomeController {
        // ...
    }

#### `@Autowired` 🧷

Injects dependencies automatically. Can be used on:
- Constructor ✅
- Setter 🔧
- Field ❌ (not recommended)

    @Autowired
    private EmailService emailService;

AUtowired can be ignored if there is only one constructor in latest spring.

---

### 2. 🧾 Java Configuration (`@Configuration` + `@Bean`)

    @Configuration
    public class AppConfig {

        @Bean
        public EmailService emailService() {
            return new EmailService();
        }

        @Bean
        public OrderService orderService() {
            return new OrderService(emailService());
        }
    }

---

### 3. 📄 XML Configuration (Legacy)

    <bean id="emailService" class="com.example.EmailService"/>
    <bean id="orderService" class="com.example.OrderService">
        <constructor-arg ref="emailService"/>
    </bean>

---

## 📝 Summary Table

| 🔖 Concept         | 💬 Description                                      |
|--------------------|-----------------------------------------------------|
| **DI**             | Injecting dependencies from the outside             |
| **IoC**            | Spring controls object creation                     |
| **Bean**           | Spring-managed object                               |
| `@Component`       | Generic Spring-managed bean                         |
| `@Service`         | Service-layer bean                                  |
| `@Repository`      | Data access layer bean                              |
| `@Controller`      | Web controller bean                                 |
| `@Autowired`       | Annotation for dependency injection                 |

---

✅ **Best Practice:** Favor **constructor injection** for mandatory dependencies. Use `@Service`, `@Component`, and other annotations to keep your configuration minimal and clear.

---

Let me know if you'd like to extend this with:
- `@Qualifier` for multiple bean selection
- `@Primary`
- `@PostConstruct` and `@PreDestroy` lifecycle hooks
- Spring Boot DI examples
