---
type: learning-roadmap
status: learning
topic: Data Platform capstone project
---

# Tháng 6 — Ghép thành một Data Platform project hoàn chỉnh

## Mục tiêu

Tháng quan trọng nhất. Không học thêm framework mới. Ghép mọi thứ đã học (Tháng 1–5) thành một hệ thống end-to-end thật, sau đó cố tình làm nó hỏng, debug và cải thiện.

## Kiến trúc gợi ý

```text
Source (giả lập OLTP hoặc API) 
    → Kafka (ingest, delivery semantics đã học Tháng 3)
    → Spark Structured Streaming/batch (xử lý, đã học Tháng 2)
    → Iceberg table trên object storage (lưu trữ, đã học Tháng 4)
    → lớp serving/BI đơn giản (query trực tiếp trên Iceberg hoặc qua warehouse)
Toàn bộ chạy trên container, deploy bằng Terraform + Kubernetes (Tháng 5)
```

Có thể dùng lại project xuyên suốt ở [[Lộ trình học Data Modeling]] (e-commerce: order, payment, inventory) làm domain, thay vì nghĩ domain mới.

## Tuần 1 — Thiết kế

- Vẽ architecture end-to-end, xác định rõ từng thành phần dùng khái niệm gì đã học tháng nào.
- Thiết kế schema OLTP giả lập, thiết kế Kafka topic (partition, key), thiết kế Iceberg table (partition spec).
- Viết rõ: delivery semantics mong muốn, idempotency đặt ở bước nào, SCD dùng ở dimension nào.

## Tuần 2 — Build pipeline thật

- Viết producer đẩy event giả lập vào Kafka.
- Viết Spark job đọc từ Kafka, xử lý, ghi vào Iceberg (áp dụng checkpoint, exactly-once nếu khả thi).
- Containerize từng thành phần bằng Docker.

## Tuần 3 — Deploy và giám sát

- Deploy bằng Terraform (hạ tầng) + Kubernetes (workload: Job/CronJob cho batch, Deployment cho phần chạy liên tục).
- Thêm data quality check tối thiểu (ví dụ: row count reconciliation giữa Kafka và Iceberg, kiểm tra duplicate).
- Thêm log/metric cơ bản để biết pipeline đang chạy hay đã chết.

## Tuần 4 — Cố tình làm hỏng, debug, viết lại

- Cố tình gây lỗi: kill một broker, gửi message trùng, đổi schema nguồn đột ngột, làm consumer chậm lại (test backpressure), xoá checkpoint.
- Với mỗi lỗi: quan sát hệ thống phản ứng thế nào, dùng đúng công cụ debug đã học (Spark UI, Kafka consumer lag, Iceberg snapshot history) để tìm nguyên nhân.
- Viết lại phần nào cần cải thiện, ghi rõ vì sao — đây là bằng chứng thực sự đã hiểu, không phải chỉ chạy được lần đầu.

## Kết quả cần đạt cuối tháng (cũng là kết quả cần đạt cuối lộ trình 6 tháng)

- Có một hệ thống chạy được end-to-end, tự deploy bằng IaC, không làm tay qua console.
- Có ít nhất 3 lỗi đã cố tình gây ra, tìm được nguyên nhân bằng công cụ đúng, và đã sửa.
- Giải thích được toàn bộ kiến trúc bằng các khái niệm đã học — không có phần nào "chạy được nhưng không biết vì sao".

## Liên kết liên quan

- [[00 - Tổng quan lộ trình 6 tháng]]
- [[Lộ trình học Data Modeling]]
- [[05 - Tháng 5 - Cloud, Docker, Terraform, Kubernetes]]
