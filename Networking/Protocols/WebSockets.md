# WebSockets – Interview Notes

## What is WebSocket?
WebSocket is a **persistent, full-duplex communication protocol** that allows bidirectional data exchange between a client and a server over a single TCP connection.

---

## Protocols
- `ws://` → WebSocket (plaintext)
- `wss://` → Secure WebSocket (WebSocket over TLS)

---

## WebSocket Handshake

WebSocket starts as an **HTTP request** and is upgraded to a WebSocket connection.

### Client → Server
```http
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
```

### Server → Client
```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
```

After `101 Switching Protocols`, HTTP is no longer used — communication happens using WebSocket frames.

---

## Key Characteristics
- Full-duplex (client and server can send messages anytime)
- Persistent connection
- Low latency
- Message-based (not request–response)

---

## Pros
- True **bidirectional communication**
- Low latency compared to polling
- Reduced overhead (single persistent connection)
- HTTP-compatible handshake
- Firewall friendly (ports 80 / 443)

---

## Cons
- Harder to **scale horizontally**
  - Connections are stateful
  - Requires sticky sessions or external coordination
- Load balancer idle timeouts
  - Requires ping/pong keep-alives
- Proxy compatibility issues
- Not suitable for simple request-response APIs

---

## Scaling WebSockets (Interview Favorite)

### Problems
- Each client maintains a long-lived connection
- Requests cannot be freely load-balanced

### Common Solutions
- Sticky sessions at the load balancer
- External pub/sub systems
  - Redis Pub/Sub
  - Kafka
  - RabbitMQ
- Connection registry (user → server mapping)

---

## Ping / Pong
- Used to keep connections alive
- Detect broken or idle connections
- Prevent L7 load balancer timeouts

---

## WebSocket vs HTTP

| Feature | HTTP | WebSocket |
|------|------|-----------|
| Connection | Short-lived | Persistent |
| Direction | Client → Server | Bidirectional |
| Latency | Higher | Low |
| State | Stateless | Stateful |
| Use case | REST APIs | Real-time systems |

---

## WebSocket vs SSE (Server-Sent Events)

| Feature | WebSocket | SSE |
|------|---------|-----|
| Direction | Bidirectional | Server → Client |
| Protocol | Custom | HTTP |
| Complexity | Higher | Simpler |
| Scaling | Harder | Easier |
| Use case | Chat, gaming | Notifications, feeds |

---

## Common Use Cases
- Real-time chat
- Live notifications
- Stock price updates
- Multiplayer games
- Collaboration tools

---

## Interview One-Liners
- “WebSocket is a full-duplex, persistent protocol upgraded from HTTP.”
- “Scaling WebSockets requires sticky sessions or external pub/sub.”
- “Ping/pong frames prevent load balancer timeouts.”
- “WebSockets are message-based, not REST.”

---

## When NOT to Use WebSockets
- Simple CRUD APIs
- Infrequent updates
- Stateless microservices
- Strict proxy-controlled networks

---

## Summary
WebSockets are ideal for **real-time, low-latency communication**, but require **careful handling of scaling, load balancing, and connection lifecycle** in production.
