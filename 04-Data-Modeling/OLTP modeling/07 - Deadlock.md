---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Deadlock

Deadlock xảy ra khi hai transaction giữ những lock mà transaction còn lại đang cần.

```text
A khóa Account 1
B khóa Account 2
A muốn khóa Account 2 → chờ B
B muốn khóa Account 1 → chờ A
```

Ví dụ chuyển tiền, một transaction chuyển từ Account 1 sang 2, transaction khác chuyển từ Account 2 sang 1. Nếu mỗi transaction khóa account nguồn trước, chúng có thể khóa chéo nhau.

### Cách giảm deadlock

- Luôn khóa nhiều row theo cùng một thứ tự, ví dụ ID nhỏ trước.
- Giữ transaction ngắn.
- Chỉ lock những row cần thiết.
- Không gọi external service trong lúc đang giữ lock.
- Cấu hình lock timeout hoặc deadlock detection.
- Retry transaction nếu database rollback nó vì deadlock.

Deadlock có thể xảy ra trong hệ thống có concurrency cao. Application nên có cơ chế retry an toàn thay vì giả định deadlock không bao giờ xuất hiện.
