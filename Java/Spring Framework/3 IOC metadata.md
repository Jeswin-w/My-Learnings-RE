# IOC Meta data

The spring's IOC container needs some meta-data to intiate the beans in the application context or bean factory.

There are several ways to define the application meta-data 

# Spring Bean Configuration Styles

Spring supports four main ways to define beans in its IoC container:

---

## 1. XML Configuration

**Description**: Traditional and explicit method using XML files.

**Example**:

    <beans xmlns="http://www.springframework.org/schema/beans">
        <bean id="myService" class="com.example.MyServiceImpl"/>
    </beans>

---

## 2. Java-based Configuration

**Description**: Uses Java classes annotated with `@Configuration` and methods annotated with `@Bean`.

**Example**:

    @Configuration
    public class AppConfig {
        @Bean
        public MyService myService() {
            return new MyServiceImpl();
        }
    }

---

## 3. Annotation-based Configuration

**Description**: Uses annotations like `@Component`, `@Service`, etc., and component scanning to detect and register beans.
no need to use @bean
**Example**:

    @Component
    public class MyServiceImpl implements MyService {
        // implementation
    }

And in your configuration:

    @Configuration
    @ComponentScan("com.example")
    public class AppConfig {
    }

---

## 4. Groovy-based Configuration

**Description**: Uses a Groovy DSL script to define beans in a concise, scriptable format.

**Example** (`beans.groovy`):

    beans {
        myService(com.example.MyServiceImpl)
    }

And load it with:

    ApplicationContext context = new GenericGroovyApplicationContext("beans.groovy");

---

For more details, refer to the official Spring documentation:  
🔗 [Spring Bean Configuration Basics](https://docs.spring.io/spring-framework/reference/core/beans/basics.html)
