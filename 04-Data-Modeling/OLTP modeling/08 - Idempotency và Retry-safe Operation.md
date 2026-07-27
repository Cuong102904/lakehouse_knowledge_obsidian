---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Idempotency và Retry-safe Operation

### Idempotency

Một operation là **idempotent** nếu thực hiện một lần hay nhiều lần thì trạng thái cuối cùng vẫn giống nhau.

```text
Đặt order.status = PAID

Lần 1: PENDING → PAID
Lần 2: PAID → PAID
```

Idempotency cần thiết vì request có thể bị retry khi network timeout, client không nhận được response, worker bị crash hoặc message broker gửi lại message.

Nếu request thanh toán đã trừ tiền nhưng response bị mất, client có thể gửi lại. Nếu server không có idempotency, tiền có thể bị trừ hai lần.

### Idempotency key

Client gửi một key đại diện cho một ý định nghiệp vụ:

```http
POST /payments
Idempotency-Key: pay-order-123-attempt-1
```

Server lưu key và kết quả xử lý vào database:

```text
payment_requests
-----------------
idempotency_key UNIQUE
order_id
request_hash
status
response_body
```

Luồng xử lý:

```text
Request đầu tiên:
  chưa có key → xử lý → lưu kết quả → trả response

Request retry:
  đã có key → không xử lý lại → trả response cũ
```

`UNIQUE constraint` rất quan trọng. Chỉ kiểm tra key bằng application code là chưa đủ, vì hai request đồng thời có thể cùng thấy key chưa tồn tại.

Không được dùng cùng một key cho hai payload khác nhau. Ví dụ key `abc` đã dùng cho order 123 thì không được dùng lại cho order 456.

### Retry-safe operation

Retry-safe operation là operation có thể chạy lại sau lỗi mà không làm sai dữ liệu hoặc tạo hiệu ứng phụ trùng lặp.

Operation tương đối an toàn:

```sql
UPDATE orders
SET status = 'PAID'
WHERE id = 123
  AND status = 'PENDING';
```

Chạy lại sau khi order đã `PAID` sẽ không tạo thêm payment.

Operation không an toàn nếu chạy lại:

```sql
UPDATE accounts
SET balance = balance - 100
WHERE id = 1;
```

Chạy lại sẽ trừ thêm 100. Cần kết hợp idempotency key, unique constraint hoặc một business identifier duy nhất để bảo vệ operation.

### Idempotency và duplicate prevention

- **Duplicate prevention**: ngăn tạo dữ liệu trùng bằng `UNIQUE`, ví dụ một order chỉ có một payment thành công.
- **Idempotency**: request retry nhiều lần nhưng hiệu ứng nghiệp vụ chỉ xảy ra một lần.

Hai cơ chế này thường được dùng cùng nhau trong payment, tạo order, gửi message và xử lý event.

## Cách ghi nhớ

```text
State      = hiện tại đang ở đâu?
History    = đã đi qua những trạng thái nào?
Event      = chuyện gì đã xảy ra?
Invariant  = điều gì không được phép sai?
Transaction boundary = những bước nào phải đi cùng nhau?

Atomicity   = làm hết hoặc không làm gì
Consistency  = dữ liệu luôn đúng rule
Isolation    = transaction không phá nhau
Durability   = commit rồi thì không mất

Concurrent update = nhiều request cùng sửa dữ liệu
Lost update       = update bị request khác ghi đè
Race condition    = kết quả phụ thuộc vào timing

Pessimistic lock  = khóa trước khi sửa
Optimistic lock   = kiểm tra version trước khi ghi
Deadlock          = các transaction chờ lock của nhau

Idempotency       = retry nhiều lần nhưng hiệu ứng chỉ xảy ra một lần
Retry-safe        = chạy lại không làm dữ liệu sai hoặc bị nhân đôi
```
