---
type: learning-roadmap
status: learning
topic: Data Modeling
---

# Lộ trình học Data Modeling

## Mục tiêu

Có khả năng phân tích requirements, lựa chọn loại data model phù hợp, thiết kế schema cho OLTP và OLAP, đồng thời hiểu trade-off về correctness, history, performance và maintainability.

## Điểm xuất phát

- Đã biết relational theory.
- Đã học 3NF và indexes.
- Không cần học lại SQL syntax cơ bản.

## Phân biệt các trục khái niệm

OLTP và OLAP là cách phân loại theo workload, không phải toàn bộ các loại data model.

- **Conceptual model**: business entities và relationships.
- **Logical model**: attributes, keys, cardinality, constraints.
- **Physical model**: tables, data types, indexes, partitions và engine-specific details.
- **OLTP**: phục vụ transaction và operational application.
- **OLAP**: phục vụ analytical query và BI.

## Giai đoạn 1 — Relational modeling trong thực tế

**Mức độ: học sâu**

Nội dung:

- Chuyển business requirements thành entity, relationship và business rule.
- Xác định cardinality: 1–1, 1–N, N–N.
- Natural key, surrogate key và immutable identifier.
- Primary key, foreign key, unique constraint, check constraint.
- Normalization và denormalization dưới góc nhìn trade-off.
- Source of truth và ownership của dữ liệu.
- Schema evolution và backward-compatible migration.

Thực hành:

- Thiết kế schema cho e-commerce, library và subscription SaaS.
- Với mỗi schema, viết ERD, DDL, constraints và một migration thay đổi schema.

## Giai đoạn 2 — OLTP modeling

**Mức độ: học sâu**

Nội dung:

- State model và history/event model.
- Business invariant và cách bảo vệ bằng constraint hoặc transaction.
- Transaction boundary.
- Atomicity, consistency, isolation và durability trong workload thực tế.
- Concurrent update, lost update và race condition.
- Pessimistic locking, optimistic locking và deadlock.
- Idempotency và retry-safe operation.
- Audit trail, soft delete và temporal data.
- Multi-tenancy và row-level security.

Các bài toán cần hiểu:

- Place order và reserve inventory.
- Transfer money.
- Cancel order và hoàn stock.
- Xử lý duplicate request khi client retry.

Kết quả cần đạt:

> Có thể giải thích transaction nào cần atomic, invariant nào thuộc database, và cách schema ngăn dữ liệu rơi vào trạng thái không hợp lệ.

## Giai đoạn 3 — Dimensional modeling / Kimball

**Mức độ: học rất sâu**

Nội dung:

- Business process và grain.
- Fact table và dimension table.
- Additive, semi-additive và non-additive measures.
- Star schema và snowflake schema.
- Conformed dimensions.
- Date dimension.
- Degenerate dimension và role-playing dimension.
- Transaction fact, periodic snapshot fact và accumulating snapshot fact.
- Slowly Changing Dimensions: Type 0, 1, 2 và các biến thể cần biết.
- Late-arriving dimensions/facts.
- Incremental load và reconciliation.

Thực hành:

- Xây dựng model bán hàng từ `orders` và `order_items`.
- Viết rõ grain cho từng fact table.
- Thiết kế `dim_customer` với SCD Type 1 và Type 2.
- Tạo các câu query doanh thu theo ngày, sản phẩm, customer và region.

## Giai đoạn 4 — Data Warehouse architecture

**Mức độ: hiểu rõ**

Nội dung:

- Kimball và data marts.
- Inmon và enterprise data warehouse normalized.
- Data Vault: Hub, Link, Satellite.
- One Big Table và wide table.
- Source layer, staging, integration và presentation layer.
- Batch load, CDC và incremental processing.

Mục tiêu của giai đoạn này là biết lựa chọn kiến trúc, chưa cần triển khai sâu mọi framework.

## Giai đoạn 5 — Lakehouse và analytical platform

**Mức độ: học sâu vừa**

Nội dung:

- Medallion architecture: Bronze, Silver, Gold.
- Schema-on-write và schema-on-read.
- Data lake, data warehouse và lakehouse.
- Open table format và transaction trên object storage.
- Partitioning, clustering và file layout.
- Data quality, lineage và schema evolution.

Liên hệ triển khai:

- Databricks/Delta Lake.
- Microsoft Fabric.
- Azure Data Lake và Synapse.

## Giai đoạn 6 — Các model chuyên biệt

**Mức độ: biết khái niệm, học sâu khi có nhu cầu**

- Document model.
- Key-value model.
- Wide-column model.
- Graph model.
- Time-series model.
- Event sourcing.
- Feature store model.
- Anchor modeling.

Không cần học sâu tất cả. Chỉ đào sâu khi workload yêu cầu, ví dụ graph cho network, time-series cho IoT hoặc event sourcing cho audit-driven system.

## Mức độ ưu tiên

| Chủ đề | Mức độ |
|---|---|
| Relational modeling thực tế | Rất sâu |
| OLTP schema và transaction | Sâu |
| Kimball dimensional modeling | Rất sâu |
| ETL/ELT, CDC, SCD và data quality | Sâu |
| Inmon | Hiểu rõ |
| Data Vault | Hiểu rõ, thực hành cơ bản |
| Lakehouse/Medallion | Sâu vừa |
| Document, Graph, Time-series | Biết và học theo nhu cầu |

## Project xuyên suốt

Xây dựng hệ thống analytics cho một e-commerce:

1. Thiết kế OLTP schema cho customer, product, order, payment và inventory.
2. Mô phỏng transaction đặt hàng, thanh toán và hoàn hàng.
3. Thêm audit log, idempotency key và optimistic locking.
4. Dùng CDC hoặc batch extraction để đưa dữ liệu sang analytical layer.
5. Thiết kế Kimball star schema cho sales và inventory.
6. Xử lý customer history bằng SCD Type 2.
7. Viết data quality checks và reconciliation giữa OLTP và OLAP.

## Tiêu chí hoàn thành

- Viết được ERD từ requirements.
- Giải thích được grain của mỗi bảng quan trọng.
- Thiết kế được transaction boundary.
- Phân biệt được state, snapshot và event.
- Xử lý được concurrent update và retry.
- Thiết kế được star schema cho một business process.
- Chọn được giữa normalized, dimensional, Data Vault hoặc wide model dựa trên workload.

## Trình tự học ngắn gọn

```text
Relational modeling thực tế
        ↓
OLTP: transaction, invariant, concurrency
        ↓
Kimball: fact, dimension, grain, SCD
        ↓
ETL/ELT, CDC, data quality
        ↓
Warehouse và Lakehouse architecture
        ↓
Data Vault và các model chuyên biệt
```

## Câu hỏi tự kiểm tra

- Một dòng trong bảng này đại diện cho điều gì?
- Entity nào là source of truth?
- Invariant nào phải được database enforce?
- Operation nào cần nằm trong cùng transaction?
- Nếu request được retry, dữ liệu có bị tạo duplicate không?
- Nếu business attribute thay đổi, có cần giữ lịch sử không?
- Mô hình này phục vụ transaction hay analytical query?
