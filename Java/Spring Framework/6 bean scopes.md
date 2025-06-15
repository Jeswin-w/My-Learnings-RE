# Bean scopes

 The Spring Framework supports six scopes, four of which are available only if you use a web-aware ApplicationContext. You can also create a custom scope.

The following table describes the supported scopes:

# Spring Bean Scopes

| **Scope**     | **Description**                                                                                   | **Context**                           |
|---------------|---------------------------------------------------------------------------------------------------|----------------------------------------|
| `singleton`   | (Default) A single bean instance per Spring IoC container.                                        | General                                |
| `prototype`   | A new bean instance is created each time it is requested.                                         | General                                |
| `request`     | A new bean instance per HTTP request.                                                             | Web-aware ApplicationContext only      |
| `session`     | A new bean instance per HTTP session.                                                             | Web-aware ApplicationContext only      |
| `application` | A single bean instance per `ServletContext`.                                                      | Web-aware ApplicationContext only      |
| `websocket`   | A new bean instance per WebSocket lifecycle.                                                      | Web-aware ApplicationContext only      |
| `thread`      | One bean instance per thread. Not registered by default; use `SimpleThreadScope` to enable.       | Requires manual registration           |

https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html