---
type: learning-roadmap
status: learning
topic: Apache Spark
---

# Tháng 2 — Spark thật sâu

## Mục tiêu

Không học thêm API PySpark tràn lan. Hiểu Spark chạy thế nào ở bên dưới, đủ để tự đọc Spark UI và chẩn đoán một job chậm.

## Tuần 1 — Partition, shuffle, stage/task, driver/executor

- Partition trong Spark: khác gì với partition ở database (Tháng 1) và partition file trên storage (Tháng 4).
- Narrow vs wide transformation, khi nào tạo shuffle boundary → stage mới.
- Job → stage → task, DAG scheduler quyết định thế nào.
- Driver vs executor: driver làm gì, executor làm gì, vì sao driver có thể là bottleneck (collect, broadcast lớn).

## Tuần 2 — Memory, broadcast join, sort-merge join, skew

- Executor memory layout: execution memory vs storage memory, vì sao OOM xảy ra.
- Broadcast join: khi nào Spark tự chọn, khi nào phải ép bằng hint, giới hạn kích thước.
- Sort-merge join: cách hoạt động, chi phí so với broadcast join.
- Data skew: dấu hiệu nhận biết trong Spark UI (task time lệch nhau), kỹ thuật xử lý (salting, repartition, skew join hint).
- Liên kết với [[Lộ trình học Distributed Processing và Pipeline Reliability]] giai đoạn 1.

## Tuần 3 — Predicate pushdown, AQE, caching, checkpoint

- Predicate pushdown: filter được đẩy xuống tận storage layer (Parquet) thế nào — chuẩn bị cho Tháng 4.
- Adaptive Query Execution (AQE): tự động coalesce partition, chuyển broadcast join, xử lý skew join tại runtime.
- Cache/persist: khi nào cache có lợi, khi nào cache làm chậm hơn (memory pressure).
- Checkpoint trong Spark: khác với checkpoint trong Structured Streaming, dùng để cắt lineage dài.

## Tuần 4 — Tự tạo job chậm, debug bằng Spark UI, tổng quan Structured Streaming

- Bài tập chính của tháng: tự viết một job có vấn đề (skew, join sai loại, thiếu predicate pushdown, quá nhiều shuffle), sau đó dùng Spark UI (Stages, SQL tab, task time distribution) để tìm nguyên nhân và sửa.
- Structured Streaming: chạy trên cùng Spark SQL engine, tập trung vào scalable và fault-tolerant processing — đọc lại khái niệm checkpoint, backpressure, exactly-once ở [[Lộ trình học Distributed Processing và Pipeline Reliability]] giai đoạn 2 và áp vào Structured Streaming cụ thể.

## Kết quả cần đạt cuối tháng

- Nhìn Spark UI, chỉ ra được stage nào chậm và vì sao (shuffle, skew, hay spill).
- Giải thích được khi nào Spark chọn broadcast join, khi nào chọn sort-merge join.
- Tự sửa được một job chậm do skew hoặc do thiếu predicate pushdown.
- Giải thích Structured Streaming đảm bảo fault-tolerance bằng checkpoint như thế nào.

## Liên kết liên quan

- [[00 - Tổng quan lộ trình 6 tháng]]
- [[Lộ trình học Distributed Processing và Pipeline Reliability]]
- [[01 - Tháng 1 - SQL, Data Modeling, Database Fundamentals]]
- [[03 - Tháng 3 - Kafka và Distributed Systems]]
