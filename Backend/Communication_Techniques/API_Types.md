# Introduction to APIs: SOAP, REST, and GraphQL

APIs (Application Programming Interfaces) enable communication between clients and servers.  
Here we explore three popular API styles: **SOAP**, **REST**, and **GraphQL**.

---

## 1. SOAP (Simple Object Access Protocol)

### Overview  
SOAP is a protocol for exchanging structured information in web services using XML. It relies on a strict messaging format and standards like WS-Security.

### Advantages
- **Strong standards and built-in error handling**  
- Supports **formal contracts** via WSDL (Web Services Description Language)  
- Supports advanced security through WS-Security  
- Works well with **stateful operations**

### Where It Is Used
- Enterprise-level applications requiring **high security and transactional reliability**  
- Legacy systems integration  
- Banking, telecommunication, and payment systems  

### Common Vulnerabilities
- XML-based attacks such as **XML External Entity (XXE)** attacks  
- **SOAP Action spoofing**  
- Overly verbose XML messages can cause **performance overhead**

---

## 2. REST (Representational State Transfer)

### Overview  
REST is an architectural style that uses standard HTTP methods (GET, POST, PUT, DELETE) to interact with resources, typically represented in JSON or XML.

### Advantages
- **Lightweight and easy to use**  
- Stateless interactions, improving scalability  
- Uses standard HTTP methods and status codes  
- Wide adoption and broad tool support  

### Where It Is Used
- Public web APIs (e.g., social media, cloud services)  
- Mobile and web applications  
- Microservices architectures  

### Common Vulnerabilities
- Improper authentication/authorization leading to **API abuse**  
- Lack of input validation causing **injection attacks** (e.g., SQL injection)  
- **Excessive data exposure** if filtering is not implemented correctly  

---

## 3. GraphQL

### Overview  
GraphQL is a query language and runtime for APIs that lets clients request exactly the data they need, minimizing over-fetching and under-fetching.

### Advantages
- Clients can specify precisely what data they want  
- Supports complex queries and aggregation in a single request  
- Strongly typed schema enabling better validation and tooling  
- Enables faster front-end development with flexible data fetching  

### Where It Is Used
- Modern web and mobile apps needing flexible, efficient data retrieval  
- Projects with complex or evolving data requirements  
- When frontend developers want more control over API data  

### Common Vulnerabilities
- Complex queries can lead to **Denial of Service (DoS)** via expensive queries  
- Insufficient authorization on nested queries may expose sensitive data  
- Overly permissive schemas can increase attack surface  

---

> ⚠️ **Summary:**  
> Each API style has strengths and weaknesses. Choose SOAP for strict contracts and security, REST for simplicity and wide support, and GraphQL for flexible, efficient data querying. Always implement robust authentication, authorization, and input validation to secure your APIs.

---
# 🔧 API Request & Response Examples (Expanded)

This section covers how clients interact with SOAP, REST, and GraphQL APIs.  
It includes request and response formats, error handling, and how authentication is commonly handled.

---

## 1. 🧼 SOAP Example

### ✅ Request (Get Weather)

```http
POST /weather HTTP/1.1
Host: api.example.com
Content-Type: text/xml; charset=utf-8
SOAPAction: "getWeather"

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:wea="http://weather.example.com/">
   <soapenv:Header/>
   <soapenv:Body>
      <wea:getWeather>
         <wea:city>London</wea:city>
      </wea:getWeather>
   </soapenv:Body>
</soapenv:Envelope>
```

### ✅ Response

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:wea="http://weather.example.com/">
   <soapenv:Body>
      <wea:getWeatherResponse>
         <wea:temperature>15°C</wea:temperature>
         <wea:condition>Cloudy</wea:condition>
      </wea:getWeatherResponse>
   </soapenv:Body>
</soapenv:Envelope>
```

### ❌ Error Response

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
   <soapenv:Body>
      <soapenv:Fault>
         <faultcode>soapenv:Client</faultcode>
         <faultstring>Invalid city parameter</faultstring>
      </soapenv:Fault>
   </soapenv:Body>
</soapenv:Envelope>
```

### 🔐 Authentication

SOAP often uses **WS-Security headers** in the XML body or relies on **HTTP Basic Auth**.

---

## 2. 🌐 REST Example

### ✅ Request (Get Weather)

```http
GET /weather?city=London HTTP/1.1
Host: api.example.com
Authorization: Bearer <your-token>
Accept: application/json
```

### ✅ Response

```json
{
  "city": "London",
  "temperature": "15°C",
  "condition": "Cloudy"
}
```

### ❌ Error Response

```json
{
  "error": "Invalid city",
  "code": 400
}
```

### 🔐 Authentication

- **Bearer tokens (OAuth 2.0)**
- **API keys** in headers or query strings
- **HTTP Basic Auth** (less common)

---

## 3. 🧠 GraphQL Example

### ✅ Request (Query Weather)

```http
POST /graphql HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer <your-token>

{
  "query": "{ weather(city: \"London\") { temperature condition } }"
}
```

### ✅ Response

```json
{
  "data": {
    "weather": {
      "temperature": "15°C",
      "condition": "Cloudy"
    }
  }
}
```

### ❌ Error Response

```json
{
  "errors": [
    {
      "message": "City not found",
      "path": ["weather"],
      "extensions": {
        "code": "CITY_NOT_FOUND"
      }
    }
  ],
  "data": null
}
```

### 🔐 Authentication

GraphQL APIs typically use:
- **Bearer tokens** in the `Authorization` header
- May also use **API keys** or **session cookies**

---

## 🛠️ Real-World Usage Summary

| API Style | Best For                        | Common Auth         | Typical Format | Strength                        |
|-----------|----------------------------------|----------------------|----------------|---------------------------------|
| **SOAP**     | Enterprise systems, secure apps | WS-Security, Basic Auth | XML            | Rigid standards, strong contracts |
| **REST**     | Web/mobile apps, public APIs   | OAuth, API keys       | JSON, XML       | Simplicity, scalability          |
| **GraphQL**  | Modern UIs, flexible queries   | OAuth, Bearer tokens  | JSON            | Query flexibility, efficiency    |

---


