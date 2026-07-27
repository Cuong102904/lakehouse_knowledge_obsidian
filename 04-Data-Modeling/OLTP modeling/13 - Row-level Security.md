---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Row-level Security

Row-level security, viết tắt là **RLS**, là cơ chế database tự giới hạn row mà user hoặc session được phép đọc hoặc sửa.

```text
User thuộc Tenant A
→ chỉ được SELECT, UPDATE hoặc DELETE row có tenant_id = A
```

Nếu application vô tình chạy `SELECT * FROM orders`, RLS vẫn có thể ngăn việc trả về order của các tenant khác.

RLS là thêm một lớp bảo vệ ở database, nhưng không thay thế hoàn toàn application authorization:

- **Application authorization**: user có được thực hiện action này không?
- **RLS**: user được nhìn thấy hoặc sửa những row nào?

RLS cần bảo vệ riêng các thao tác `SELECT`, `INSERT`, `UPDATE` và `DELETE`. Khi insert, phải đảm bảo user Tenant A không thể tự gửi `tenant_id = B`.

Nếu dùng connection pool, cần reset tenant context sau mỗi request. Context của Tenant A không được giữ lại rồi dùng nhầm cho request của Tenant B.
