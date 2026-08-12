---
type: learning-roadmap
status: learning
topic: SQL, Data Modeling, Database Fundamentals
---

# Tháng 1 — SQL + Data Modeling + Database Fundamentals

## Mục tiêu

Nhìn một pipeline hoặc một schema và giải thích được *tại sao* nó thiết kế như vậy — không chỉ viết SQL chạy đúng kết quả.

## Tuần 1 — SQL join và window function

- Các loại join: inner, left/right, full, cross, self-join, anti-join, semi-join.
- Window function: `ROW_NUMBER`, `RANK`, `DENSE_RANK`, `LAG`/`LEAD`, running total, moving average.
- Bài tập: viết lại một truy vấn dùng subquery bằng window function và so sánh execution plan.

Vị trí trong vault: `01-Programming/SQL`.

## Tuần 2 — Execution plan, indexing, partitioning

- Đọc execution plan: seq scan, index scan, index-only scan, join method (nested loop, hash join, merge join), cost estimate.
- Index: B-tree, khi nào index không được dùng, covering index, composite index order.
- Table partitioning ở tầng database (range, list, hash) — phân biệt với partitioning ở tầng Spark sẽ học Tháng 2.
- Bài tập: lấy một query chậm, đọc plan, thêm index hoặc partition, đo lại.

## Tuần 3 — Transaction, isolation level, MVCC

- Transaction boundary, atomicity trong thực tế (không chỉ định nghĩa).
- Isolation level: Read Uncommitted, Read Committed, Repeatable Read, Serializable — anomaly nào mỗi mức chặn được.
- MVCC: vì sao đọc không block viết, snapshot đọc được là gì.
- Đã có nền tảng liên quan trong vault, đọc lại trước khi học sâu thêm: [[04 - ACID trong OLTP]], [[05 - Concurrent Update và Race Condition]], [[06 - Locking trong OLTP]].
- Bài tập: mô phỏng hai transaction concurrent update cùng một dòng ở các isolation level khác nhau, quan sát kết quả khác nhau thế nào.

## Tuần 4 — Ôn lại dimensional modeling: fact/dimension, grain, SCD, CDC, idempotency

- Không học lại từ đầu — ôn và làm chắc lại theo [[Lộ trình học Data Modeling]] giai đoạn 3 (Kimball) và giai đoạn 2 (OLTP, idempotency).
- Grain: viết được grain cho 3 fact table khác nhau, giải thích một dòng đại diện cho gì.
- SCD Type 1 vs Type 2: chọn loại nào cho attribute nào và vì sao.
- CDC: cách bắt insert/update/delete từ nguồn, ảnh hưởng đến SCD Type 2 thế nào.
- Idempotency: đọc lại [[08 - Idempotency và Retry-safe Operation]] dưới góc nhìn "load lại cùng batch CDC hai lần không được tạo duplicate".

## Kết quả cần đạt cuối tháng

- Đọc execution plan và chỉ ra chính xác bước nào tốn chi phí.
- Giải thích isolation level nào phù hợp cho một use case cụ thể (ví dụ: đặt hàng, chuyển tiền).
- Thiết kế được star schema với grain rõ ràng và SCD phù hợp cho một business process.
- Giải thích được vì sao một pipeline CDC → dimension cần idempotency ở bước load.

## Liên kết liên quan

- [[00 - Tổng quan lộ trình 6 tháng]]
- [[Lộ trình học Data Modeling]]
- [[02 - Tháng 2 - Spark]]
