## TLS

TLS is a transport layer protocol (Layer 4) built over TCP.

TLS is used to encrypt the content being sent in TCP connection.

Once the TLS connection is setup the TLS handshake will occue.


## 🔐 TLS 1.2 Handshake Sequence

```mermaid
sequenceDiagram
    participant Client
    participant Server

    Client->>Server: ClientHello
    Server->>Client: ServerHello
    Server->>Client: Certificate,ServerKeyExchange (if needed), Certificate request (mtls if needed)
    Server->>Client: ServerHelloDone
    Client->>Server: ClientKeyExchange, Client Certiifcate (mtls if needed), ChangeCipherSpec
    Server->>Client: ChangeCipherSpec
    Note right of Client: Encrypted communication begins
```
![TLS 1.2 wireshark image](../../images/TLS1.2_wireshark.png)