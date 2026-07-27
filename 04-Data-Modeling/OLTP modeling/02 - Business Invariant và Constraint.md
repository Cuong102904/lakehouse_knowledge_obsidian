---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Business Invariant và Constraint

Business invariant là điều kiện phải luôn đúng khi dữ liệu hợp lệ.

Trong ví dụ đặt hàng:

```text
inventory.available_quantity >= 0
order_item.quantity > 0
product trong order phải tồn tại
một payment transaction không được ghi nhận hai lần
```

### Bảo vệ bằng constraint

Database constraint phù hợp với các rule đơn giản:

- `NOT NULL`: giá trị bắt buộc có.
- `CHECK`: ví dụ `quantity > 0`, `amount >= 0`.
- `FOREIGN KEY`: product hoặc customer phải tồn tại.
- `UNIQUE`: ngăn duplicate payment hoặc duplicate email.

### Bảo vệ bằng transaction

Transaction cần thiết khi invariant liên quan đến nhiều thao tác.

Ví dụ kho chỉ còn `1` sản phẩm. Hai khách cùng đặt hàng. Nếu cả hai cùng đọc thấy `quantity = 1` rồi cùng giảm kho, hệ thống có thể chấp nhận hai order dù chỉ có một sản phẩm.

Phép kiểm tra và phép giảm kho phải được thực hiện an toàn như một thao tác nguyên tử:

```sql
UPDATE inventory
SET available_quantity = available_quantity - 1
WHERE product_id = 101
  AND available_quantity >= 1;
```

Nếu không update được dòng nào thì không đủ hàng.

### Nguyên tắc

Rule nào database có thể enforce chắc chắn thì nên để database enforce. Application logic vẫn cần thiết cho rule phức tạp, nhưng không nên là lớp bảo vệ duy nhất.
