---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Transaction Boundary

Transaction boundary là ranh giới xác định những thao tác nào phải commit hoặc rollback cùng nhau.

Ví dụ reserve inventory:

```text
BEGIN
  giảm available_quantity
  tạo inventory_reservation
  tạo order_item
COMMIT
```

Nếu một bước thất bại, rollback toàn bộ. Không được để xảy ra trạng thái:

```text
đã giảm kho nhưng chưa có reservation
```

### Transaction quá nhỏ

Nếu tạo order, trừ kho và tạo reservation nằm trong các transaction riêng, một bước có thể thành công còn bước khác thất bại. Dữ liệu dễ rơi vào trạng thái dở dang.

### Transaction quá lớn

Không nên giữ transaction trong lúc gọi payment provider, gửi email hoặc gọi shipping service. External service có thể chậm, làm transaction giữ lock lâu; hơn nữa database không thể rollback hành động đã xảy ra ở hệ thống bên ngoài.

### Cách xác định boundary

1. Liệt kê các bước của business operation.
2. Xác định invariant phải đúng sau operation.
3. Gom các thao tác cần atomic vào cùng transaction.
4. Tách tác vụ chậm hoặc external service ra ngoài transaction.
5. Dùng state, retry, idempotency hoặc reconciliation cho phần tách ra.
