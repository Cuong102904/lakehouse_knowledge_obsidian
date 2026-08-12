---
type: learning-roadmap
status: learning
topic: Kafka, Distributed Systems
---

# Tháng 3 — Kafka + Distributed Systems

## Mục tiêu

Hiểu Kafka không phải như một hàng đợi message thông thường, mà như một hệ phân tán có replication, leader election và failure recovery — và hiểu các nguyên lý nền bên dưới.

## Tuần 1 — Partition, broker, replication, leader election

- Partition trong Kafka: đơn vị song song và đơn vị ordering — chỉ đảm bảo order trong một partition, không đảm bảo giữa các partition.
- Broker, replica, leader/follower, ISR (in-sync replica).
- Leader election: xảy ra khi nào, ai quyết định (KRaft controller, không phải ZooKeeper).
- Kiến trúc hiện đại dùng KRaft — học theo kiến trúc này, không cần đào sâu ZooKeeper-era trừ khi làm việc với cluster cũ.

## Tuần 2 — Consumer group, offset, ordering, delivery semantics

- Consumer group: cách phân chia partition giữa các consumer, rebalance xảy ra khi nào.
- Offset: commit tự động vs commit thủ công, ảnh hưởng đến delivery semantics.
- Ordering: chỉ giữ trong partition, thiết kế key để dữ liệu liên quan rơi vào cùng partition.
- Delivery semantics: at-most-once, at-least-once, exactly-once — Kafka đạt exactly-once bằng cơ chế nào (idempotent producer, transactional API).
- Liên kết với [[Lộ trình học Distributed Processing và Pipeline Reliability]] giai đoạn 2 — idempotency ở consumer side bù cho at-least-once.

## Tuần 3 — Retry, dead-letter, backpressure, schema evolution

- Retry: retry ở producer, retry ở consumer, vấn đề khi retry làm mất ordering.
- Dead-letter queue: khi nào đẩy message sang DLQ, ai xử lý lại.
- Backpressure: consumer chậm hơn producer, Kafka xử lý qua lag thế nào, không có backpressure "tự động" như streaming engine — phải tự thiết kế.
- Schema evolution: Schema Registry, compatibility mode (backward, forward, full), điều gì làm consumer cũ vỡ.

## Tuần 4 — CAP, consistency, failure recovery, tổng hợp

- CAP theorem: không cần thuộc lòng định nghĩa, cần hiểu Kafka đánh đổi consistency/availability thế nào khi một broker chết.
- Failure recovery: broker chết, partition failover, dữ liệu có mất không (phụ thuộc `acks`, `min.insync.replicas`).
- Đọc lại [[CAP-Theorem]] đã có trong vault và áp vào ví dụ Kafka cụ thể thay vì học lại lý thuyết trừu tượng.
- Bài tập tổng hợp: thiết kế topic (số partition, replication factor, key) cho một use case cụ thể (ví dụ order event), giải thích trade-off.

## Kết quả cần đạt cuối tháng

- Giải thích được vì sao Kafka chỉ đảm bảo order trong một partition.
- Chọn được delivery semantics phù hợp và biết cần idempotency ở đâu để bù.
- Thiết kế topic với số partition/replication hợp lý cho một use case, giải thích được trade-off.
- Giải thích được điều gì xảy ra khi một broker chết, dữ liệu có mất không và vì sao.

## Liên kết liên quan

- [[00 - Tổng quan lộ trình 6 tháng]]
- [[Lộ trình học Distributed Processing và Pipeline Reliability]]
- [[CAP-Theorem]]
- [[02 - Tháng 2 - Spark]]
- [[04 - Tháng 4 - Lakehouse và Iceberg]]
