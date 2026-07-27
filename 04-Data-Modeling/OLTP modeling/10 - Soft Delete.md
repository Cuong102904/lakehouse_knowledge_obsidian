---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Soft Delete

Soft delete là không xóa vật lý record mà đánh dấu record đã bị xóa.

```text
deleted_at = NULL       → đang hoạt động
deleted_at có giá trị   → đã bị xóa
```

Ví dụ:

```sql
UPDATE products
SET deleted_at = CURRENT_TIMESTAMP
WHERE id = 101;
```

Query thông thường phải lọc record chưa bị xóa:

```sql
SELECT *
FROM products
WHERE deleted_at IS NULL;
```

Soft delete hữu ích khi cần khôi phục dữ liệu, giữ lịch sử, tránh xóa nhầm hoặc vẫn cần tham chiếu đến record cũ trong order.

Soft delete không phải là xóa hoàn toàn. Record vẫn chiếm dung lượng, vẫn chứa dữ liệu nhạy cảm và có thể xuất hiện nếu query quên lọc `deleted_at`.

### Soft delete và unique constraint

Nếu user cũ đã bị soft delete nhưng user mới muốn dùng lại email cũ, unique constraint thông thường có thể gây lỗi. Có thể dùng partial unique index nếu database hỗ trợ:

```sql
CREATE UNIQUE INDEX unique_active_email
ON users(email)
WHERE deleted_at IS NULL;
```

Ý nghĩa: chỉ các user chưa bị xóa mới phải có email unique.

### Những câu hỏi cần quyết định

- Record đã soft delete có được khôi phục không?
- User thông thường có được nhìn thấy không?
- Có được tạo dữ liệu mới liên quan đến record đó không?
- Bảng liên quan có bị soft delete theo không?
- Khi nào cần xóa vật lý để giải phóng dữ liệu?
