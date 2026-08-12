---
type: learning-roadmap
status: learning
topic: Data Lake, Lakehouse, Iceberg
---

# Tháng 4 — Data Lake/Lakehouse + Iceberg

## Mục tiêu

Hiểu file format cấp thấp (Parquet) trước khi học table format cấp cao (Iceberg) — vì mọi tối ưu của Iceberg (partition pruning, statistics, compaction) đều dựa trên đặc tính của Parquet.

## Tuần 1 — Parquet

- Columnar storage: vì sao đọc theo cột nhanh hơn cho analytical query.
- Row group, page, cách Parquet tổ chức dữ liệu bên trong file.
- Statistics (min/max) ở tầng row group — nền tảng của predicate pruning.
- Compression (Snappy, Zstd...) và trade-off giữa tỉ lệ nén và tốc độ đọc.
- Predicate pruning: filter loại bỏ row group không cần đọc, nối lại với predicate pushdown đã học ở [[02 - Tháng 2 - Spark]].

## Tuần 2 — Iceberg: snapshot, manifest, metadata

- Table format giải quyết vấn đề gì mà chỉ có file Parquet trên storage không giải quyết được (atomic commit, tránh đọc dữ liệu dở dang).
- Snapshot: mỗi lần ghi tạo snapshot mới, table state tại một thời điểm là gì.
- Manifest list và manifest file: cấu trúc metadata trỏ đến data file, vì sao giúp query nhanh hơn liệt kê file trực tiếp trên storage.
- Metadata file (`.metadata.json`): version, schema, partition spec hiện tại.

## Tuần 3 — Partition evolution, schema evolution, compaction

- Partition evolution: đổi partition scheme mà không cần rewrite toàn bộ dữ liệu cũ — khác biệt lớn so với partition kiểu Hive.
- Schema evolution: add/drop/rename column, đổi kiểu dữ liệu tương thích — an toàn hơn so với Parquet thô vì Iceberg track theo column ID, không theo tên hoặc vị trí.
- Compaction: gộp file nhỏ (nối lại với [[Lộ trình học Distributed Processing và Pipeline Reliability]] giai đoạn 4), chạy bằng Spark job riêng hoặc tự động.

## Tuần 4 — Time travel, optimistic concurrency, multi-engine

- Time travel: query theo snapshot ID hoặc timestamp cũ — dùng cho debug, audit, hoặc rollback.
- Optimistic concurrency: hai job cùng ghi vào một table, ai thắng khi conflict, retry thế nào.
- Multi-engine: Iceberg là table format độc lập engine, đọc/viết được từ Spark, Flink, Trino — kiểm tra version và compatibility engine cụ thể tại thời điểm học (xem ghi chú cần kiểm tra ở [[00 - Tổng quan lộ trình 6 tháng]]), không tin theo con số version cụ thể đã cũ.

## Kết quả cần đạt cuối tháng

- Giải thích vì sao Parquet statistics giúp predicate pruning nhanh hơn.
- Giải thích Iceberg giải quyết vấn đề gì mà Parquet + Hive partition không giải quyết được.
- Thực hiện được partition evolution và schema evolution trên một table thử nghiệm, quan sát dữ liệu cũ không bị ảnh hưởng.
- Chạy compaction và time travel trên một table có nhiều snapshot.

## Liên kết liên quan

- [[00 - Tổng quan lộ trình 6 tháng]]
- [[Lộ trình học Distributed Processing và Pipeline Reliability]]
- [[02 - Tháng 2 - Spark]]
- [[05 - Tháng 5 - Cloud, Docker, Terraform, Kubernetes]]
