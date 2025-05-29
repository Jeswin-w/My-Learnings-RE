## 🧠 What is a Process?

- A **process** is a set of instructions (in machine language or assembly) that the CPU can execute.
- It has its own **isolated memory**, which cannot be accessed by other processes (except in certain cases like child processes sharing heap memory).
- Each process is uniquely identified by a **Process ID (PID)**.
- The **CPU scheduler** manages execution:
  - If a process has executable instructions, the CPU runs it.
  - If the process is waiting for data (e.g., from memory or disk), the CPU is reallocated to another ready process.

## 🧵 What is a Thread?

- A **thread** is the smallest unit of execution within a process. It is often referred to as a **lightweight process (LWP)**.
- A thread represents a **set of instructions** that can be scheduled for execution by the CPU.
- Unlike processes, **threads share the same memory space** of their parent process (including heap and data segments).
- Multiple threads within the same process can **access and modify shared data**, which introduces potential **race conditions** without proper synchronization.
- Each thread has its own **Thread ID (TID)**, **stack**, and **registers**, but **shares code, data, and open files** with other threads in the process.
- Threads are **independently scheduled** by the CPU, allowing for concurrent operations within a single process.

> Note ⚠️ : Java threads are mapped 1:1 to OS threads

## 🧵 Single-Threaded Process

- Node.js uses a single-threaded event loop model.
- Simplicity: no need to manage concurrency or synchronization between threads.
- Best suited for I/O-bound, non-blocking applications (e.g., web APIs).

---

## 🧩 Multiple Processes

- A single application can spawn multiple **processes**.
- Each process has its **own memory space** and **independent execution**.
- Examples:
  - **Nginx**: Uses multiple worker processes for handling requests.
  - **PostgreSQL / MySQL**: Spawn worker processes for client connections.

### 🧠 Shared Memory

- Some systems like **PostgreSQL** and **MySQL** allow **shared memory** segments for caching or coordination.
- This is **in addition** to each process’s private memory.

### ⚙️ Multi-core Utilization

- Each process can run on a **separate CPU core**, unlike a single-threaded model.
- Provides **parallelism** and improved performance on multi-core systems.

### 🧵 Copy-On-Write (COW) in Forking

- **Redis** uses a fork to create a child process for **snapshot backups** (RDB saves).
- Problem: While snapshotting, memory is changing, risking **inconsistency**.
- Solution: Use **Copy-On-Write (COW)**:
  - Memory **pages** are only copied if a write occurs.
  - This avoids duplicating the entire memory immediately.
  - Ensures the backup sees a **consistent** view of the memory at the time of fork.

---

## 📄 What Are Pages?

- Memory is divided into fixed-size blocks called **pages** (commonly 4 KB).
- The OS loads memory into RAM page by page.
- Pages help manage memory efficiently and allow **virtual memory** to work.

---

## 🌱 What Is Forking Memory?

- **Forking** is the act of creating a child process by duplicating the parent process.
- Initially, the child **shares the same pages** with the parent (via COW).
- When either process writes to a page, the OS:
  - **Copies** that page to new memory (Copy-On-Write).
- Efficient way to spawn processes without fully duplicating memory unless necessary.
- Forking is not in windows.

---

## 🧵 Multi-Threaded Process

- A **single process** contains **multiple threads**.
- All threads share the **same memory space**.
- Takes advantage of **multi-core CPUs** for parallel execution.
- Requires **less memory** than multiple processes since threads don’t duplicate memory.
- Introduces challenges like **race conditions**, requiring synchronization (e.g., **locks**, **latches**).

---

### ⚠️ Concurrency Issues in SQL Server

- **Primary Clustered Key**:  
  All inserts target the **same page** in the index.
  - Multiple threads inserting at once will each:
    - Lock the page
    - Insert data
    - Unlock the page
  - This results in **contention** on that page.
- 🔧 **Optimization**:
  - Use a **single thread** to batch and insert all rows, reducing lock contention and improving throughput.

---

### 🧪 Examples of Multi-Threaded Systems

- **Apache HTTP Server**
- **Apache Tomcat**
- **Microsoft SQL Server**

## How Many Processes or Threads Should You Use?

- **Number of CPU cores ≈ Number of processes or threads is usually ideal for CPU-bound applications.**
  - For example, on a machine with 4 CPU cores, running 4 processes or 4 threads allows full utilization of all cores.
  - Each process or thread can run on its own core simultaneously, maximizing CPU efficiency.

- **However, if your application involves many I/O or database operations, having more threads can improve performance.**
  - I/O and DB calls often cause threads to wait (blocked or sleeping) while waiting for data.
  - During this waiting time, CPU cores can run other threads.
  - This means having many threads helps keep CPU busy despite blocking operations.

- **Only as many threads as there are CPU cores can run at exactly the same time.**
  - On a 4-core machine, only 4 threads run truly in parallel at any moment.
  - Other threads are scheduled by the OS and get CPU time in slices (time-sharing).
  - This leads to context switching overhead but allows handling many concurrent requests.

- **Thread count tuning is about balancing:**
  - Maximizing CPU core utilization.
  - Minimizing context switching and overhead.
  - Ensuring enough threads to cover I/O wait times without excessive resource use.

- **In practice:**
  - For CPU-intensive apps, keep thread/process count close to the number of cores.
  - For I/O-heavy apps (like web servers, database clients), use a larger thread pool (sometimes 10x or more the number of cores).
  - Monitor performance metrics (CPU usage, response times, memory) and adjust accordingly.

---

**Summary Table**

| Scenario               | Recommended Threads/Processes             |
|------------------------|------------------------------------------|
| CPU-bound applications  | Number of cores (e.g., 4 on 4-core CPU)  |
| I/O-bound applications  | Many more threads (e.g., 100+ on 4-core) |

---

If you want, I can help you write a configuration example for Tomcat or NGINX based on this guidance. Just ask!  


