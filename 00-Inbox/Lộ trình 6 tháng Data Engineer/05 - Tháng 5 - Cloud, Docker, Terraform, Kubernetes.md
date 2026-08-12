---
type: learning-roadmap
status: learning
topic: Cloud, Docker, Terraform, Kubernetes
---

# Tháng 5 — Cloud + Docker + Terraform + Kubernetes

## Mục tiêu

Chuyển từ "người viết pipeline" sang hiểu pipeline thực sự chạy ở đâu, bằng cách gì. Không lao vào operator/service mesh — chỉ đủ để tự deploy và vận hành những gì học ở Tháng 2–4.

## Tuần 1 — Docker

- Image vs container, layer, cách build image hiệu quả (cache layer, multi-stage build).
- Volume: dữ liệu sống ngoài container thế nào, phân biệt với dữ liệu trong container bị mất khi container chết.
- Networking cơ bản: container gọi nhau qua tên service trong cùng network.
- `docker-compose`: chạy thử một cụm nhỏ (ví dụ Kafka + Spark) trên máy local trước khi lên cloud.

## Tuần 2 — Cloud provider (chọn 1, dùng xuyên suốt)

- Chọn cloud đang dùng hoặc sẽ dùng — nếu chưa có ràng buộc, vault này đã có sẵn nhánh `06-Cloud-Data-Engineering/Azure` nên có thể ưu tiên.
- Core service cho data pipeline: object storage, managed compute cho Spark, managed Kafka (hoặc tương đương), IAM cơ bản (role, policy, service principal).
- Không cần học hết catalog dịch vụ — chỉ học đủ để chạy được stack Tháng 2–4 trên cloud.

## Tuần 3 — Terraform

- Resource, provider, state file — vì sao state phải được quản lý cẩn thận (remote state, state lock).
- Module: đóng gói resource lặp lại (ví dụ một module cho storage bucket + IAM role đi kèm).
- Dependency giữa resource: implicit (tham chiếu attribute) vs explicit (`depends_on`).
- Bài tập: viết Terraform tạo hạ tầng tối thiểu cho một job Spark (storage + compute + IAM).

## Tuần 4 — Kubernetes (chỉ phần cần dùng)

- Pod, Deployment, Job/CronJob — phân biệt khi nào dùng Job (batch, chạy một lần) và CronJob (lịch định kỳ, ví dụ Spark batch job).
- ConfigMap, Secret: tách config và credential ra khỏi image.
- Service: expose Pod cho nhau hoặc ra ngoài, phân biệt ClusterIP/NodePort/LoadBalancer ở mức cơ bản.
- Resource request/limit: vì sao thiếu request/limit làm Pod bị OOMKilled hoặc chiếm hết node.
- Autoscaling cơ bản (HPA) và networking cơ bản — dừng ở đây, không học operator/service mesh trong tháng này.

## Kết quả cần đạt cuối tháng

- Chạy được một stack nhỏ (ví dụ Kafka local) bằng `docker-compose`.
- Viết Terraform tạo và huỷ được hạ tầng tối thiểu cho một job, không làm bằng tay qua console.
- Deploy được một Spark batch job như Kubernetes CronJob, đọc config từ ConfigMap và credential từ Secret.
- Giải thích được resource request/limit ảnh hưởng gì đến việc Pod bị kill hoặc scheduler từ chối schedule.

## Liên kết liên quan

- [[00 - Tổng quan lộ trình 6 tháng]]
- [[04 - Tháng 4 - Lakehouse và Iceberg]]
- [[06 - Tháng 6 - Data Platform Project]]
