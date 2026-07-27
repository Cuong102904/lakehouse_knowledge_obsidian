---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Audit Trail

Audit trail là lịch sử ghi lại các thay đổi quan trọng đối với dữ liệu:

- Ai thay đổi?
- Thay đổi lúc nào?
- Entity nào bị thay đổi?
- Giá trị cũ và giá trị mới là gì?
- Vì sao hoặc service nào thực hiện?

Ví dụ:

```text
Order 123: PENDING → PAID
changed_by = user_45
source = payment-service
changed_at = 2026-07-28 10:30
```

Một bảng audit có thể có:

```text
audit_log
---------
id
entity_type
entity_id
action
old_values
new_values
changed_by
source
request_id
created_at
```

Audit trail dùng để điều tra dữ liệu sai, theo dõi thay đổi quyền hoặc giá, phục vụ compliance và biết service nào gây ra lỗi.

Audit trail khác history model ở trọng tâm:

- **History model**: entity đã đi qua các state nào?
- **Audit trail**: ai đã thực hiện thay đổi đó và bằng request nào?

Nếu có thể, update dữ liệu chính và ghi audit nên nằm trong cùng transaction:

```text
BEGIN
  update order
  insert audit_log
COMMIT
```

Không nên lưu password, token, secret hoặc đầy đủ thông tin thẻ vào audit log. Audit log thường cần phân quyền đọc riêng và có thể cần partition hoặc retention policy vì tăng rất nhanh.
