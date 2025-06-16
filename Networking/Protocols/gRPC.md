# gRPC (google Remote Procedural call)

This protocol takes http2 to next level.

What problem this solves?

- Client libraries: for each protocol we need to ad new libraries, need to patch upgrade libraries

1 library for popular languages 

it uses http2 and protocol buffers

grpc modes:

1. Unary (either server or client streaming)

2. Server Streaming
3. CLient streaming
4. bidirectional (server and client streaming at same time)

