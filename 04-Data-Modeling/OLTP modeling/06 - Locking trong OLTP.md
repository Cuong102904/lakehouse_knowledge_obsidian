---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Locking trong OLTP

### Pessimistic locking

Pessimistic locking giả định xung đột sẽ xảy ra, nên khóa row trước khi sửa:

```sql
BEGIN;

SELECT available_quantity
FROM inventory
WHERE product_id = 101
FOR UPDATE;

-- kiểm tra và giảm kho
-- tạo order item

COMMIT;
```

Transaction khác muốn sửa cùng row phải chờ transaction hiện tại kết thúc.

Phù hợp với inventory, số dư tài khoản hoặc quota có tranh chấp cao. Nhược điểm là transaction khác phải chờ, giữ lock lâu sẽ làm giảm throughput và có thể gây deadlock.

### Optimistic locking

Optimistic locking giả định xung đột ít xảy ra. Dữ liệu có thêm cột `version`:

```text
id = 101, balance = 100, version = 3
```

Khi update, chỉ ghi nếu version vẫn là version đã đọc:

```sql
UPDATE accounts
SET balance = 70,
    version = version + 1
WHERE id = 101
  AND version = 3;
```

Nếu kết quả là `0 rows affected`, dữ liệu đã bị transaction khác thay đổi. Application phải đọc lại, retry, tính toán lại hoặc báo conflict.

Phù hợp với chỉnh sửa profile, form hoặc metadata, nơi xung đột hiếm xảy ra.

| | Pessimistic locking | Optimistic locking |
|---|---|---|
| Cách xử lý | Khóa trước khi sửa | Kiểm tra `version` khi ghi |
| Khi conflict | Transaction sau phải chờ | Transaction sau bị từ chối |
| Phù hợp | Inventory, balance, quota | Profile, form, metadata |
| Rủi ro | Chờ lâu, deadlock | Conflict, cần retry |
