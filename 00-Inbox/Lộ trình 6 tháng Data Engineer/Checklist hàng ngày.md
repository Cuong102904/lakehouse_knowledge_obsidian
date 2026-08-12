---
type: checklist
status: learning
topic: Data Engineer 6-month plan
---

# Checklist hàng ngày — Lộ trình 6 tháng Data Engineer

Mỗi tháng chia 4 tuần, mỗi tuần 5 ngày học + 1 ngày ôn tập/thực hành tổng hợp. Tick từng ngày khi hoàn thành, không tick trước khi thực sự làm bài tập của ngày đó — checklist này để theo dõi thật, không phải để lấp đầy.

## Tháng 1 — SQL, Data Modeling, Database Fundamentals

**Tuần 1 — Join và window function**
- [ ] Ngày 1: Các loại join (inner, left/right, full, cross, self, anti, semi)
- [ ] Ngày 2: Window function — `ROW_NUMBER`, `RANK`, `DENSE_RANK`
- [ ] Ngày 3: Window function — `LAG`/`LEAD`, running total, moving average
- [ ] Ngày 4: Viết lại 3 query dùng subquery bằng window function
- [ ] Ngày 5: So sánh execution plan trước/sau khi đổi sang window function
- [ ] Ngày 6 (ôn tập): Tự giải 5 bài SQL join/window không xem lại note

**Tuần 2 — Execution plan, indexing, partitioning**
- [ ] Ngày 1: Đọc execution plan — scan type, cost estimate
- [ ] Ngày 2: Join method trong plan — nested loop, hash join, merge join
- [ ] Ngày 3: Index — B-tree, composite index order, covering index
- [ ] Ngày 4: Table partitioning (range/list/hash) ở tầng database
- [ ] Ngày 5: Lấy 1 query chậm thật, thêm index/partition, đo lại
- [ ] Ngày 6 (ôn tập): Giải thích bằng lời một execution plan cho người khác hiểu

**Tuần 3 — Transaction, isolation level, MVCC**
- [ ] Ngày 1: Đọc lại [[04 - ACID trong OLTP]]
- [ ] Ngày 2: Isolation level — Read Uncommitted, Read Committed
- [ ] Ngày 3: Isolation level — Repeatable Read, Serializable, anomaly mỗi mức chặn
- [ ] Ngày 4: MVCC — vì sao đọc không block viết
- [ ] Ngày 5: Mô phỏng 2 transaction concurrent update ở các isolation level khác nhau
- [ ] Ngày 6 (ôn tập): Đọc lại [[05 - Concurrent Update và Race Condition]] và [[06 - Locking trong OLTP]]

**Tuần 4 — Dimensional modeling, SCD, CDC, idempotency**
- [ ] Ngày 1: Ôn fact/dimension, viết grain cho 3 fact table
- [ ] Ngày 2: SCD Type 1 vs Type 2 — chọn loại cho từng attribute cụ thể
- [ ] Ngày 3: CDC — cách bắt insert/update/delete, ảnh hưởng đến SCD Type 2
- [ ] Ngày 4: Đọc lại [[08 - Idempotency và Retry-safe Operation]] dưới góc nhìn load lại batch CDC
- [ ] Ngày 5: Thiết kế star schema hoàn chỉnh cho 1 business process
- [ ] Ngày 6 (ôn tập): Tự kiểm tra bằng câu hỏi cuối [[01 - Tháng 1 - SQL, Data Modeling, Database Fundamentals]]

## Tháng 2 — Spark

**Tuần 1 — Partition, shuffle, stage/task, driver/executor**
- [ ] Ngày 1: Partition trong Spark, phân biệt với partition Tháng 1
- [ ] Ngày 2: Narrow vs wide transformation, shuffle boundary
- [ ] Ngày 3: Job → stage → task, DAG scheduler
- [ ] Ngày 4: Driver vs executor, vì sao driver có thể là bottleneck
- [ ] Ngày 5: Chạy 1 job đơn giản, đọc Spark UI để thấy stage/task thật
- [ ] Ngày 6 (ôn tập): Vẽ lại DAG của 1 job bằng tay trước khi xem Spark UI

**Tuần 2 — Memory, join strategy, skew**
- [ ] Ngày 1: Executor memory layout — execution vs storage memory
- [ ] Ngày 2: Broadcast join — khi nào tự chọn, khi nào cần hint
- [ ] Ngày 3: Sort-merge join — cách hoạt động, chi phí
- [ ] Ngày 4: Data skew — dấu hiệu trong Spark UI
- [ ] Ngày 5: Kỹ thuật xử lý skew — salting, repartition, skew join hint
- [ ] Ngày 6 (ôn tập): So sánh thời gian chạy join skew trước/sau khi xử lý

**Tuần 3 — Predicate pushdown, AQE, caching, checkpoint**
- [ ] Ngày 1: Predicate pushdown — filter đẩy xuống storage layer
- [ ] Ngày 2: AQE — coalesce partition tự động
- [ ] Ngày 3: AQE — chuyển broadcast join, xử lý skew join tại runtime
- [ ] Ngày 4: Cache/persist — khi nào có lợi, khi nào hại
- [ ] Ngày 5: Checkpoint trong Spark — cắt lineage dài
- [ ] Ngày 6 (ôn tập): Bật/tắt AQE trên cùng 1 job, so sánh plan và thời gian chạy

**Tuần 4 — Debug job chậm, Structured Streaming**
- [ ] Ngày 1: Tự viết 1 job có vấn đề (skew hoặc join sai loại)
- [ ] Ngày 2: Dùng Spark UI tìm nguyên nhân, sửa lỗi thứ 1
- [ ] Ngày 3: Tự viết job có vấn đề thứ 2 (thiếu predicate pushdown/quá nhiều shuffle), sửa
- [ ] Ngày 4: Structured Streaming — micro-batch model, checkpoint trong streaming
- [ ] Ngày 5: Structured Streaming — áp lại khái niệm backpressure, exactly-once
- [ ] Ngày 6 (ôn tập): Viết lại toàn bộ quá trình debug 2 job thành 1 ghi chú case study

## Tháng 3 — Kafka và Distributed Systems

**Tuần 1 — Partition, broker, replication, leader election**
- [ ] Ngày 1: Partition Kafka — đơn vị song song và ordering
- [ ] Ngày 2: Broker, replica, leader/follower
- [ ] Ngày 3: ISR (in-sync replica), vai trò trong durability
- [ ] Ngày 4: Leader election với KRaft controller
- [ ] Ngày 5: Dựng thử 1 cluster Kafka nhỏ bằng `docker-compose` (chuẩn bị trước Tháng 5)
- [ ] Ngày 6 (ôn tập): Vẽ sơ đồ 1 cluster Kafka với partition/replica/leader

**Tuần 2 — Consumer group, offset, ordering, delivery semantics**
- [ ] Ngày 1: Consumer group — phân chia partition, rebalance
- [ ] Ngày 2: Offset — commit tự động vs thủ công
- [ ] Ngày 3: Ordering — chỉ giữ trong partition, thiết kế key
- [ ] Ngày 4: Delivery semantics — at-most-once, at-least-once
- [ ] Ngày 5: Delivery semantics — exactly-once (idempotent producer, transactional API)
- [ ] Ngày 6 (ôn tập): Viết lại vì sao idempotency ở consumer bù cho at-least-once

**Tuần 3 — Retry, dead-letter, backpressure, schema evolution**
- [ ] Ngày 1: Retry ở producer, retry ở consumer, vấn đề mất ordering
- [ ] Ngày 2: Dead-letter queue — khi nào đẩy sang, ai xử lý lại
- [ ] Ngày 3: Backpressure trong Kafka — lag, không tự động như streaming engine
- [ ] Ngày 4: Schema Registry, compatibility mode
- [ ] Ngày 5: Mô phỏng 1 thay đổi schema làm consumer cũ vỡ, rồi sửa bằng compatibility mode đúng
- [ ] Ngày 6 (ôn tập): Tổng hợp lại toàn bộ failure mode đã học trong tuần

**Tuần 4 — CAP, consistency, failure recovery, tổng hợp**
- [ ] Ngày 1: Đọc lại [[CAP-Theorem]]
- [ ] Ngày 2: Áp CAP vào Kafka — trade-off khi 1 broker chết
- [ ] Ngày 3: `acks` và `min.insync.replicas` — ảnh hưởng đến mất dữ liệu
- [ ] Ngày 4: Thiết kế topic (partition, replication factor, key) cho 1 use case cụ thể
- [ ] Ngày 5: Test thử — kill 1 broker trong cluster local, quan sát hệ thống phản ứng
- [ ] Ngày 6 (ôn tập): Tự kiểm tra bằng câu hỏi cuối [[03 - Tháng 3 - Kafka và Distributed Systems]]

## Tháng 4 — Data Lake/Lakehouse + Iceberg

**Tuần 1 — Parquet**
- [ ] Ngày 1: Columnar storage — vì sao nhanh hơn cho analytical query
- [ ] Ngày 2: Row group, page
- [ ] Ngày 3: Statistics (min/max) ở row group
- [ ] Ngày 4: Compression — trade-off tỉ lệ nén và tốc độ đọc
- [ ] Ngày 5: Predicate pruning — nối lại với predicate pushdown Tháng 2
- [ ] Ngày 6 (ôn tập): Đọc thử metadata của 1 file Parquet thật bằng tool (ví dụ `parquet-tools`)

**Tuần 2 — Iceberg: snapshot, manifest, metadata**
- [ ] Ngày 1: Vấn đề table format giải quyết mà Parquet thô không giải quyết được
- [ ] Ngày 2: Snapshot — table state tại 1 thời điểm
- [ ] Ngày 3: Manifest list và manifest file
- [ ] Ngày 4: Metadata file — version, schema, partition spec hiện tại
- [ ] Ngày 5: Tạo 1 Iceberg table thử, ghi vài lần, xem lịch sử snapshot
- [ ] Ngày 6 (ôn tập): Vẽ sơ đồ metadata → manifest → data file

**Tuần 3 — Partition evolution, schema evolution, compaction**
- [ ] Ngày 1: Partition evolution — đổi scheme không cần rewrite dữ liệu cũ
- [ ] Ngày 2: Schema evolution — track theo column ID
- [ ] Ngày 3: Thực hiện thử add/drop/rename column trên table thử nghiệm
- [ ] Ngày 4: Compaction — gộp file nhỏ, chạy bằng Spark job
- [ ] Ngày 5: Tạo chủ động nhiều file nhỏ rồi chạy compaction, so sánh trước/sau
- [ ] Ngày 6 (ôn tập): Tổng hợp lại sự khác biệt Iceberg vs Hive partition kiểu cũ

**Tuần 4 — Time travel, optimistic concurrency, multi-engine**
- [ ] Ngày 1: Time travel — query theo snapshot ID hoặc timestamp
- [ ] Ngày 2: Optimistic concurrency — conflict khi 2 job cùng ghi
- [ ] Ngày 3: Mô phỏng 2 job ghi conflict, quan sát ai thắng, retry thế nào
- [ ] Ngày 4: Kiểm tra version Iceberg và engine hỗ trợ hiện tại (không tin số liệu cũ)
- [ ] Ngày 5: Thử đọc cùng 1 Iceberg table từ 2 engine khác nhau nếu có điều kiện
- [ ] Ngày 6 (ôn tập): Tự kiểm tra bằng câu hỏi cuối [[04 - Tháng 4 - Lakehouse và Iceberg]]

## Tháng 5 — Cloud, Docker, Terraform, Kubernetes

**Tuần 1 — Docker**
- [ ] Ngày 1: Image vs container, layer
- [ ] Ngày 2: Build image hiệu quả — cache layer, multi-stage build
- [ ] Ngày 3: Volume — dữ liệu sống ngoài container
- [ ] Ngày 4: Networking cơ bản giữa container
- [ ] Ngày 5: Viết `docker-compose` chạy Kafka + 1 service khác
- [ ] Ngày 6 (ôn tập): Build lại toàn bộ stack local từ đầu, không xem lại note

**Tuần 2 — Cloud provider**
- [ ] Ngày 1: Chọn cloud, xác nhận provider dùng xuyên suốt
- [ ] Ngày 2: Object storage — bucket, permission cơ bản
- [ ] Ngày 3: Managed compute cho Spark (ví dụ managed Spark service của provider đã chọn)
- [ ] Ngày 4: Managed messaging (Kafka tương đương) nếu dùng, hoặc tự host
- [ ] Ngày 5: IAM cơ bản — role, policy, service principal
- [ ] Ngày 6 (ôn tập): Vẽ sơ đồ các service cloud sẽ dùng cho Tháng 6

**Tuần 3 — Terraform**
- [ ] Ngày 1: Resource, provider
- [ ] Ngày 2: State file — vì sao cần remote state, state lock
- [ ] Ngày 3: Module — đóng gói resource lặp lại
- [ ] Ngày 4: Dependency — implicit vs `depends_on`
- [ ] Ngày 5: Viết Terraform tạo hạ tầng tối thiểu cho 1 job Spark
- [ ] Ngày 6 (ôn tập): `terraform destroy` rồi `apply` lại từ đầu, xác nhận reproducible

**Tuần 4 — Kubernetes**
- [ ] Ngày 1: Pod, Deployment
- [ ] Ngày 2: Job vs CronJob — khi nào dùng loại nào
- [ ] Ngày 3: ConfigMap, Secret
- [ ] Ngày 4: Service, resource request/limit
- [ ] Ngày 5: Autoscaling cơ bản (HPA), networking cơ bản
- [ ] Ngày 6 (ôn tập): Deploy 1 Spark batch job như CronJob, đọc config/secret đúng cách

## Tháng 6 — Data Platform Project

**Tuần 1 — Thiết kế**
- [ ] Ngày 1: Vẽ architecture end-to-end đầy đủ
- [ ] Ngày 2: Thiết kế schema OLTP giả lập
- [ ] Ngày 3: Thiết kế Kafka topic (partition, key)
- [ ] Ngày 4: Thiết kế Iceberg table (partition spec)
- [ ] Ngày 5: Viết rõ delivery semantics, idempotency, SCD dùng ở đâu
- [ ] Ngày 6 (ôn tập): Review lại thiết kế, tìm điểm chưa rõ trước khi build

**Tuần 2 — Build pipeline**
- [ ] Ngày 1: Viết producer đẩy event giả lập vào Kafka
- [ ] Ngày 2: Viết Spark job đọc từ Kafka
- [ ] Ngày 3: Spark job xử lý và ghi vào Iceberg
- [ ] Ngày 4: Thêm checkpoint, kiểm tra exactly-once nếu khả thi
- [ ] Ngày 5: Containerize từng thành phần bằng Docker
- [ ] Ngày 6 (ôn tập): Chạy end-to-end local bằng `docker-compose` từ đầu đến cuối

**Tuần 3 — Deploy và giám sát**
- [ ] Ngày 1: Viết Terraform cho hạ tầng của project
- [ ] Ngày 2: Deploy workload lên Kubernetes
- [ ] Ngày 3: Thêm data quality check (reconciliation, duplicate check)
- [ ] Ngày 4: Thêm log cơ bản
- [ ] Ngày 5: Thêm metric cơ bản để biết pipeline sống hay chết
- [ ] Ngày 6 (ôn tập): Xác nhận toàn bộ deploy lại được từ đầu bằng IaC, không sửa tay

**Tuần 4 — Cố tình làm hỏng, debug**
- [ ] Ngày 1: Kill 1 broker, quan sát và debug
- [ ] Ngày 2: Gửi message trùng, kiểm tra idempotency có chặn được không
- [ ] Ngày 3: Đổi schema nguồn đột ngột, kiểm tra pipeline phản ứng thế nào
- [ ] Ngày 4: Làm consumer chậm lại để test backpressure, xoá checkpoint để test recovery
- [ ] Ngày 5: Viết lại/cải thiện các phần đã lộ ra vấn đề
- [ ] Ngày 6: Viết tổng kết toàn bộ lộ trình 6 tháng — phần nào hiểu chắc, phần nào cần học thêm

## Liên kết liên quan

- [[00 - Tổng quan lộ trình 6 tháng]]
