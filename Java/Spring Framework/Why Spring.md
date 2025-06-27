
# Why Use Spring When Java EE is Available

### Traditional Java EE Approach (Servlet-Based)

* When a request comes in, it is handled by **Tomcat** (a servlet container).
* Tomcat looks up the **`web.xml`** file to find the correct **servlet mapping** based on the URL.
* The mapped **servlet** handles the request and executes logic based on the HTTP method (GET/POST).

#### Problems with Java EE

* **`web.xml` becomes large and hard to manage**:

  * Contains mappings for servlets, filters, listeners, etc.
* Harder to manage applications with complex routing and logic.
* Tight coupling between components makes testing harder.

---

## Spring Framework

Spring simplifies Java web development using annotations and better design principles.

### Request Flow in Spring MVC

1. Request comes to **Tomcat**.
2. Tomcat forwards it to the **DispatcherServlet**.
3. **DispatcherServlet** uses `HandlerMapping` to find the correct **Controller**.
4. Spring creates the Controller object and uses **IoC (Inversion of Control)** to inject dependencies.
5. The method annotated with `@GetMapping` or `@PostMapping` is invoked.

### Key Concepts

#### Inversion of Control (IoC)

* Maintains object dependencies via **Dependency Injection (DI)**.
* Promotes **loose coupling** and **easier unit testing**.

#### Configuration Annotations

* Use annotations like `@Configuration`, `@Bean`, etc., to define beans and app setup.

#### Component Scan

* `@ComponentScan` tells Spring where to look for components like Controllers, Services, etc.

### Setting Up a Spring MVC App (Manually)

You typically need:

* A **config class** (with `@Configuration`)
* A **controller class** (with `@Controller`)
* A `pom.xml` for Maven dependencies
* Extend **DispatcherServlet** and configure it in `web.xml`

---

## Spring Boot

Spring Boot builds on top of Spring and simplifies the entire setup.

### Key Features

* **Dependency Management**:

  * Uses **Spring Boot Starter POMs** to manage compatible library versions.

* **Auto Configuration**:

  * Automatically sets up DispatcherServlet, AppConfig, and ComponentScan.
  * Uses `@SpringBootApplication` to combine:

    * `@Configuration`
    * `@EnableAutoConfiguration`
    * `@ComponentScan`

* **Embedded Server**:

  * Includes **embedded Tomcat**, no need to deploy to an external server.

### Pros of Spring Boot

* Fast development and production-ready apps.
* **Convention over configuration** — sensible defaults reduce boilerplate.
* Fully based on Spring, so all Spring features are available.
* Just run the main class and the app is live (`public static void main`).

---

