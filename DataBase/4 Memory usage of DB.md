# 📦 How Tables and Views Are Stored in a Database

## 📌 1. Row ID / Tuple ID

Every row in a database has an internal identifier:

- **PostgreSQL**: Called `ctid`, a **tuple ID** indicating physical location as `(block_number, row_offset)`.
- **MSSQL (SQL Server)**: Uses a **Row Identifier (RID)** format: `File:Page:Slot`, uniquely pointing to a row.
- **MySQL (InnoDB)**: Stores the **primary key** value as a clustered index; no separate row ID is used.

> Even if you define a primary key, the DB still tracks an internal ID for physical access efficiency.

---

## 📄 2. Pages

- **Rows are stored in fixed-size pages**.
  - **PostgreSQL**: 8KB
  - **MySQL (InnoDB)**: 16KB
  - **SQL Server**: 8KB
- A page includes:
  - **Page Header** (metadata)
  - **Row Slots**
  - **Free Space**
- Pages are the **atomic unit of I/O**, not individual rows.

> A read always fetches the entire page, so optimizing what rows go into each page is important for performance.

---

## 💾 3. Disk I/O

- **I/O operations** fetch data from disk into memory.
- DBs don't read rows — they read **pages**, possibly in bulk.
- If not cached, a page read = **1 disk I/O**.
- Some I/O requests hit the **OS page cache**, avoiding physical disk reads.

### Ways to Minimize I/O:
- Use **indexes**.
- Use **covering indexes** (with all queried columns).
- Keep frequently read data **small and hot** (i.e., in cache).
- Use **sequential scans** for large data blocks instead of many random I/Os.

---

## 🗃️ 4. Heap

- The **heap** is where the actual table data (rows) is stored.
- It is an unordered collection of pages.
- Expensive to search large tables in heap without an index.
- Updating a row may require placing a new version of it elsewhere (e.g., PostgreSQL’s MVCC model).

---

## 🔍 5. Indexes

Indexes are **data structures** (usually B-Trees) used to speed up lookups:

- Indexes contain:
  - **Indexed columns**
  - **Pointers** to the **heap location** (Page ID + Row ID or Primary Key)
- Types:
  - **Primary Index**: Built on the primary key.
  - **Secondary Index**: Built on non-PK columns.

> Indexes are stored in pages, too, and incur I/O overhead, especially when large or fragmented.

### Popular Index Structures:
- **B-Tree**: Balanced tree structure for range and equality queries.
- **Hash Index**: For equality-only lookups.
- **GIN / GiST** (PostgreSQL): For full-text and complex queries.
- **Clustered Indexes**: The table **data itself is stored in the index order**.
- **Non-clustered Indexes**: Points to the heap or primary key.

---

## 🧬 6. Clustered vs. Non-Clustered Storage

| DB        | Clustered Index Behavior                                                   |
|-----------|-----------------------------------------------------------------------------|
| **MySQL InnoDB** | Always clustered by **primary key**. All other indexes point to PK.            |
| **PostgreSQL**   | Does **not support clustered tables** natively. All indexes are **secondary**. |
| **MSSQL**        | Allows **clustered indexes** (one per table); can be defined on any column.     |

> In PostgreSQL, **all indexes point to the `ctid` (tuple ID)**. So, if you update a row (which creates a new tuple), **all secondary indexes must be updated**.

> In MySQL, other indexes just point to the **primary key**, so as long as the PK remains the same, you avoid widespread index updates.

---

## ❓ Frequently Asked Questions

### Q: Why does PostgreSQL update all indexes on row update?

PostgreSQL uses MVCC (Multi-Version Concurrency Control). Updating a row **does not modify it in place** — it **creates a new version** of the row with a new `ctid`. Since indexes point to `ctid`, all secondary indexes must now reference the new one.

In contrast, **MySQL InnoDB** points secondary indexes to the **primary key**, so updates only affect the index if the **PK value changes**.

---

### Q: Why avoid UUIDs in MySQL as Primary Keys?

- UUIDs are **random**, leading to:
  - Poor page locality (inserts go all over the B-Tree).
  - High index fragmentation.
  - Increased I/O.

> **Sequential integer keys (e.g., AUTO_INCREMENT)** are much more storage and cache friendly.

---

### Q: What happens if a table has no index?

- The DB performs a **sequential scan** (a.k.a. full table scan).
- It reads all pages of the heap.
- Can be **very slow** for large tables unless the whole table is cached in RAM.
- Some databases (like PostgreSQL) may **parallelize** the scan across cores to improve performance.

---

### Q: Do Views take up storage?

- **Views** are virtual.
- They do **not** store data unless you use **Materialized Views**.
- A materialized view is like a cached result set and **does consume storage**.

---

## 📚 Additional Reading

- PostgreSQL Storage Internals: [https://www.postgresql.org/docs/current/storage.html](https://www.postgresql.org/docs/current/storage.html)
- MySQL InnoDB Architecture: [https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-format.html](https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-format.html)
- SQL Server Page Architecture: [https://learn.microsoft.com/en-us/sql/relational-databases/pages-and-extents-architecture-guide](https://learn.microsoft.com/en-us/sql/relational-databases/pages-and-extents-architecture-guide)
