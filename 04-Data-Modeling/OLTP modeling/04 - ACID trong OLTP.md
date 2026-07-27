---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# ACID trong OLTP

ACID là bốn thuộc tính giúp transaction xử lý an toàn:

- **Atomicity**: thành công toàn bộ hoặc rollback toàn bộ.
- **Consistency**: sau transaction, dữ liệu vẫn thỏa các constraint và business invariant.
- **Isolation**: các transaction chạy đồng thời nhưng không phá dữ liệu của nhau.
- **Durability**: sau `COMMIT`, dữ liệu không mất khi database bị crash.

Ví dụ mua sản phẩm cuối cùng trong kho. Các bước giảm kho, tạo `order_item`, đổi trạng thái order và ghi lịch sử cần được xử lý đúng transaction boundary. Nếu tạo item thất bại thì việc giảm kho cũng phải rollback.

Database chỉ rollback được thao tác bên trong database. Nó không tự rollback được email đã gửi, payment đã thực hiện hoặc request đã gửi sang service khác. Vì vậy không nên giữ database transaction để chờ external service.

`Consistency` có hai lớp:

- **Database consistency**: `quantity > 0`, `price >= 0`, foreign key phải tồn tại, email không trùng. Bảo vệ bằng constraint.
- **Business consistency**: order đã `SHIPPED` không quay lại `PENDING`, một payment chỉ được ghi nhận thành công một lần. Bảo vệ bằng transaction, constraint và logic nghiệp vụ.
