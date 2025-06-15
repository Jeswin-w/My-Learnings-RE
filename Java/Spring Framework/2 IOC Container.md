# Inversion of Control (IoC)

- **IoC** is a design principle where the control of object creation and dependency management is transferred from the object itself to a container or framework.
- In the context of Spring, **IoC is implemented via Dependency Injection (DI)**.
- **Dependencies are injected automatically** when an object is created as a Spring-managed **bean**.
- **Beans** are objects that are created and maintained by Spring’s IoC container.

## IoC Containers in Spring

- Spring provides two main types of IoC containers:
  - **BeanFactory**
  - **ApplicationContext**

- `ApplicationContext` is a **superset** of `BeanFactory`, offering additional features such as event propagation, internationalization, and annotation support.

---

## BeanFactory vs ApplicationContext

| **Feature**              | **BeanFactory**                                                                 | **ApplicationContext**                                                                 |
|--------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| **Definition**           | Basic container providing core bean management capabilities.                    | Advanced container extending BeanFactory with additional enterprise-level features.     |
| **Usage**                | Suitable for simple, standalone applications.                                   | Suitable for enterprise apps, including web, AOP, ORM, and distributed systems.         |
| **Bean Scopes Supported**| Supports only `singleton` and `prototype` scopes.                               | Supports all scopes: `singleton`, `prototype`, `request`, `session`, `application`, etc.|
| **Annotation Support**   | Does **not** support annotations; configuration must be done in XML.            | Fully supports annotation-based configuration and autowiring.                          |
| **Internationalization** | Not supported.                                                                  | Supports i18n through the `MessageSource` interface.                                    |
| **Event Handling**       | Not supported.                                                                  | Supports event publishing using `ApplicationEvent` and `ApplicationListener`.           |
| **Bean Post-Processing** | Requires manual registration of post-processors.                                | Automatically registers `BeanPostProcessor` and `BeanFactoryPostProcessor`.             |
| **Initialization**       | Beans are created lazily (on demand).                                           | Beans are created eagerly (at startup), unless marked as lazy.                          |
| **Resource Usage**       | Lightweight and low memory usage.                                               | Slightly heavier, suitable for enterprise-scale applications.                           |

---