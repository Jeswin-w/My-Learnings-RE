# ACID Properties in Databases

ACID stands for **Atomicity**, **Consistency**, **Isolation**, and **Durability**. These are a set of properties that guarantee reliable processing of database transactions.

---

## 🚀 Transaction

A **transaction** is a sequence of operations (usually SQL queries) that are treated as a single unit of work. Either all operations succeed, or none do.

### Example: Transferring Money Between Accounts

1. SELECT balance FROM Account A
2. UPDATE Account A - deduct money
3. UPDATE Account B - deposit money

All the above steps should either complete successfully or none should be applied.

### Transaction Lifecycle

1. **BEGIN** – Start the transaction
2. **COMMIT** – Apply all changes
3. **ROLLBACK** – Undo all changes (in case of error)

> ⚡ PostgreSQL is known for fast commit performance.

### Read-Only Transactions

Use read-only transactions when you want a consistent **snapshot** of data as it existed at the start of the transaction. They are useful for reporting or analytics to avoid locking and improve performance.

> Even if you don’t manually begin a transaction, the database creates an implicit transaction per query and commits it immediately.

---

## 🧱 Atomicity

Atomicity ensures that all operations within a transaction are completed successfully. If any part fails, the entire transaction is rolled back.

### Key Points

- All operations in the transaction must succeed.
- If one fails, the entire transaction is rolled back.
- If the DB crashes before commit, the transaction is rolled back automatically.

### Storage Implication

- Some databases write to disk during query execution (faster commit).
- Others keep everything in memory until commit (faster rollback).

---

## 🔐 Isolation

Isolation ensures that concurrent transactions do not interfere with each other.

### Read Phenomena (Concurrency Issues)

1. **Dirty Read**
   - A transaction reads uncommitted changes from another transaction.
   - Example: T1 updates a row, T2 reads it before T1 commits. T1 rolls back, but T2 already read invalid data.

2. **Non-Repeatable Read**
   - A transaction reads the same row twice and sees different data.
   - Example: T1 reads a row, T2 updates it, T1 reads again – value has changed.

3. **Phantom Read**
   - A transaction reads a set of rows that match a condition. Another transaction inserts or deletes rows that match the condition during the first transaction.
   - Example: T1 queries rows between two dates. T2 inserts a new row that matches the condition. T1 queries again and sees a new “phantom” row.

4. **Lost Update**
   - Two transactions read and update the same data simultaneously. One update overwrites the other.
   - Example: T1 and T2 read the same value, both increment it, and save it. One update is lost.

> PostgreSQL handles isolation with MVCC (Multi-Version Concurrency Control), where each transaction gets a snapshot and does not overwrite others’ data.

---

## 🔒 Isolation Levels

Each level offers a different balance of performance vs. accuracy:

| Isolation Level     | Prevents                      | Description                                              |
|---------------------|-------------------------------|----------------------------------------------------------|
| **Read Uncommitted**| Nothing                       | Reads uncommitted changes from other transactions        |
| **Read Committed**  | Dirty Reads                   | Only sees committed changes (default in many DBs)        |
| **Repeatable Read** | Dirty + Non-Repeatable Reads  | Same row will not change within a transaction            |
| **Snapshot**        | Dirty + Non-Repeatable Reads  | Snapshot taken at transaction start (Postgres default)   |
| **Serializable**    | All phenomena (Full Isolation)| Ensures complete isolation, as if transactions run one-by-one |

### Locking Strategies

- **Pessimistic Concurrency**: Uses row/table/page locks to prevent conflicts.
- **Optimistic Concurrency**: No locking. Conflicts are detected at commit and transactions may fail.

> PostgreSQL:
> - Repeatable Read uses row-level locks on read.
> - Serializable uses an optimistic approach.
> - `SELECT FOR UPDATE` is a pessimistic mechanism to explicitly lock rows.

---

## 📏 Consistency

**Consistency** ensures that a transaction moves the database from one valid state to another, adhering to all defined rules.

### Ensured By

- Constraints (e.g., foreign keys, uniqueness)
- Data types and triggers
- Application logic
- Atomicity & Isolation (indirect contributors)

### Types of Consistency

1. **Data Integrity Consistency**  
   - Defined by schema & rules
   - Enforced through ACID properties

2. **Read Consistency**  
   - Whether a committed change is visible to other transactions
   - Affected by replication, isolation level, and caching
   - **Eventual consistency** is common in distributed systems

---


## 💾 Durability

**Durability** guarantees that once a transaction is committed, its changes are permanent—even in case of a crash.

### Durability Mechanisms

- **WAL (Write-Ahead Log)**  
  - Logs changes before applying them  
  - Enables recovery after crashes  
- **Append-Only Files (AOF)**  
  - Common in NoSQL (e.g., Redis)  
- **Snapshots**  
  - In-memory writes followed by batch disk writes  

### OS-Level Caching

- Disk writes may first go to **OS cache**, not immediately to disk.
- **fsync** forces flush to disk, ensuring durability.

> WAL avoids expensive full data writes by logging compressed, sequential changes.

---


## Summary Table

| ACID Property | Description | Example |
|---------------|-------------|---------|
| **Atomicity** | All-or-nothing execution | Bank transfer: Deduct + Add money must both succeed |
| **Consistency** | Valid state transitions | Enforcing foreign key or constraint rules |
| **Isolation** | Transactions don’t interfere | Reading stale data, dirty reads |
| **Durability** | Committed data persists | System crash after commit retains data |

---

## References

- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [ACID Transactions – Wikipedia](https://en.wikipedia.org/wiki/ACID)
- [Transaction Isolation – Martin Kleppmann’s Book Notes](https://martin.kleppmann.com/)

