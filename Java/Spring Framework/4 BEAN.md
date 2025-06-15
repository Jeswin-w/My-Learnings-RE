# Bean in Spring

A **bean** is an object managed by the Spring IoC container. Beans are defined using metadata (e.g., XML, annotations, or Java config). Internally, Spring stores this metadata as `BeanDefinition` objects.

---

## BeanDefinition Metadata

| Property                 | Explained in                |
|--------------------------|-----------------------------|
| Class                    | Instantiating Beans         |
| Name                     | Naming Beans                |
| Scope                    | Bean Scopes                 |
| Constructor arguments    | Dependency Injection        |
| Properties               | Dependency Injection        |
| Autowiring mode          | Autowiring Collaborators    |
| Lazy initialization mode | Lazy-initialized Beans      |
| Initialization method    | Initialization Callbacks    |
| Destruction method       | Destruction Callbacks       |

---

## Registering External Beans

You can register an object created **outside Spring** using:

- `getAutowireCapableBeanFactory()`
- `registerSingleton()` or `registerBeanDefinition()`

> ℹ️ Register early so the container can apply autowiring and lifecycle management properly.

---

## Bean Overriding

If two beans have the **same name**, the last one overrides the previous.

To disable overriding:

```java
context.setAllowBeanDefinitionOverriding(false);
```

Spring will then throw an exception for duplicate bean names.

---

## Naming Beans

- Beans can be named using `id`, `name`, or aliases.
- If omitted, Spring auto-generates a unique name.
- Names follow standard Java variable naming rules.

---

## Initializing Beans

Beans must have a `class` defined. They can be initialized via:

- **Constructor**
- **Factory method**

> 🛠 Factory method initialization is supported **only** via `@Bean` or XML — **not** with `@Component`, `@Service`, or similar annotations.

---

## Bean Properties / Dependencies

In XML configuration, you can inject:

- Primitive values (e.g., strings, numbers)
- Other beans
- Collections (e.g., lists, sets, maps, properties)

With **Java-based configuration** and annotations, this becomes easier and more type-safe.

Example using `@Bean` method:

```java
@Bean
public ComplexObject complexObject() {
    ComplexObject obj = new ComplexObject();
    obj.setName("example");
    obj.setAdminEmails(Map.of(
        "support", "support@example.org",
        "dev", "dev@example.org"
    ));
    return obj;
}
```

---

Spring provides flexible options for defining and wiring beans — XML, Java-based config, and annotations — choose the one that suits your project's structure and preferences.
