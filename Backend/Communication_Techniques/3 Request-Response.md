# 📡 Request - Response

The **request-response** model is the most commonly used method to send and receive data across the internet.

---

## 🔄 Flow

1. 🧑‍💻 **Client** sends a request  
2. 🖥️ **Server** parses the request  
3. ⚙️ **Server** processes the request  
4. 🖥️ **Server** sends a response  
5. 🧑‍💻 **Client** parses the response  
6. 🧑‍💻 **Client** consumes the data  

---

## 🧩 What Happens in Parsing?

Data formats like **JSON**, **XML**, and **Protobuf** are parsed to extract meaningful information.

### Common Data Formats

| Format   | Speed        | Notes                                  |
|----------|--------------|----------------------------------------|
| Protobuf | 🚀 Fastest   | Efficient binary format by Google      |
| JSON     | ⚡ Fast       | Widely used, human-readable            |
| XML      | 🐢 Slower    | Verbose but highly structured via XSD  |

> 🔍 **XSD (XML Schema Definition)** is used to validate the structure of XML documents.

---

## 🌐 Where Are They Used?

These formats and the request-response model are used **everywhere on the internet**:

- 🔒 HTTPS, DNS, SSH
- 🔁 RPC (Remote Procedure Calls)
- 🗃️ SQL & Database Protocols
- 🔌 APIs: REST, SOAP, GraphQL

---

> 💡 Whether you're loading a web page, querying a database, or calling a cloud function — you're almost certainly using the request-response model behind the scenes.

---

## 🧬 Anatomy of a Request & Response

### 📤 HTTP Request

When a client sends a request, it typically includes:

| Part            | Description                                      |
|-----------------|--------------------------------------------------|
| **Method**      | HTTP verb like `GET`, `POST`, `PUT`, `DELETE`    |
| **URL**         | Target endpoint (e.g., `/api/users`)             |
| **Headers**     | Metadata (e.g., `Content-Type`, `Authorization`) |
| **Body**        | Optional data sent (e.g., JSON, form data)       |

📝 Example:

```http
POST /api/login HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer <token>

{
  "username": "john",
  "password": "secret"
}
```
---

## 🧬 Anatomy of a Request & Response

### 📤 HTTP Request

When a client sends a request, it typically includes:

| Part        | Description                                      |
|-------------|--------------------------------------------------|
| **Method**  | HTTP verb like `GET`, `POST`, `PUT`, `DELETE`    |
| **URL**     | Target endpoint (e.g., `/api/users`)             |
| **Headers** | Metadata (e.g., `Content-Type`, `Authorization`) |
| **Body**    | Optional data sent (e.g., JSON, form data)       |

#### 📄 Example Request:

``` 
HTTP/1.1 200 OK
Content-Type: application/json

{
"message": "Login successful",
"userId": 42
}
```

---

## 🧵 What is CRLF?

**CRLF** stands for **Carriage Return + Line Feed** (`\r\n`).  
It is used to separate lines in HTTP messages, as per the protocol defined in [RFC 7230](https://tools.ietf.org/html/rfc7230).

---

### ✉️ In HTTP Requests

Every part of an HTTP request is separated using `\r\n`:

```
GET / HTTP/1.1\r\n
Host: example.com\r\n
User-Agent: curl/7.64.1\r\n
\r\n
```

- Each **header line** ends with `\r\n`
- A **blank line (`\r\n\r\n`)** separates headers from the body
- If a body is present (e.g., in `POST` requests), it comes after this blank line

---

### 📤 In HTTP Responses

The structure is similar:

```HTTP/1.1 200 OK\r\n
Content-Type: text/plain\r\n
Content-Length: 13\r\n
\r\n
Hello, world!
```

---

### 🛡️ Why It Matters

- Correct CRLF usage ensures the HTTP message is parsed properly.
- Improper use can break communication or cause bugs.
- **CRLF Injection** is a security vulnerability where attackers insert headers or split the response using `\r\n`.

> 🔐 Always validate and sanitize inputs used in HTTP headers to prevent CRLF injection.

---
---

## ⚠️ What is CRLF Injection?

**CRLF Injection** is a security vulnerability where an attacker inserts malicious **Carriage Return + Line Feed** (`\r\n`) characters into user input that is later included in HTTP headers.

### How it works:

- The attacker injects `\r\n` sequences to **break the structure** of HTTP headers.
- This allows them to **add new headers**, **split responses**, or **manipulate server behavior**.

### Why it's dangerous:

- Can lead to **HTTP response splitting**.
- May enable **cross-site scripting (XSS)**, **cache poisoning**, or **session fixation** attacks.
- Exploits improper input validation and header construction.

### Example:

If a server blindly inserts user input into a header like:

If a server blindly inserts user input into a header like:

Set-Cookie: sessionId=USER_INPUT

and the attacker sets USER_INPUT to:

abc123\r\nSet-Cookie: admin=true

The server ends up sending **two cookies** instead of one:

Set-Cookie: sessionId=abc123
Set-Cookie: admin=true

This happens because the injected \r\n breaks the original header line into two separate headers, allowing the attacker to add arbitrary headers.

