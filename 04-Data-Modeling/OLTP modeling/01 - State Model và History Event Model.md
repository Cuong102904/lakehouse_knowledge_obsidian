---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# State Model và History Event Model

## Bức tranh tổng quát

Khi thiết kế OLTP, không chỉ cần biết dữ liệu nằm trong bảng nào. Cần biết:

1. Entity đang ở **state** nào.
2. Điều kiện nào phải luôn đúng (**business invariant**).
3. Những thao tác nào phải thành công hoặc thất bại cùng nhau (**transaction boundary**).

Ví dụ dùng xuyên suốt: khách đặt mua sản phẩm cuối cùng trong kho.


### State model

State model lưu trạng thái hiện tại của entity.

Ví dụ `orders.status`:

```text
PENDING → PAID → SHIPPED → COMPLETED
    ↓
CANCELLED
```

State trả lời câu hỏi:

> Đơn hàng hiện tại đang ở trạng thái nào?

Application dùng state hiện tại để quyết định hành động tiếp theo. Ví dụ chỉ order `PAID` mới được ship.

### History model

History model lưu các lần thay đổi state:

```text
PENDING → PAID → SHIPPED
```

Nó trả lời:

> Entity đã thay đổi như thế nào theo thời gian?

History giúp biết ai thay đổi, thay đổi lúc nào và trạng thái cũ là gì. Thực tế thường lưu cả `orders.status` (state hiện tại) và `order_status_history` (lịch sử).

### Event model

Event model lưu sự kiện nghiệp vụ đã xảy ra:

```text
OrderPlaced
PaymentCompleted
InventoryReserved
OrderShipped
```

Event trả lời:

> Chuyện gì đã xảy ra?

Ví dụ:

```text
PaymentCompleted → gửi thông báo → bắt đầu chuẩn bị giao hàng
```

### Phân biệt nhanh

| Model | Câu hỏi | Ví dụ |
|---|---|---|
| State | Hiện tại là gì? | Order đang `PAID` |
| History | Đã thay đổi ra sao? | `PENDING → PAID` |
| Event | Chuyện gì đã xảy ra? | `PaymentCompleted` |

### Vấn đề cần chú ý

- Chỉ lưu state thì không biết đầy đủ lịch sử.
- State và history nên được cập nhật trong cùng transaction, nếu không hai nơi có thể không khớp.
- Event có thể bị gửi trùng hoặc đến sai thứ tự; consumer cần xử lý idempotency và kiểm tra state.
