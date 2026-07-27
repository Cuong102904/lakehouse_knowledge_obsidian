---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Concurrent Update và Race Condition

### Concurrent update

Concurrent update xảy ra khi nhiều request cùng đọc hoặc sửa một dữ liệu gần như đồng thời.

Ví dụ kho có `available_quantity = 1`:

```text
A đọc quantity = 1
B đọc quantity = 1
A tạo order và ghi quantity = 0
B cũng tạo order và ghi quantity = 0
```

Database cuối cùng có quantity bằng 0 nhưng hai order đã được chấp nhận. Một sản phẩm bị bán hai lần.

### Lost update

Lost update xảy ra khi update của transaction này bị transaction khác ghi đè.

```text
balance ban đầu = 100
A đọc 100, muốn trừ 30
B đọc 100, muốn trừ 50
A ghi 70
B ghi 50
```

Kết quả đúng phải là 20, nhưng kết quả cuối cùng là 50. Thay đổi của A đã bị mất.

### Race condition

Race condition xảy ra khi kết quả phụ thuộc vào thứ tự hoặc thời điểm các request chạy.

Logic không an toàn:

```text
SELECT quantity
Nếu quantity đủ:
    UPDATE quantity
```

Khoảng thời gian giữa `SELECT` và `UPDATE` cho phép request khác thay đổi dữ liệu. Nên gộp điều kiện kiểm tra và thay đổi:

```sql
UPDATE inventory
SET available_quantity = available_quantity - 1
WHERE product_id = 101
  AND available_quantity >= 1;
```

Kiểm tra số dòng bị ảnh hưởng:

- `1 row affected`: giữ hàng thành công.
- `0 rows affected`: không đủ hàng hoặc sản phẩm không tồn tại.
