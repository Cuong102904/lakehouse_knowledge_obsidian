---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Tổng hợp OLTP Modeling

Ví dụ một hệ thống SaaS bán hàng:

```text
orders
------
id
tenant_id
order_number
status
deleted_at
created_at
updated_at
```

Các rule:

```text
1. order_number unique trong từng tenant.
2. Tenant chỉ thấy order của chính mình.
3. Order đã soft delete không xuất hiện trong query thông thường.
4. Thay đổi status phải được audit.
5. Order đã COMPLETED không được sửa tùy ý.
```

Thiết kế có thể gồm:

```text
UNIQUE (tenant_id, order_number)
RLS theo tenant_id
soft delete bằng deleted_at
audit_log lưu thay đổi status
transaction bảo vệ update status và audit log
```

Khi Tenant A hoàn tất order:

```text
BEGIN
  kiểm tra order thuộc Tenant A
  kiểm tra state hiện tại
  đổi status thành COMPLETED
  ghi audit_log
COMMIT
```

Nếu request cố truy cập order của Tenant B, RLS phải ngăn việc đọc hoặc update row đó.
