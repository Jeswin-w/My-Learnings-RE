# gRPC (Google Remote Procedure Call)

gRPC is a high-performance, open-source universal RPC framework developed by Google. It builds upon HTTP/2 and uses Protocol Buffers (Protobuf) as its Interface Definition Language (IDL) and serialization format.

## 🚀 What Problem Does gRPC Solve?

Traditional REST APIs often require:

- Separate client libraries for different protocols
- Manual patching and version upgrades
- Redundant boilerplate code

gRPC addresses this by offering:

- One unified client library per language
- Strongly-typed schema via Protobuf
- Efficient binary communication over HTTP/2
- Native streaming support

## 🔌 gRPC Communication Modes

1. Unary RPC  
   Single request and single response

2. Server Streaming  
   Client sends one request, receives a stream of responses

3. Client Streaming  
   Client sends a stream of requests, receives one response

4. Bidirectional Streaming  
   Both client and server send streams of messages concurrently

## 🧬 Example .proto File

```proto
syntax = "proto3";

package x;

service Todo {
  rpc createTodo (TodoItem) returns (TodoItem);
  rpc readTodos (Void) returns (TodoItems);
  rpc readTodoStreak (Void) returns (stream TodoItem);
}

message Void {}

message TodoItem {
  int32 id = 1;
  string text = 2;
}

message TodoItems {
  repeated TodoItem items = 1;
}
```

## ✅ Pros of gRPC

1. **High Performance**  
   Uses HTTP/2 and Protobuf for fast, compact binary communication

2. **Unified Client Libraries**  
   Single client library per language reduces duplication and errors

3. **Streaming Support**  
   Enables real-time upload/download or event-based architectures

4. **Request Cancellation**  
   HTTP/2 allows mid-flight cancellation

5. **Modern Stack**  
   Efficient, contract-first APIs with support for multiplexing

## ❌ Cons of gRPC

1. **Requires Protobuf Schema**  
   Developers must define and maintain `.proto` files

2. **Thick Clients**  
   Client setup is heavier compared to simple REST calls

3. **Poor Proxy Support**  
   Binary formats and HTTP/2 aren't friendly with traditional HTTP proxies

4. **Limited Error Handling**  
   gRPC doesn't offer standardized error models like REST (HTTP codes)

5. **No Native Browser Support**  
   Requires gRPC-Web to run in browsers

6. **Streaming Timeouts**  
   Long-lived streams can suffer disconnects or timeouts in pub/sub scenarios
