# Spring Security Architecture

## Overview

Spring Security is a powerful and customizable authentication and access-control framework for Java applications. It uses a filter-based architecture to intercept and secure HTTP requests.

## 1. Request Flow

Client → [Security Filter Chain] → DispatcherServlet → Controllers

- **Security Filter Chain**: A series of servlet filters provided by Spring Security.
- **DispatcherServlet**: Standard Spring MVC front controller.
- **Controllers**: Your application logic.

Filters run **before** requests reach your controllers.

## 2. Default Filters in Spring Security

When you include Spring Security in your project, it automatically registers several filters, such as:

- SecurityContextPersistenceFilter  
- UsernamePasswordAuthenticationFilter  
- BasicAuthenticationFilter  
- CsrfFilter  
- ExceptionTranslationFilter  
- FilterSecurityInterceptor  

These filters execute in a specific order and handle:

- Authentication  
- Authorization  
- Session management  
- CSRF protection  
- Exception handling  

## 3. Default Login Behavior

### UsernamePasswordAuthenticationFilter

This filter handles form login requests at `/login`. When Spring Security is used with no custom configuration:

- A default login page is provided.  
- Spring checks if `spring.security.user.password` is defined in `application.properties`.

#### If not defined:

- A **random password** is generated at runtime.  
- It is printed in the console log like:

    Using generated security password: a3f8bcd2-1d0a-4a0e-b829-3498de32167b

#### To set a fixed login:

    spring.security.user.name=admin  
    spring.security.user.password=secret123

## 4. CSRF (Cross-Site Request Forgery)

### What is CSRF?

An attacker can trick a logged-in user into sending a malicious request (POST, PUT, DELETE) to your app (e.g., via a malicious site or link). This can perform actions without the user’s consent.

### How Spring Protects Against CSRF

Spring Security **enables CSRF protection by default** for any non-GET/HEAD/OPTIONS requests.

To prevent CSRF attacks:

- A CSRF token is generated per session.  
- This token must be included in each modifying request (POST/PUT/DELETE).  
- Spring checks the presence and validity of this token before processing the request.

### How Is CSRF Token Sent to the Client?

#### 1. Hidden Field in Forms

For server-side rendered HTML with Spring MVC, Spring automatically includes a hidden input in forms:

    <input type="hidden" name="_csrf" value="token-value" />

This token is validated on form submission.

#### 2. Exposed as a Request Attribute

You can access the CSRF token in your view templates:

    CsrfToken token = (CsrfToken) request.getAttribute("_csrf");

#### 3. JavaScript (SPA/API) Clients

For REST APIs, use a CSRF token repository like `CookieCsrfTokenRepository`:

    http.csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse());

Spring will then:

- Send the CSRF token in a cookie (`XSRF-TOKEN`).  
- Expect clients to send it back in an HTTP header (e.g., `X-XSRF-TOKEN`).

This allows frontend frameworks like React or Angular to retrieve and send the token automatically.

## Summary

| Feature               | Description                                                              |
|-----------------------|--------------------------------------------------------------------------|
| Filter Chain          | Series of filters that run before controller logic                       |
| Default Login Filter  | `UsernamePasswordAuthenticationFilter` handles login at `/login`        |
| Random Password       | Generated if not set in `application.properties`                         |
| CSRF Protection       | Enabled by default; requires token on modifying requests                 |
| Token Delivery        | Via hidden form input, request attributes, or cookies                    |

## References

- [Spring Security Documentation](https://docs.spring.io/spring-security/reference/)
