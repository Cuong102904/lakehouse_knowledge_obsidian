---
type: learning-roadmap
status: learning
topic: Data Engineer 6-month plan
---

# Lộ trình 6 tháng Data Engineer

## Mục tiêu

Đi từ hiểu concept nền tảng đến build và vận hành được một Data Platform end-to-end, đủ để giải thích được *tại sao* một pipeline thiết kế như vậy, không chỉ biết viết code cho nó chạy.

## Nguyên tắc xuyên suốt

- Mỗi tháng học sâu một lớp trong stack, không học rộng nhiều framework cùng lúc.
- Ưu tiên hiểu nguyên lý (partitioning, consistency, delivery semantics...) trước, công cụ cụ thể (Spark, Kafka, Iceberg...) là nơi áp dụng nguyên lý đó.
- Tháng sau luôn build trên khái niệm tháng trước — xem mục Liên kết giữa các tháng.
- Tháng 6 không học thêm framework mới, chỉ ghép và vận hành.
- Checklist học theo ngày nằm ở [[Checklist hàng ngày]].

## Cấu trúc 6 tháng

| Tháng | Chủ đề | Trọng tâm |
|---|---|---|
| 1 | SQL + Data Modeling + DB fundamentals | Join, window, execution plan, indexing, transaction, isolation level, MVCC, dimensional modeling, SCD, CDC, idempotency |
| 2 | Spark sâu | Partition, shuffle, stage/task, driver/executor, memory, join strategy, skew, AQE, caching, checkpoint |
| 3 | Kafka + distributed systems | Partition, broker, replication, consumer group, offset, delivery semantics, backpressure, CAP |
| 4 | Data Lake/Lakehouse + Iceberg | Parquet, snapshot/manifest, partition/schema evolution, compaction, time travel, optimistic concurrency |
| 5 | Cloud + Docker + Terraform + Kubernetes | Container, IaC, orchestration — chuyển từ viết pipeline sang vận hành pipeline |
| 6 | Data Platform project | Ghép toàn bộ thành hệ thống end-to-end, cố tình làm hỏng và debug |

## Liên kết giữa các tháng

```text
Tháng 1 (SQL, modeling, transaction)
        ↓ transaction/isolation → nền cho cách Spark xử lý concurrent write, cách Kafka đảm bảo ordering
Tháng 2 (Spark)
        ↓ Spark là compute engine chạy trên dữ liệu mà Kafka đưa vào và Iceberg lưu trữ
Tháng 3 (Kafka, distributed systems)
        ↓ Kafka là nguồn streaming; delivery semantics ở đây quyết định idempotency cần làm ở đâu
Tháng 4 (Lakehouse, Iceberg)
        ↓ Iceberg là nơi Spark đọc/viết; schema evolution, compaction nối lại với Tháng 2 và Tháng 3
Tháng 5 (Cloud, Docker, Terraform, Kubernetes)
        ↓ hạ tầng để chạy Spark job, Kafka cluster, Iceberg table ở production
Tháng 6 (Data Platform project)
        Ghép tất cả, cố tình làm hỏng, debug, cải thiện
```

Không có tháng nào độc lập — nếu một tháng học yếu, tháng sau sẽ lộ ra ngay vì phải dùng lại khái niệm đó.

## Chi tiết từng tháng

- [[01 - Tháng 1 - SQL, Data Modeling, Database Fundamentals]]
- [[02 - Tháng 2 - Spark]]
- [[03 - Tháng 3 - Kafka và Distributed Systems]]
- [[04 - Tháng 4 - Lakehouse và Iceberg]]
- [[05 - Tháng 5 - Cloud, Docker, Terraform, Kubernetes]]
- [[06 - Tháng 6 - Data Platform Project]]

## Ghi chú cần kiểm tra lại

- Thông tin "Iceberg 1.11.0 phát hành tháng 5/2026" trong bản mô tả gốc là ngày trong tương lai so với kiến thức hiện có, chưa xác nhận được — cần tự kiểm tra version mới nhất tại thời điểm học thay vì tin theo số liệu này.
- Chọn cloud provider cụ thể (AWS/GCP/Azure) trước khi vào Tháng 5, vault này hiện có sẵn nhánh `06-Cloud-Data-Engineering/Azure` nên có thể ưu tiên Azure nếu chưa có lý do chọn khác.

## Liên kết liên quan

- [[Lộ trình học Data Modeling]]
- [[Lộ trình học Distributed Processing và Pipeline Reliability]]
- [[Checklist hàng ngày]]
