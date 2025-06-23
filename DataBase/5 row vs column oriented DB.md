# Row vs Column Oriented Databases

Understanding the difference between row-oriented and column-oriented storage models, along with their performance, use cases, and real-world implementations.

---

## Row-Oriented Databases

### Characteristics

- Tables are stored **row-by-row** on disk.  
- A single block I/O read can fetch **all columns** of one or more rows.  
- Once a row is found, all its columns can be quickly accessed from memory.  
- Best suited for **OLTP (Online Transaction Processing)** systems.

### Performance Notes

- Efficient for queries that need full rows, such as:  
  SELECT * FROM employees WHERE id = 123;

- Less efficient for column-specific analytics like:  
  SELECT SUM(salary) FROM employees;

  Because it loads **all columns** of each row into memory.

- `SELECT *` is generally fast and I/O efficient.

### Storage Notes

- If the WHERE clause filters on a sequential or indexed column, the DBMS can quickly locate the required block or page.  
- A **row can span multiple pages**, especially if it contains large fields.

### TOAST in PostgreSQL

- **TOAST** stands for **The Oversized-Attribute Storage Technique**.  
- Used when rows contain very large fields (e.g., TEXT, BYTEA, JSONB).  
- PostgreSQL stores large values **out-of-line** in a separate internal table called a **TOAST table**.  
- This keeps the main table compact and improves I/O efficiency.

---

## Column-Oriented Databases

### Characteristics

- Tables are stored **column-by-column** on disk.  
- A single I/O block read fetches **many values of the same column** across rows.  
- Much faster for operations involving a few columns but many rows.  
- Ideal for **OLAP (Online Analytical Processing)**.

### Performance Notes

- Efficient for analytics, e.g.:  
  SELECT SUM(salary) FROM employees;

  Only the `salary` column is read — minimal I/O.

- Inefficient for:  
  SELECT * FROM employees;

  Each column is in a different block, requiring **multiple I/Os**.

- Excellent performance for range filters or partial column access:  
  SELECT name FROM employees WHERE salary > 100000;

### Storage and Deletion

- Data is stored like:  
  salary column → 1:1000, 2:2000, 3:3000  
  (row ID : value)

- For filtering:  
  1. The DB identifies matching row IDs from a condition.  
  2. It fetches needed column values from relevant blocks.

- For deletion:  
  Values need to be removed across **multiple column pages**, which is more complex than row-store deletion.

---

## When to Use Row vs Column Store

### Row-Oriented:

- Fast inserts, updates, and deletes.  
- Best for transactional systems.  
- Use cases: e-commerce, banking, ERP systems.

### Column-Oriented:

- Fast reads, aggregations, and filtering.  
- Best for analytics and BI workloads.

---

## Compression in Column Stores

- Columns have homogeneous data → better compression using run-length, dictionary encoding, etc.  
- Leads to smaller storage and faster I/O.

---

## Write Tradeoffs in Column Stores

- Frequent inserts/updates are slower because data must be written to multiple column files.  
- Deletes are complex, often requiring tombstones or background merges.

---

## Examples of Column-Oriented Databases

- ClickHouse: real-time analytics OLAP DB.  
- Amazon Redshift: cloud data warehouse.  
- Google BigQuery: serverless analytics platform.  
- Snowflake: cloud-native data warehouse.  
- Apache Druid: time-series analytics DB.  
- Vertica: enterprise analytics DB.

---
