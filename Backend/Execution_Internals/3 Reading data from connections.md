# Internals of How Data Is Read After Connections

While most of the heavy lifting is done by libraries (like `libc`, `OpenSSL`, or language runtimes), it’s important to understand the boundary between what these libraries do and what your application directly controls.

---

## Send and Receive Buffers in TCP Communication

TCP communication involves **kernel-managed buffers** for both sending and receiving data. The application and the kernel coordinate through system calls like `read()` and `send()`.

---

### 🔹 Receive Path (Inbound Data Flow)

1. **Client sends data** over an established TCP connection.
2. The **kernel receives the packets** and places them in the socket's **receive buffer** (a queue allocated per connection).
3. The kernel then **sends an ACK** back to the sender:
   - This may be **delayed** due to the **Delayed ACK** optimization, which waits briefly to batch ACKs or piggyback them on outbound data.
   - The ACK also advertises the **window size**, indicating how much more space is available in the receive buffer (this is part of TCP flow control).
4. The **application calls `read()`**, which:
   - Copies data from the kernel’s receive buffer into the application’s user-space memory.
   - Removes the corresponding data from the kernel buffer.
   - Updates the TCP window to allow the sender to transmit more data.
5. If there is **no data available**, `read()`:
   - **Blocks** the thread by default (in blocking mode).
   - Can be monitored non-blockingly using **`epoll`**, which notifies the app when the socket is readable.
   - Can be made more efficient using **`io_uring`**, which supports asynchronous I/O and allows **zero-copy** techniques.

---

### 🔹 Send Path (Outbound Data Flow)

1. The **application calls `send()`**, passing data to the kernel.
2. The kernel **copies the data** to the **send buffer**.
3. The **TCP stack transmits** the data in segments over the network.
4. The data remains in the buffer until:
   - The receiver **ACKs** the data.
   - At that point, the kernel removes acknowledged data from the send buffer.

---

### 🔸 What Is the Sliding Window in TCP?

The **TCP sliding window** is a dynamic mechanism for **flow control**. It ensures that:
- The **sender does not overwhelm the receiver**.
- The receiver controls how much data it can accept by advertising the size of its receive window.
- As data is read by the receiving application, the window “slides forward,” allowing the sender to send more.

It involves:
- **Sender’s view**: How much unacknowledged data it can send.
- **Receiver’s view**: How much buffer space remains.

---

## ⚠️ Potential Problems and Bottlenecks

If the **application is slow to read** from the receive buffer:
- The buffer **fills up**.
- The **TCP window shrinks to zero**.
- The kernel **stops acknowledging new data** beyond the window size.
- The **client experiences backpressure** — its send calls may block or slow down due to TCP flow control.

This illustrates why **slow receivers can throttle senders**, even if the network is fast.

---

