---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Multi-tenancy

Multi-tenancy là mô hình một hệ thống phục vụ nhiều tenant độc lập. Tenant có thể là công ty, trường học, cửa hàng hoặc tổ chức trong một SaaS.

Mục tiêu chính:

```text
Tenant A không đọc được dữ liệu của Tenant B.
Tenant A không sửa hoặc xóa được dữ liệu của Tenant B.
```

Mô hình phổ biến là dùng chung table và thêm `tenant_id`:

```text
orders
------
id
tenant_id
customer_id
status
total
created_at
```

Query phải giới hạn theo tenant:

```sql
SELECT *
FROM orders
WHERE tenant_id = :current_tenant_id;
```

Unique constraint cũng thường phải chứa `tenant_id`:

```sql
UNIQUE (tenant_id, order_number)
```

Vì order number `1001` có thể tồn tại ở Tenant A và Tenant B.

### Các mô hình lưu trữ

| Mô hình | Ý tưởng | Trade-off |
|---|---|---|
| Shared database, shared tables | Các tenant dùng chung database và table | Rẻ, dễ scale nhưng phải bảo vệ filter rất kỹ |
| Shared database, separate schema | Mỗi tenant có một schema | Cô lập tốt hơn nhưng quản lý phức tạp |
| Separate database | Mỗi tenant có một database | Cô lập mạnh nhưng chi phí vận hành cao |

Shared tables thường phù hợp với nhiều tenant nhỏ. Tenant lớn hoặc yêu cầu cô lập cao có thể dùng schema hoặc database riêng.
