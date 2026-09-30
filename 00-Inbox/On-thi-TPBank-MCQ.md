# Ôn thi TPBank Fresh DE — MCQ Practice

> Questions and explanations in **English**. Each topic has **Part A – Knowledge Check** (recall) and **Part B – Applied / Advanced** (scenario, output prediction, design choices).
> Answers are hidden in collapsible blocks — try first, then expand. Works in Obsidian Reading view.

## Progress tracker

| # | Topic | Status | Date reviewed | Score |
|---|-------|--------|---------------|-------|
| 1 | SQL | [x] | 2026-09-30 | 14/15 |
| 1b | SQL — Advanced | [x] | 2026-09-30 | 7/8 |
| 1c | Advanced DE — Applied (extraction/CDC/idempotency) | [x] | 2026-09-30 | 3/5 |
| 2 | Database Fundamentals | [ ] | | /15 |
| 3 | DE Concepts | [ ] | | /15 |
| 4 | Big Data Tools | [ ] | | /15 |

*How to use:* review one topic per sitting, tick `[x]` + fill date/score, then continue with the next topic next time.

---

## Tổng kết tiến độ (cập nhật 2026-09-30)

### ✅ Đã ôn & nắm được

- **SQL cơ bản (14/15):** thứ tự thực thi mệnh đề, WHERE vs HAVING, COUNT(*) vs COUNT(col), UNION vs UNION ALL, DDL vs DML (TRUNCATE vs DELETE), xử lý NULL (`IS NULL`, `NOT IN` + NULL), RANK/DENSE_RANK/ROW_NUMBER, CROSS JOIN, AVG bỏ NULL, window function tìm top-N theo nhóm, đánh index cho cột lọc.
  - ⚠️ *Điểm yếu:* **số dòng của LEFT JOIN** (giữ toàn bộ bảng trái + NULL cho phần không khớp).
- **SQL nâng cao (7/8):** leftmost-prefix rule của composite index, covering index, index selectivity (không index cột cardinality thấp), VIEW vs MATERIALIZED VIEW, recursive CTE, window frame (`ROWS BETWEEN ...` = sliding window vs running total), correlated subquery.
  - ⚠️ *Điểm yếu:* **leading wildcard** `LIKE '%...'` phá index; `LIKE '...%'` thì vẫn dùng được index.
- **Advanced DE — Applied (3/5):** trích xuất nhất quán từ OLTP (đọc read replica + snapshot isolation), incremental extraction bằng high-watermark, log-based CDC (Debezium — bắt cả INSERT/UPDATE/DELETE), idempotent load bằng MERGE/upsert, partition overwrite.
  - ⚠️ *Điểm yếu:* (1) **isolation cho consistent read** — Read Committed vẫn bị read skew, cần Repeatable Read/Snapshot; (2) **chọn đúng đòn idempotency theo hình dạng target** — bảng partition thì overwrite partition, đừng MERGE toàn bảng.

### 🔜 Còn phải ôn (ưu tiên từ trên xuống)

- [ ] **Database Fundamentals (topic 2):** keys, chuẩn hóa 1NF–BCNF (nhận diện vi phạm), index clustered/non-clustered, ACID, **isolation levels ↔ anomaly** (dirty/non-repeatable/phantom), OLTP vs OLAP, deadlock, idempotency.
- [ ] **DE Concepts (topic 3):** ETL vs ELT, DWH vs Data Lake vs Lakehouse, star vs snowflake, fact vs dimension, **SCD Type 1/2/3**, batch vs streaming, data quality dimensions.
- [ ] **Big Data Tools (topic 4):** Spark (in-memory, lazy eval, RDD lineage, data skew), Kafka (partition/ordering, pub-sub), Airflow (DAG orchestration), Hadoop (HDFS/MapReduce/YARN), Hive.
- [ ] **Advanced DE — Applied (tiếp):** replication lag khi đọc replica, schema evolution, partitioning vs bucketing, late-arriving data, file formats (Parquet/Avro), backfill.
- [ ] **Bổ sung nếu còn giờ:** Python cơ bản, object vs block storage, đọc query execution plan.

### 🎯 Cần xem lại ngay (danh sách sai)

1. LEFT JOIN → giữ **toàn bộ** dòng bảng trái (row count = bảng trái).
2. `LIKE '%...'` (leading wildcard) → **không dùng được index**; `LIKE '...%'` thì được.
3. Consistent multi-row read → cần **Repeatable Read/Snapshot**, không phải Read Committed.
4. Bảng partition → **overwrite partition**, không MERGE toàn bảng.

---

## 1. SQL

### Part A — Knowledge Check

**Q1.** In which order are these SQL clauses logically executed?
A. SELECT → FROM → WHERE → GROUP BY
B. FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
C. FROM → SELECT → WHERE → ORDER BY
D. WHERE → FROM → GROUP BY → SELECT

<details><summary>Answer</summary>

**B.** Logical order is FROM → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT. This is why a SELECT alias cannot be used in WHERE, but can be used in ORDER BY.
</details>

**Q2.** Which clause filters rows *after* aggregation?
A. WHERE  B. HAVING  C. GROUP BY  D. ON

<details><summary>Answer</summary>

**B. HAVING** — WHERE filters individual rows before grouping; HAVING filters aggregated groups.
</details>

**Q3.** What is the difference between `COUNT(*)` and `COUNT(column)`?
A. No difference
B. `COUNT(*)` counts all rows; `COUNT(column)` skips NULLs in that column
C. `COUNT(column)` counts all rows; `COUNT(*)` skips NULLs
D. Both skip NULLs

<details><summary>Answer</summary>

**B.** `COUNT(*)` counts every row including NULLs; `COUNT(column)` ignores rows where that column is NULL. `SUM`/`AVG` also ignore NULLs (AVG divides by the non-NULL count).
</details>

**Q4.** Which is TRUE about `UNION` vs `UNION ALL`?
A. UNION keeps duplicates, UNION ALL removes them
B. UNION removes duplicates (and sorts), UNION ALL keeps everything
C. They are identical
D. UNION ALL removes duplicates only across the first column

<details><summary>Answer</summary>

**B.** UNION deduplicates (extra work → slower); UNION ALL returns all rows including duplicates (faster).
</details>

**Q5.** Which command is DDL (not DML)?
A. UPDATE  B. INSERT  C. TRUNCATE  D. SELECT

<details><summary>Answer</summary>

**C. TRUNCATE** is DDL — it cannot be rolled back (in most engines), resets identity, and is faster. DELETE is DML and can be rolled back.
</details>

**Q6.** How do you correctly test whether a column is NULL?
A. `col = NULL`  B. `col == NULL`  C. `col IS NULL`  D. `col EQUALS NULL`

<details><summary>Answer</summary>

**C. `col IS NULL`.** Any comparison with NULL using `=` returns UNKNOWN, never TRUE.
</details>

**Q7.** `RANK()` vs `DENSE_RANK()` when there is a tie?
A. Both skip numbers after a tie
B. RANK skips numbers (1,1,3), DENSE_RANK does not (1,1,2)
C. DENSE_RANK skips, RANK does not
D. Both produce 1,2,3 always

<details><summary>Answer</summary>

**B.** With a tie: RANK → 1,1,3 (gap); DENSE_RANK → 1,1,2 (no gap); ROW_NUMBER → always 1,2,3 unique.
</details>

**Q8.** What type of JOIN produces a Cartesian product?
A. INNER JOIN  B. LEFT JOIN  C. CROSS JOIN  D. SELF JOIN

<details><summary>Answer</summary>

**C. CROSS JOIN** — every row of table A paired with every row of table B (m × n rows).
</details>

### Part B — Applied / Advanced

**Q9.** Table `orders` has 10 rows; `customers` has 4 rows. 3 orders have no matching customer. How many rows does `orders LEFT JOIN customers ON orders.cust_id = customers.id` return?
A. 7  B. 10  C. 13  D. 40

<details><summary>Answer</summary>

**B. 10.** LEFT JOIN keeps all left-table (orders) rows; unmatched ones get NULLs for customer columns. Row count = left table count.
</details>

**Q10.** You LEFT JOIN and want to keep unmatched left rows, but also filter the right table by `status = 'active'`. Where should the filter go to NOT break the LEFT JOIN?
A. In the `WHERE` clause
B. In the `ON` clause
C. In a `HAVING` clause
D. Anywhere, same result

<details><summary>Answer</summary>

**B. In the `ON` clause.** Putting `right.status='active'` in WHERE turns the LEFT JOIN into an effective INNER JOIN (NULL rows fail the filter). Keep it in ON to preserve unmatched left rows.
</details>

**Q11.** `SELECT dept, COUNT(*) FROM emp GROUP BY dept HAVING COUNT(*) > 5;` — what does it return?
A. All departments
B. Employees in departments with more than 5 people
C. Departments that have more than 5 employees, with their counts
D. Error: cannot use COUNT in HAVING

<details><summary>Answer</summary>

**C.** It returns one row per department having more than 5 employees, showing the department and its count.
</details>

**Q12.** A column `bonus` has values [100, NULL, 200, NULL]. What does `AVG(bonus)` return?
A. 75  B. 150  C. 100  D. NULL

<details><summary>Answer</summary>

**B. 150.** AVG ignores NULLs: (100 + 200) / 2 = 150, not divided by 4.
</details>

**Q13.** `SELECT * FROM t WHERE col NOT IN (1, 2, NULL);` returns no rows even though data exists. Why?
A. Syntax error
B. NOT IN with a NULL in the list evaluates to UNKNOWN for every row
C. NULL equals everything
D. NOT IN is not valid SQL

<details><summary>Answer</summary>

**B.** `NOT IN (..., NULL)` becomes `col <> NULL` → UNKNOWN, so no row qualifies. Use `NOT EXISTS` or filter out NULLs to avoid this trap.
</details>

**Q14.** You need the 2nd highest salary per department. Best tool?
A. `MAX(salary)`
B. `ORDER BY salary DESC LIMIT 1 OFFSET 1` without partitioning
C. `DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC)` then filter rank = 2
D. `GROUP BY dept HAVING salary = 2`

<details><summary>Answer</summary>

**C.** A window function partitioned by department gives a per-group rank; filter where rank = 2. DENSE_RANK handles salary ties sensibly.
</details>

**Q15.** Query on a 50M-row table filtering `WHERE email = 'x@y.com'` is slow. Best first fix?
A. Add an index on `email`
B. Add more RAM
C. Rewrite with SELECT *
D. Use UNION ALL

<details><summary>Answer</summary>

**A. Add an index on `email`.** An index turns a full table scan into a fast lookup for equality/range filters (trade-off: slower writes, more storage).
</details>

---

## 1b. SQL — Advanced (complex queries, indexing internals, views)

**Q1.** A composite index exists on `(country, city)`. Which query gets an efficient index seek on the leading column?
A. `WHERE city = 'Hanoi'`
B. `WHERE country = 'VN' AND city = 'Hanoi'`
C. Only if you reorder to put `city` first
D. Only queries using both columns with `OR`

<details><summary>Answer</summary>

**B.** Leftmost-prefix rule: the index is sorted by `country` first, then `city`. It can seek on `country` alone or `country`+`city`, but NOT on `city` alone. Predicate order in WHERE is irrelevant — the optimizer reorders `AND` conditions.
</details>

**Q2.** An index exists on `email`. Which query CANNOT use it for a seek?
A. `WHERE email = 'a@b.com'`
B. `WHERE email LIKE 'john%'`
C. `WHERE email LIKE '%gmail.com'`
D. `WHERE email > 'm'`

<details><summary>Answer</summary>

**C.** Leading wildcard `'%...'` defeats a B-tree (sorted left-to-right) → full scan. Trailing wildcard `'john%'` becomes a range seek and DOES use the index. A function on the column (`UPPER(email)=...`) also breaks it unless a function-based index exists.
</details>

**Q3.** What is a covering index for a query?
A. An index covering all rows in the table
B. An index that includes all columns the query needs, so it's answered from the index alone (no table lookup)
C. An index auto-created on the primary key
D. An index spanning multiple tables

<details><summary>Answer</summary>

**B.** The engine reads only the index — no key/bookmark lookup back to the base table. Achieved via composite key columns or `INCLUDE` columns.
</details>

**Q4.** Why might the optimizer ignore an index on `gender` ('M'/'F', ~50/50) for `WHERE gender='F'`?
A. Indexes never work on text
B. Low selectivity — matches ~50% of rows, so a full scan is cheaper than seek + many lookups
C. It must be a primary key
D. Because it's nullable

<details><summary>Answer</summary>

**B.** Selectivity rule: index high-cardinality columns (many distinct values). Low-cardinality columns (gender, boolean, status) return too many rows for a seek to beat a scan.
</details>

**Q5.** Key difference between a VIEW and a MATERIALIZED VIEW?
A. A view stores data physically; a matview is just a saved query
B. A view re-runs its query on each access (always fresh); a matview stores the precomputed result physically (fast reads, can be stale until REFRESH)
C. They are identical
D. A matview cannot be indexed

<details><summary>Answer</summary>

**B.** View = virtual (stored query, always fresh, no speedup). Materialized view = stored result (fast, indexable, but stale until refreshed). Use matviews to cache expensive aggregations.
</details>

**Q6.** What does a recursive CTE enable?
A. Faster filtering
B. Traversing hierarchical/graph data (org charts, category trees, BOM) by self-reference
C. Creating an index
D. Replacing GROUP BY

<details><summary>Answer</summary>

**B.** Anchor member seeds the start; recursive member joins back to the CTE to descend level by level until no new rows appear.
</details>

**Q7.** `SUM(amount) OVER (ORDER BY date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)` computes?
A. Grand total of all rows
B. Running total over the whole partition
C. Moving sum of the current row + 2 previous rows (3-row sliding window)
D. Average of the last 2 rows

<details><summary>Answer</summary>

**C.** The frame clause defines a 3-row sliding window (moving sum). Without a frame, an `ORDER BY` window defaults to a running/cumulative total. `ROWS` = physical rows; `RANGE` = logical value range.
</details>

**Q8.** What characterizes a correlated subquery and its performance concern?
A. Runs once and caches the result
B. References outer-query columns, so it re-executes per outer row — potentially slow, often rewritable as a JOIN/window function
C. Only usable in FROM
D. Always uses an index

<details><summary>Answer</summary>

**B.** It can't run independently (depends on the outer row) → row-by-row execution. On large tables, rewrite as a join or precompute with a window function/CTE.
</details>

---

## 1c. Advanced DE — Applied (extraction, CDC, idempotency)

> Scenario-based. Options are deliberately close in wording — you must understand the concept, not eliminate by length.

**Q1.** Full nightly extract of a 200M-row `transactions` table from a 24/7 production OLTP DB, needing a consistent snapshot with minimal impact. Best approach?
A. Run `SELECT *` against the primary during business hours
B. Read from a read replica / standby with snapshot isolation, during a low-traffic window
C. Lock the whole table for the duration of the extract
D. Copy the underlying data files while the DB is running

<details><summary>Answer</summary>

**B.** Never run big scans on the OLTP primary — offload to a replica. Snapshot isolation gives a consistent point-in-time view while writes continue. Trade-off: replica has replication lag (fine for nightly analytics).
</details>

**Q2.** The full extract is too heavy; switch to incremental on an **append-only** table with a `created_at` column. Correct strategy?
A. Compare full table checksums each night
B. Use a high-watermark: store last max `created_at`, pull `WHERE created_at > watermark`
C. Truncate and reload everything anyway
D. Randomly sample 10% of rows

<details><summary>Answer</summary>

**B.** High-watermark moves only the delta. Watch edges: late-arriving/clock-skew rows (use a small overlap window + idempotent load); works only because the table is append-only (updates/deletes need `updated_at` or CDC).
</details>

**Q3.** The `accounts` table is frequently UPDATED and sometimes DELETED. Watermark misses hard DELETEs and loads the source. Best way to capture all changes with minimal impact?
A. Full reload every night
B. Log-based CDC (read WAL/binlog via Debezium): streams inserts/updates/deletes
C. Add more indexes on `updated_at`
D. Poll the table every 5 seconds with `SELECT *`

<details><summary>Answer</summary>

**B.** CDC tails the transaction log the DB already writes — no query load, no locks, and it captures DELETEs (which a watermark cannot). Decision tree: append-only small → full reload; append-only large → watermark; mutable + low latency → CDC.
</details>

**Q4.** CDC job crashes halfway, Airflow retries from the start, leaving duplicate rows. Which makes the load correctly idempotent (inserts + updates + deletes)?
A. INSERT all events, then nightly dedup keeping latest per `account_id` via `ROW_NUMBER()`
B. DELETE this batch's `account_id` set, then INSERT the new versions, in one transaction
C. MERGE on `account_id`: update on match, insert on no-match, apply CDC deletes — keyed on PK
D. Stage to a temp table, then INSERT ... SELECT only `account_id`s not already in target

<details><summary>Answer</summary>

**C.** Only MERGE/upsert on the PK converges to the same final state for inserts, updates AND deletes on replay. A ignores deletes; B is fragile with delta-only batches; D silently drops updates (insert-if-not-exists).
</details>

**Q5.** Table is partitioned by `event_date`. A day's job failed midway; rerun just that day, no duplicates, don't touch other days. Best approach?
A. `DELETE WHERE event_date = '...'` then append the rerun's rows in the same job
B. `INSERT` again for that day and rely on `UNIQUE(txn_id)` to reject duplicates
C. Overwrite only the `event_date = '...'` partition atomically with that day's full output
D. `MERGE` every rerun row against the whole table keyed on `txn_id` across all partitions

<details><summary>Answer</summary>

**C.** Partition overwrite = atomic swap of one partition; rerun-safe, cheap, others untouched. D works but scans all partitions (wrong tool). A is two non-atomic steps (crash = empty day). Lever by target shape: partitioned batch → overwrite partition; mutable CDC target → MERGE on PK.
</details>

---

## 2. Database Fundamentals

### Part A — Knowledge Check

**Q1.** What does the "I" in ACID stand for, and what does it guarantee?
A. Integrity — data is correct
B. Isolation — concurrent transactions don't interfere
C. Indexing — fast lookups
D. Immutability — data can't change

<details><summary>Answer</summary>

**B. Isolation** — concurrent transactions behave as if executed serially; intermediate states aren't visible to others.
</details>

**Q2.** A table violates 1NF when:
A. It has a transitive dependency
B. A column stores multiple values / repeating groups (non-atomic)
C. It has a composite key
D. It has no primary key

<details><summary>Answer</summary>

**B.** 1NF requires atomic values — no multi-valued cells or repeating groups.
</details>

**Q3.** 3NF removes which kind of dependency?
A. Partial dependency on part of a composite key
B. Transitive dependency (non-key depends on another non-key)
C. Multi-valued dependency
D. Foreign key dependency

<details><summary>Answer</summary>

**B. Transitive dependency.** 2NF removes partial dependencies; 3NF removes transitive ones (non-key → non-key).
</details>

**Q4.** Which isolation level prevents dirty reads but still allows non-repeatable reads?
A. Read Uncommitted
B. Read Committed
C. Repeatable Read
D. Serializable

<details><summary>Answer</summary>

**B. Read Committed** — no dirty reads, but the same row re-read in one transaction may change (non-repeatable read still possible).
</details>

**Q5.** A clustered index:
A. Is stored separately and points to rows
B. Determines the physical storage order of the table (one per table)
C. Can exist many times per table
D. Only works on text columns

<details><summary>Answer</summary>

**B.** A clustered index defines the physical row order, so a table has at most one. Non-clustered indexes are separate structures pointing back to rows.
</details>

**Q6.** What is a candidate key?
A. Any column
B. A minimal attribute set that uniquely identifies a row (a PK candidate)
C. A foreign key
D. A column with an index

<details><summary>Answer</summary>

**B.** A candidate key uniquely identifies rows and is minimal. One candidate is chosen as the primary key; the rest are alternate keys.
</details>

**Q7.** OLTP vs OLAP — which pairing is correct?
A. OLTP = analytics, denormalized; OLAP = transactions, normalized
B. OLTP = transactions, normalized, many small writes; OLAP = analytics, denormalized, large reads
C. Both are identical
D. OLTP is column-store, OLAP is row-store

<details><summary>Answer</summary>

**B.** OLTP: frequent small transactions, normalized, often row-store. OLAP: large aggregate reads, denormalized/star, often column-store.
</details>

**Q8.** Which phenomenon does Serializable prevent that Repeatable Read does not?
A. Dirty read  B. Non-repeatable read  C. Phantom read  D. Lost update

<details><summary>Answer</summary>

**C. Phantom read** — new rows matching a query condition appearing between reads. Only Serializable fully prevents phantoms.
</details>

### Part B — Applied / Advanced

**Q9.** A table `Orders(order_id, product_id, product_name, product_category)` has `product_name` and `product_category` determined by `product_id`, not the whole key. Which normal form is violated and what's the fix?
A. 1NF — split multi-valued cells
B. 2NF — partial dependency; move product attributes to a Product table
C. BCNF only
D. Nothing is wrong

<details><summary>Answer</summary>

**B. 2NF.** `product_name`/`product_category` depend on part of a composite key (`product_id`), a partial dependency. Move them into a separate `Product` table keyed by `product_id`.
</details>

**Q10.** Two users both read `balance = 100`, each add 50, and write back. Final balance is 150, not 200. What is this called?
A. Dirty read  B. Lost update  C. Phantom read  D. Deadlock

<details><summary>Answer</summary>

**B. Lost update.** Concurrent read-modify-write without proper locking/isolation loses one update. Fix with higher isolation, row locks, or atomic `UPDATE ... SET balance = balance + 50`.
</details>

**Q11.** A reporting table is read constantly but rarely written. You add 5 indexes. What's the main trade-off, and is it acceptable here?
A. Reads slow down — not acceptable
B. Writes slow down and storage grows — acceptable because it's read-heavy
C. No trade-off
D. The table loses ACID

<details><summary>Answer</summary>

**B.** Indexes speed reads but slow inserts/updates and use storage. For a read-heavy reporting table, that trade-off is usually fine.
</details>

**Q12.** Transaction A locks row 1 then waits for row 2; Transaction B locks row 2 then waits for row 1. What happens?
A. Both commit
B. Deadlock — the DB aborts one transaction
C. Phantom read
D. Dirty read

<details><summary>Answer</summary>

**B. Deadlock.** Circular wait; the engine detects it and rolls back a victim transaction. Prevent by acquiring locks in a consistent order.
</details>

**Q13.** You must guarantee an operation runs exactly once even if the client retries after a timeout. Which design property do you need?
A. Durability  B. Idempotency  C. Normalization  D. Sharding

<details><summary>Answer</summary>

**B. Idempotency.** An idempotent operation produces the same result no matter how many times it's applied (e.g., using a unique request key), making retries safe.
</details>

**Q14.** Which normalization change trades read speed for less redundancy, and is therefore usually AVOIDED in a data warehouse?
A. Denormalization
B. Higher normalization (more tables, more joins)
C. Adding indexes
D. Partitioning

<details><summary>Answer</summary>

**B.** DWH favors denormalization (star schema) for fewer joins and faster analytical reads; heavy normalization adds joins that slow analytics.
</details>

**Q15.** You need to keep the full change history of a customer's address for auditing. Which approach fits an OLTP design?
A. Overwrite the address column
B. Add a history/event table (or effective-dated rows) recording each change
C. Delete old rows
D. Use a larger VARCHAR

<details><summary>Answer</summary>

**B.** Keep an audit/history table or effective-dated (temporal) rows so every change is preserved. Overwriting loses history.
</details>

---

## 3. DE Concepts

### Part A — Knowledge Check

**Q1.** Key difference between ETL and ELT?
A. ELT transforms before loading; ETL transforms after
B. ETL transforms before loading into the warehouse; ELT loads raw first, then transforms inside the warehouse
C. They are the same
D. ELT has no transform step

<details><summary>Answer</summary>

**B.** ETL = Transform then Load (classic DWH). ELT = Load raw, then Transform using the warehouse/lake compute (Snowflake, BigQuery, Spark) — common in modern lake/lakehouse setups.
</details>

**Q2.** A Data Lake differs from a Data Warehouse mainly because it:
A. Only stores structured data
B. Stores raw data of any format (schema-on-read), cheaply
C. Cannot store files
D. Is always faster for BI

<details><summary>Answer</summary>

**B.** Data Lake = any format, schema-on-read, low cost. DWH = structured, schema-on-write, tuned for BI. Ungoverned lakes risk becoming "data swamps".
</details>

**Q3.** In a star schema, the fact table typically contains:
A. Descriptive text attributes
B. Measures (numeric facts) plus foreign keys to dimensions
C. Only primary keys
D. One row total

<details><summary>Answer</summary>

**B.** Fact = numeric measures + FKs to dimensions (many rows). Dimensions hold descriptive context (fewer rows).
</details>

**Q4.** SCD Type 2 handles a dimension change by:
A. Overwriting the old value
B. Adding a new row with versioning (effective dates / current flag) to preserve history
C. Adding a "previous value" column
D. Deleting the dimension

<details><summary>Answer</summary>

**B.** Type 2 inserts a new row and marks the old one expired, preserving full history. Type 1 overwrites (no history); Type 3 keeps one prior value in a column.
</details>

**Q5.** A Lakehouse combines:
A. Two data warehouses
B. Data lake storage + warehouse features like ACID transactions and governance (e.g., Delta Lake, Iceberg)
C. Only streaming
D. Only OLTP databases

<details><summary>Answer</summary>

**B.** Lakehouse = cheap open lake storage plus DWH-grade reliability (ACID, schema enforcement, time travel) via table formats like Delta Lake / Apache Iceberg.
</details>

**Q6.** Which is a data quality dimension?
A. Compression  B. Completeness  C. Sharding  D. Indexing

<details><summary>Answer</summary>

**B. Completeness.** Common DQ dimensions: Accuracy, Completeness, Consistency, Timeliness, Validity, Uniqueness.
</details>

**Q7.** Batch vs streaming processing — which statement is correct?
A. Batch is real-time; streaming is periodic
B. Batch processes data in scheduled chunks (higher latency, high throughput); streaming processes events continuously (low latency)
C. They are identical
D. Streaming cannot handle large volumes

<details><summary>Answer</summary>

**B.** Batch = scheduled bulk jobs, higher latency. Streaming = continuous per-event/near-real-time processing, low latency.
</details>

**Q8.** Snowflake schema differs from star schema because:
A. It has no fact table
B. Its dimension tables are normalized into multiple related tables
C. It denormalizes dimensions fully
D. It only works in the cloud

<details><summary>Answer</summary>

**B.** Snowflake normalizes dimensions into sub-tables (less redundancy, more joins). Star keeps dimensions flat/denormalized (fewer joins, simpler queries).
</details>

### Part B — Applied / Advanced

**Q9.** Your source is semi-structured JSON logs; you want cheap storage and to transform later using Spark inside the platform. Which pattern fits?
A. ETL into a normalized OLTP DB
B. ELT into a data lake / lakehouse, transform with in-platform compute
C. Overwrite a star schema nightly
D. Load directly into an OLTP index

<details><summary>Answer</summary>

**B. ELT into a lake/lakehouse.** Land raw JSON cheaply, then transform with Spark/SQL in-platform — the standard modern ingestion pattern for varied/semi-structured data.
</details>

**Q10.** A `DimCustomer` must track a customer's tier changes over time so past sales still map to the tier they had *at the time of sale*. Which SCD type?
A. Type 1  B. Type 2  C. Type 3  D. No SCD needed

<details><summary>Answer</summary>

**B. Type 2.** Point-in-time accuracy for historical facts requires versioned dimension rows with effective dates, so each fact joins to the correct historical version.
</details>

**Q11.** In a sales star schema, where does `total_amount` belong and where does `customer_city` belong?
A. Both in the fact table
B. `total_amount` in fact; `customer_city` in the customer dimension
C. Both in a dimension
D. `total_amount` in dimension; `customer_city` in fact

<details><summary>Answer</summary>

**B.** Numeric measures like `total_amount` are facts; descriptive attributes like `customer_city` live in the customer dimension.
</details>

**Q12.** A daily pipeline sometimes reruns after a partial failure, creating duplicate rows in the target. What design fixes this?
A. Add more workers
B. Make the load idempotent (e.g., MERGE/upsert on a key, or overwrite the partition)
C. Increase memory
D. Switch to streaming

<details><summary>Answer</summary>

**B.** Idempotent loads — MERGE/upsert on a business key or full partition overwrite — make reruns safe and prevent duplicates.
</details>

**Q13.** You must detect that a nightly feed silently dropped 30% of expected records. Which data quality check catches this?
A. A schema/type check
B. A completeness/row-count (volume) check against expected thresholds
C. A uniqueness check
D. A format/validity check

<details><summary>Answer</summary>

**B.** A volume/completeness check comparing actual vs expected row counts flags missing data even when each present row is individually valid.
</details>

**Q14.** Business needs fraud alerts within seconds of a transaction. Which processing model?
A. Nightly batch
B. Stream processing (e.g., Kafka + Spark/Flink Structured Streaming)
C. Weekly ETL
D. Manual export

<details><summary>Answer</summary>

**B. Stream processing.** Second-level latency requires continuous event processing, not scheduled batch.
</details>

**Q15.** BI users complain star-schema reports are slow because of many joins across normalized dimensions. What's a reasonable fix?
A. Normalize further (snowflake)
B. Denormalize dimensions (flatten toward a star / wide table) to reduce joins
C. Add OLTP indexes
D. Switch to streaming

<details><summary>Answer</summary>

**B. Denormalize.** Flattening dimensions (star / wide tables) reduces join count and speeds analytical queries — the usual DWH trade-off favoring read performance.
</details>

---

## 4. Big Data Tools

### Part A — Knowledge Check

**Q1.** What is Apache Kafka primarily used for?
A. A relational database
B. A distributed publish–subscribe streaming/message platform
C. A BI dashboard
D. A file compression tool

<details><summary>Answer</summary>

**B.** Kafka is a distributed streaming platform (topics, partitions, producers/consumers) for high-throughput event pipelines. It is **not** a database.
</details>

**Q2.** What is Apache Airflow used for?
A. In-memory data processing
B. Orchestration / scheduling of pipelines defined as DAGs
C. Message queuing
D. Distributed file storage

<details><summary>Answer</summary>

**B.** Airflow schedules and orchestrates workflows expressed as DAGs (directed acyclic graphs) of tasks; it coordinates, it doesn't do the heavy data crunching itself.
</details>

**Q3.** Why is Spark generally faster than Hadoop MapReduce?
A. It uses more disk
B. It processes data in-memory and avoids writing intermediate results to disk
C. It uses SQL only
D. It runs on a single machine

<details><summary>Answer</summary>

**B.** Spark keeps intermediate data in memory across stages; MapReduce writes intermediate results to disk between map and reduce, which is slower.
</details>

**Q4.** In the Hadoop ecosystem, HDFS provides:
A. Resource scheduling
B. Distributed, fault-tolerant file storage
C. SQL querying
D. Stream processing

<details><summary>Answer</summary>

**B.** HDFS = distributed storage (with replication). YARN = resource management; MapReduce = processing; Hive = SQL layer.
</details>

**Q5.** Spark transformations (e.g., `map`, `filter`) are:
A. Eager — run immediately
B. Lazy — not executed until an action (e.g., `collect`, `count`, `save`) is called
C. Random
D. Only for streaming

<details><summary>Answer</summary>

**B. Lazy.** Transformations build a lineage/DAG; execution happens only when an action triggers it, enabling optimization.
</details>

**Q6.** In Kafka, a topic is split into ______ to enable parallelism and scaling.
A. tables  B. partitions  C. indexes  D. shards called rows

<details><summary>Answer</summary>

**B. partitions.** Partitions allow parallel consumption and ordering within each partition; consumers in a group split partitions among themselves.
</details>

**Q7.** Which best describes YARN in Hadoop?
A. The storage layer
B. The cluster resource manager / job scheduler
C. A SQL engine
D. A message broker

<details><summary>Answer</summary>

**B.** YARN (Yet Another Resource Negotiator) allocates cluster resources and schedules jobs across nodes.
</details>

**Q8.** A Spark RDD is:
A. Mutable and updated in place
B. An immutable, distributed collection rebuilt from lineage on failure
C. A single-node array
D. A SQL table with indexes

<details><summary>Answer</summary>

**B.** RDDs are immutable and partitioned across the cluster; lost partitions are recomputed from their lineage, giving fault tolerance.
</details>

### Part B — Applied / Advanced

**Q9.** You need to buffer millions of clickstream events/second and let multiple downstream systems consume them independently. Best tool?
A. Airflow  B. Kafka  C. HDFS alone  D. A single Postgres table

<details><summary>Answer</summary>

**B. Kafka.** High-throughput ingestion with a publish–subscribe model lets many consumers read the same stream independently (via consumer groups/offsets).
</details>

**Q10.** You must run a Spark transform job every night only after an upstream extract finishes, with retries and alerting on failure. Which tool coordinates this?
A. Kafka  B. Airflow (DAG with dependencies + retries)  C. HDFS  D. Hive

<details><summary>Answer</summary>

**B. Airflow.** It expresses task dependencies (extract → transform), handles scheduling, retries, and failure alerts. Spark does the compute; Airflow orchestrates.
</details>

**Q11.** A Spark job is slow and one task takes far longer than the rest. Most likely cause?
A. Too much memory
B. Data skew — one partition/key has far more data than others
C. Lazy evaluation
D. Using DataFrames instead of RDDs

<details><summary>Answer</summary>

**B. Data skew.** An uneven key distribution overloads one partition/task. Mitigations: salting keys, repartitioning, or handling skewed keys separately.
</details>

**Q12.** Your team wants SQL analysts to query huge files on HDFS without writing MapReduce/Spark code. Which tool fits?
A. Kafka  B. Hive  C. Airflow  D. YARN

<details><summary>Answer</summary>

**B. Hive.** Hive provides a SQL interface over data in HDFS, translating queries into distributed jobs so analysts use familiar SQL.
</details>

**Q13.** Why might `df.collect()` on a 500GB Spark DataFrame crash the driver?
A. collect() is lazy
B. collect() pulls all data to the single driver node, exceeding its memory
C. collect() deletes data
D. collect() only works on RDDs

<details><summary>Answer</summary>

**B.** `collect()` is an action that brings the whole dataset to the driver. For large data, write to storage or use `take`/aggregate instead of collecting everything.
</details>

**Q14.** You need ordering guarantees for events of the same user in Kafka. How do you achieve it?
A. Use one partition for everything
B. Use the user id as the partition key so all of a user's events go to the same partition
C. Kafka can't order events
D. Increase replication factor

<details><summary>Answer</summary>

**B.** Kafka guarantees order only *within* a partition. Keying by user id routes each user's events to one partition, preserving per-user order while still scaling across users.
</details>

**Q15.** For a fault-tolerant batch job, a Spark stage fails on one node. What lets Spark recover without restarting everything?
A. Indexes
B. RDD/DataFrame lineage — recompute only the lost partitions
C. Kafka offsets
D. YARN queues

<details><summary>Answer</summary>

**B. Lineage.** Spark recomputes only the lost partitions from their lineage rather than rerunning the entire job.
</details>

---

## Cần bổ sung sau (nếu còn thời gian)

- [ ] Python basics (data types, list/dict comprehension, reading code output)
- [ ] Cloud storage concepts (object storage vs block storage)
- [ ] Partitioning & bucketing in data lakes
