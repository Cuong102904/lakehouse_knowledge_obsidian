---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# State, Invariant và Transaction Boundary trong OLTP

## Bức tranh tổng quát

Khi thiết kế OLTP, không chỉ cần biết dữ liệu nằm trong bảng nào. Cần biết:

1. Entity đang ở **state** nào.
2. Điều kiện nào phải luôn đúng (**business invariant**).
3. Những thao tác nào phải thành công hoặc thất bại cùng nhau (**transaction boundary**).

Ví dụ dùng xuyên suốt: khách đặt mua sản phẩm cuối cùng trong kho.

## 1. State model và history/event model

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

## 2. Business invariant

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

## 3. Transaction boundary

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

## Ví dụ hoàn chỉnh: thanh toán order

```text
Transaction:
  ghi payment thành công
  đổi order từ PENDING sang PAID
  ghi order_status_history
  tạo event PaymentCompleted
```

Các thay đổi database trên nên thành công hoặc thất bại cùng nhau. Nếu client retry request, cần dùng `idempotency_key` để không tạo payment lần hai.

## 4. ACID trong workload thực tế

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

## 5. Concurrent update, lost update và race condition

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

## 6. Pessimistic locking và optimistic locking

### Pessimistic locking

Pessimistic locking giả định xung đột sẽ xảy ra, nên khóa row trước khi sửa:

```sql
BEGIN;

SELECT available_quantity
FROM inventory
WHERE product_id = 101
FOR UPDATE;

-- kiểm tra và giảm kho
-- tạo order item

COMMIT;
```

Transaction khác muốn sửa cùng row phải chờ transaction hiện tại kết thúc.

Phù hợp với inventory, số dư tài khoản hoặc quota có tranh chấp cao. Nhược điểm là transaction khác phải chờ, giữ lock lâu sẽ làm giảm throughput và có thể gây deadlock.

### Optimistic locking

Optimistic locking giả định xung đột ít xảy ra. Dữ liệu có thêm cột `version`:

```text
id = 101, balance = 100, version = 3
```

Khi update, chỉ ghi nếu version vẫn là version đã đọc:

```sql
UPDATE accounts
SET balance = 70,
    version = version + 1
WHERE id = 101
  AND version = 3;
```

Nếu kết quả là `0 rows affected`, dữ liệu đã bị transaction khác thay đổi. Application phải đọc lại, retry, tính toán lại hoặc báo conflict.

Phù hợp với chỉnh sửa profile, form hoặc metadata, nơi xung đột hiếm xảy ra.

| | Pessimistic locking | Optimistic locking |
|---|---|---|
| Cách xử lý | Khóa trước khi sửa | Kiểm tra `version` khi ghi |
| Khi conflict | Transaction sau phải chờ | Transaction sau bị từ chối |
| Phù hợp | Inventory, balance, quota | Profile, form, metadata |
| Rủi ro | Chờ lâu, deadlock | Conflict, cần retry |

## 7. Deadlock

Deadlock xảy ra khi hai transaction giữ những lock mà transaction còn lại đang cần.

```text
A khóa Account 1
B khóa Account 2
A muốn khóa Account 2 → chờ B
B muốn khóa Account 1 → chờ A
```

Ví dụ chuyển tiền, một transaction chuyển từ Account 1 sang 2, transaction khác chuyển từ Account 2 sang 1. Nếu mỗi transaction khóa account nguồn trước, chúng có thể khóa chéo nhau.

### Cách giảm deadlock

- Luôn khóa nhiều row theo cùng một thứ tự, ví dụ ID nhỏ trước.
- Giữ transaction ngắn.
- Chỉ lock những row cần thiết.
- Không gọi external service trong lúc đang giữ lock.
- Cấu hình lock timeout hoặc deadlock detection.
- Retry transaction nếu database rollback nó vì deadlock.

Deadlock có thể xảy ra trong hệ thống có concurrency cao. Application nên có cơ chế retry an toàn thay vì giả định deadlock không bao giờ xuất hiện.

## 8. Idempotency và retry-safe operation

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

## 9. Audit trail

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

## 10. Soft delete

Soft delete là không xóa vật lý record mà đánh dấu record đã bị xóa.

```text
deleted_at = NULL       → đang hoạt động
deleted_at có giá trị   → đã bị xóa
```

Ví dụ:

```sql
UPDATE products
SET deleted_at = CURRENT_TIMESTAMP
WHERE id = 101;
```

Query thông thường phải lọc record chưa bị xóa:

```sql
SELECT *
FROM products
WHERE deleted_at IS NULL;
```

Soft delete hữu ích khi cần khôi phục dữ liệu, giữ lịch sử, tránh xóa nhầm hoặc vẫn cần tham chiếu đến record cũ trong order.

Soft delete không phải là xóa hoàn toàn. Record vẫn chiếm dung lượng, vẫn chứa dữ liệu nhạy cảm và có thể xuất hiện nếu query quên lọc `deleted_at`.

### Soft delete và unique constraint

Nếu user cũ đã bị soft delete nhưng user mới muốn dùng lại email cũ, unique constraint thông thường có thể gây lỗi. Có thể dùng partial unique index nếu database hỗ trợ:

```sql
CREATE UNIQUE INDEX unique_active_email
ON users(email)
WHERE deleted_at IS NULL;
```

Ý nghĩa: chỉ các user chưa bị xóa mới phải có email unique.

### Những câu hỏi cần quyết định

- Record đã soft delete có được khôi phục không?
- User thông thường có được nhìn thấy không?
- Có được tạo dữ liệu mới liên quan đến record đó không?
- Bảng liên quan có bị soft delete theo không?
- Khi nào cần xóa vật lý để giải phóng dữ liệu?

## 11. Temporal data

Temporal data là dữ liệu có lịch sử theo thời gian, không chỉ lưu giá trị hiện tại mà còn biết giá trị nào có hiệu lực ở từng thời điểm.

Ví dụ lịch sử giá sản phẩm:

```text
01/01 - 10/01: giá 100
11/01 - 20/01: giá 120
Từ 21/01:      giá 150
```

Nếu chỉ lưu `products.price = 150`, không thể biết ngày 05/01 sản phẩm có giá bao nhiêu.

Có thể lưu các phiên bản:

```text
product_prices
--------------
product_id
price
valid_from
valid_to
```

Ví dụ:

```text
101 | 100 | 2026-01-01 | 2026-01-10
101 | 120 | 2026-01-11 | 2026-01-20
101 | 150 | 2026-01-21 | NULL
```

### Valid time và transaction time

- **Valid time**: khoảng thời gian giá trị có hiệu lực trong nghiệp vụ.
- **Transaction time**: thời điểm database biết hoặc lưu giá trị đó.

Ví dụ giá có hiệu lực từ ngày 11/01 nhưng nhân viên nhập vào database ngày 12/01. `valid_from` là 11/01, còn `created_at` là 12/01.

Các khoảng thời gian của cùng một entity không nên bị chồng lấn:

```text
Đúng: 01/01 - 10/01, 11/01 - 20/01
Sai:  01/01 - 15/01, 10/01 - 20/01
```

Nếu business yêu cầu dữ liệu luôn có hiệu lực thì cũng không được có khoảng trống giữa các phiên bản.

Trong order, nên lưu `unit_price` ngay tại thời điểm mua. Đây là snapshot, giúp invoice cũ không thay đổi khi giá sản phẩm thay đổi. Bảng `product_prices` vẫn có thể được dùng để lưu lịch sử giá.

## 12. Multi-tenancy

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

## 13. Row-level security

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

## 14. Kết hợp các concept

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

## Cách ghi nhớ thêm

```text
Audit trail   = ai thay đổi dữ liệu, thay đổi gì và lúc nào?
Soft delete   = đánh dấu đã xóa thay vì xóa vật lý
Temporal data = dữ liệu có lịch sử hiệu lực theo thời gian

Multi-tenancy = một hệ thống phục vụ nhiều tenant độc lập
tenant_id     = dữ liệu thuộc tenant nào
RLS           = database tự giới hạn row được đọc hoặc sửa
```
