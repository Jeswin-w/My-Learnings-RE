# 🗃️ Primary Key vs Secondary Key in Databases

Understanding the **subtle but critical** differences between **primary** and **secondary keys (indexes)** is essential for designing efficient and scalable database systems.

This document covers:
- Heap storage  
- Indexing structures  
- Clustering and performance trade-offs  
- Differences in implementation across major DBMSs  

---

## 📌 What is a Primary Key?

A **Primary Key**:
- Enforces **uniqueness** for a column or a combination of columns  
- **Cannot be NULL**  
- Typically used as the **main way to identify a row**  
- May cause the table to be **physically organized** around this key (depending on the database)

---

## 🧱 Heap Tables and Tablespaces

### 🏗️ Heap Storage

- A **heap** is a storage structure where rows are placed in the order they are inserted
- There is **no implicit order** unless you enforce one
- This is the default for many databases unless a clustered index is used

**Example (heap insert order):**  
- INSERT INTO users(id) VALUES (7);  
- INSERT INTO users(id) VALUES (1);  
- INSERT INTO users(id) VALUES (2);  
Result: Stored in insert order, not by `id`

### 📂 Tablespaces

- A **tablespace** is a logical storage unit that can contain tables and indexes
- You can configure which tables go where, helping with I/O optimization

---

## 🧭 Clustered Indexes (Primary Key Clustering)

### 🔄 What is Clustering?

- Clustering **physically organizes** rows on disk based on a given index (often the primary key)
- Benefits:
  - Fast **range queries**
  - Better **I/O performance**
  - **Predictable row locality** in storage

This means the table is no longer a heap but **an index-organized structure**

### 🏛️ Examples by DBMS

| Database      | Primary Key Clusters Table? | Can Create Clustered Index Separately? |
|---------------|-----------------------------|----------------------------------------|
| PostgreSQL    | ❌ (by default)              | ✅ via `CLUSTER` command                |
| MySQL (InnoDB)| ✅ (required)                | ❌ always clustered by PK               |
| Oracle        | ❌ (optional via IOT)        | ✅ supports IOT                         |
| SQL Server    | ✅ (default)                 | ✅ can define clustered/non-clustered   |

---

## 🔁 Insert Costs with Clustered Index

- Inserting out-of-order values (e.g., inserting `2` after `8`) causes page splits or shifts
- Databases pre-allocate space (**fill factor**) to reduce overhead

**Warning on random primary keys (e.g., UUIDs):**
- Cause scattered inserts
- Increase fragmentation
- Reduce cache locality
- Degrade clustering performance

---

## 🔍 Secondary Indexes (Non-Clustered)

A **Secondary Index**:
- Exists as a **separate data structure** (e.g., a B-tree)
- Stores:
  - Indexed column values
  - Pointers (row IDs/TIDs) to actual rows in the heap

### 🔨 Access Flow

1. Use index to find **matching keys** and **row locations**
2. Go to the heap to retrieve the full row

This two-step process is known as **bookmark lookup** or **row lookup**

### 🐌 Downsides

- **Slower** for `SELECT *` or wide column retrieval
- Requires **double I/O**:
  - First to scan index
  - Second to fetch from heap

---

## 🔁 Range Queries and Access Speed

| Query Type                      | Clustered Index | Secondary Index |
|----------------------------------|------------------|------------------|
| `SELECT * WHERE id BETWEEN 1 AND 10` | ✅ Very fast     | ❌ Slower         |
| `SELECT * WHERE name = 'Alice'`      | ✅ if clustered  | ✅                |
| Full Table Scan (`SELECT *`)        | ✅ Faster        | ❌ Less efficient |

---

## ⚙️ When to Use What?

### ✅ Use Clustered Index (Primary Key) When:
- You frequently query by **ID or ranges**
- You care about **read performance**
- Inserts are generally **in order**

### ✅ Use Secondary Index When:
- You query by **non-primary** columns
- You need **multiple indexes**
- Table order doesn’t matter

---

## 🧠 Database-Specific Notes

### PostgreSQL
- All indexes are **secondary**
- You can manually cluster a table using:
  - `CLUSTER my_table USING my_index;`
- Clustering is **not automatic** and must be re-run as needed

### MySQL (InnoDB)
- Always clusters the table by the **primary key**
- If no primary key:
  - Uses first `UNIQUE NOT NULL` index
  - Or creates a hidden auto-increment column

### Oracle
- Supports **Index Organized Tables (IOT)**
  - No separate heap; data lives in the index
- Heap tables with secondary indexes are the default

### SQL Server
- Allows:
  - **Clustered** index (only one per table)
  - **Heap** tables with multiple **non-clustered** indexes

---

## 🛠️ Advanced Concepts

### 🔄 Rebuilding and Maintenance
- Secondary indexes can fragment and require:
  - `REINDEX` or `VACUUM` (PostgreSQL)
  - `OPTIMIZE TABLE` (MySQL)
  - `ALTER INDEX REBUILD` (SQL Server)

### 🧩 Composite Indexes
- Indexes can span multiple columns
- Example: Create an index on (user_id, order_date)

### 📉 Index-only Scans
- If all queried columns exist in the index:
  - The heap doesn’t need to be accessed
  - Known as a **covering index**

---

## 📋 Summary Table

| Feature                      | Primary (Clustered) Index     | Secondary (Non-Clustered) Index |
|------------------------------|-------------------------------|----------------------------------|
| Enforces Uniqueness          | ✅                            | ❌ (can use UNIQUE)              |
| Physically Orders Table      | ✅                            | ❌                              |
| Storage Efficiency           | ✅ for range queries          | ❌                              |
| Lookup Performance           | ✅ (1 I/O)                    | ❌ (2 I/O steps)                |
| Insert Overhead              | High (if out-of-order)        | Lower                          |
| Range Query Speed            | ✅                            | ❌                              |
| Multiple per Table?          | ❌ (1 max)                    | ✅ (many)                       |

---

## 🔚 Final Thoughts

- Choose the right index based on:
  - **Query pattern**
  - **Data volume**
  - **Write vs read intensity**
- Don’t use random primary keys if clustering is important
- Benchmark your indexes with tools like:
  - `EXPLAIN ANALYZE` (PostgreSQL)
  - `EXPLAIN` (MySQL)
  - SQL Server Execution Plan Viewer

---

