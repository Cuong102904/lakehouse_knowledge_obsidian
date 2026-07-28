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

ACID là bốn thuộc tính giúp database xử lý transaction một cách đáng tin cậy. Một
**transaction** là một nhóm thao tác được database xem như một đơn vị logic. Trong
OLTP, transaction thường tương ứng với một business operation như tạo order, trừ
tồn kho hoặc chuyển tiền.

## 1. Atomicity

Atomicity nghĩa là transaction phải được thực hiện **toàn bộ hoặc không thực hiện
gì cả**. Nếu một bước thất bại, các bước đã thực hiện trước đó phải được rollback.

Ví dụ chuyển tiền:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 100000
WHERE id = 'A';

UPDATE accounts
SET balance = balance + 100000
WHERE id = 'B';

COMMIT;
```

Hai thao tác trên phải cùng thành công. Nếu bước cộng tiền cho B thất bại, database
phải hoàn tác bước trừ tiền ở A. Không được tồn tại trạng thái A đã bị trừ tiền
nhưng B chưa nhận được tiền.

Database hỗ trợ Atomicity bằng transaction log, thông tin để `UNDO` và các lệnh
`COMMIT`/`ROLLBACK`. Nếu database crash trước khi transaction commit, cơ chế
recovery sẽ loại bỏ các thay đổi dở dang. Chỉ những thay đổi của transaction đã
commit mới được giữ lại.

## 2. Consistency

Consistency nghĩa là transaction đưa database từ **một trạng thái hợp lệ sang một
trạng thái hợp lệ khác**. Sau transaction, dữ liệu vẫn phải tuân thủ constraint và
business invariant.

Ví dụ, nếu số dư không được âm:

```sql
UPDATE accounts
SET balance = balance - 100000
WHERE id = 'A'
  AND balance >= 100000;
```

Transaction phải thất bại hoặc không cập nhật nếu tài khoản không đủ tiền.

Consistency thường được bảo vệ bởi:

- Database constraint: `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL`, `CHECK`.
- Transaction boundary: các thay đổi liên quan phải commit hoặc rollback cùng nhau.
- Business logic: các quy tắc như order đã `SHIPPED` không được quay lại `PENDING`.
- Data validation: kiểm tra dữ liệu đầu vào trước khi ghi.

Có thể phân biệt hai lớp consistency:

- **Database consistency**: `quantity > 0`, `price >= 0`, foreign key phải tồn tại.
- **Business consistency**: một payment chỉ được ghi nhận thành công một lần,
  hoặc số lượng reserved không được vượt quá số lượng available.

Database chỉ tự bảo vệ được các rule đã được khai báo hoặc triển khai. Nó không thể
tự suy luận những business rule chưa được định nghĩa.

## 3. Isolation

Isolation nghĩa là các transaction chạy đồng thời nhưng không nhìn thấy hoặc ghi đè
dữ liệu của nhau theo cách tạo ra kết quả sai.

Ví dụ, hai transaction cùng đọc số dư 1.000.000đ, sau đó lần lượt muốn trừ
100.000đ và 200.000đ. Nếu không có cơ chế cô lập phù hợp, một kết quả có thể ghi đè
lên kết quả còn lại. Đây là lỗi **lost update**.

Các lỗi thường gặp khi isolation không đủ mạnh:

- **Dirty read**: đọc dữ liệu transaction khác đã thay đổi nhưng chưa `COMMIT`.
- **Non-repeatable read**: cùng đọc một dòng trong một transaction nhưng nhận hai
  giá trị khác nhau vì transaction khác đã cập nhật dòng đó.
- **Phantom read**: cùng chạy một truy vấn nhưng số dòng trả về thay đổi vì
  transaction khác thêm hoặc xóa các dòng phù hợp.

### Các hiện tượng đọc và ghi thường gặp

#### Dirty read

Transaction T1 thay đổi dữ liệu nhưng chưa commit. T2 đã đọc được thay đổi đó.
Sau đó T1 rollback, nên T2 đã đọc một giá trị chưa từng trở thành dữ liệu chính
thức.

```text
T1: UPDATE balance = 900.000   -- chưa COMMIT
T2: READ balance = 900.000
T1: ROLLBACK
```

Kết quả T2 đọc được là dữ liệu “bẩn”. `Read Committed` trở lên sẽ ngăn hiện tượng
này.

#### Non-repeatable read

T1 đọc cùng một dòng hai lần trong cùng transaction. Ở giữa hai lần đọc, T2 đã
commit một thay đổi trên dòng đó.

```text
T1: READ balance = 1.000.000
T2: UPDATE balance = 900.000; COMMIT
T1: READ balance = 900.000
```

T1 không nhận được cùng một kết quả cho cùng một dòng. `Repeatable Read` thường
được dùng khi transaction cần giữ kết quả ổn định của các dòng đã đọc.

#### Phantom read

T1 chạy một truy vấn theo điều kiện. T2 thêm hoặc xóa một dòng thỏa điều kiện đó,
sau đó T1 chạy lại cùng truy vấn và nhận được tập kết quả khác.

```text
T1: SELECT COUNT(*) FROM orders WHERE status = 'PENDING'; -- 10
T2: INSERT order có status = 'PENDING'; COMMIT
T1: SELECT COUNT(*) FROM orders WHERE status = 'PENDING'; -- 11
```

“Phantom” không phải là một dòng cũ bị thay đổi, mà là dòng mới xuất hiện hoặc biến
mất trong phạm vi truy vấn. `Serializable` là isolation level mạnh nhất để ngăn
hiện tượng này, dù cách triển khai cụ thể phụ thuộc database.

#### Lost update

Hai transaction cùng đọc một giá trị, tự tính giá trị mới rồi ghi lại. Transaction
ghi sau có thể ghi đè kết quả của transaction ghi trước.

```text
Số dư ban đầu: 1.000.000

T1: đọc 1.000.000 → tính còn 900.000
T2: đọc 1.000.000 → tính còn 800.000
T1: ghi 900.000
T2: ghi 800.000
```

Thay đổi của T1 bị mất. Có thể tránh bằng atomic update, row lock, optimistic
locking hoặc kiểm tra version:

```sql
UPDATE accounts
SET balance = balance - 100000,
    version = version + 1
WHERE id = 'A'
  AND version = 3;
```

Nếu số dòng bị update bằng `0`, transaction biết rằng dữ liệu đã bị transaction
khác thay đổi và có thể retry hoặc báo lỗi.

#### Write skew

Hai transaction đọc cùng một tập dữ liệu và mỗi transaction cập nhật một dòng khác
nhau. Từng transaction riêng lẻ có vẻ hợp lệ, nhưng khi cả hai commit, business
invariant chung bị phá vỡ.

Ví dụ hệ thống yêu cầu luôn có ít nhất một bác sĩ trực:

```text
T1: thấy bác sĩ B vẫn đang trực → cho bác sĩ A nghỉ
T2: thấy bác sĩ A vẫn đang trực → cho bác sĩ B nghỉ
T1 và T2 cùng COMMIT
```

Kết quả cuối cùng không còn bác sĩ nào trực. Trường hợp này cần lock phù hợp,
constraint, hoặc isolation mạnh hơn tùy cách thiết kế dữ liệu.

Database thường dùng row-level lock, table-level lock hoặc MVCC
(*Multi-Version Concurrency Control*) để kiểm soát việc đọc và ghi đồng thời.

Các isolation level phổ biến:

| Isolation level | Đặc điểm |
|---|---|
| `Read Uncommitted` | Có thể đọc dữ liệu chưa commit; hiệu năng cao nhưng rủi ro lớn. |
| `Read Committed` | Chỉ đọc dữ liệu đã commit; thường là mức mặc định. |
| `Repeatable Read` | Dữ liệu của các dòng đã đọc ổn định trong transaction. |
| `Serializable` | Cô lập mạnh nhất, gần giống các transaction chạy tuần tự. |

Có thể ghi nhớ theo hướng các hiện tượng bị ngăn chặn như sau:

| Isolation level | Dirty read | Non-repeatable read | Phantom read |
|---|---:|---:|---:|
| `Read Uncommitted` | Có thể xảy ra | Có thể xảy ra | Có thể xảy ra |
| `Read Committed` | Không xảy ra | Có thể xảy ra | Có thể xảy ra |
| `Repeatable Read` | Không xảy ra | Không xảy ra | Tùy database/cơ chế triển khai |
| `Serializable` | Không xảy ra | Không xảy ra | Không xảy ra |

Bảng trên mô tả cách hiểu phổ biến. Hành vi chính xác của `Repeatable Read`, MVCC
và việc khóa range có thể khác giữa PostgreSQL, MySQL, SQL Server và các database
khác.

Isolation càng mạnh thì độ an toàn càng cao, nhưng có thể làm giảm concurrency do
phải chờ lock, tạo thêm version hoặc retry transaction.

## 4. Durability

Durability nghĩa là sau khi database trả về `COMMIT` thành công, thay đổi phải được
bảo toàn kể cả khi database bị crash, process bị kill hoặc server mất điện.

Database thường sử dụng **WAL (Write-Ahead Logging)**. Nguyên tắc của WAL là phải
ghi log xuống storage trước khi ghi data page chính thức:

```text
1. Transaction tạo thay đổi trong memory
2. Database ghi thông tin thay đổi vào transaction log
3. Log được flush xuống disk
4. Ghi commit record
5. Trả về COMMIT thành công
6. Data page có thể được ghi xuống disk sau
```

Nếu database crash sau khi commit nhưng trước khi data page được ghi đầy đủ,
database đọc WAL khi khởi động lại và dùng `REDO` để áp dụng lại thay đổi đã commit.
Ngược lại, transaction chưa commit sẽ được `UNDO` để loại bỏ thay đổi dở dang.

Durability thường được củng cố thêm bằng checkpoint, replication, backup và storage
có độ bền cao. Tuy nhiên, replication không đồng nghĩa tuyệt đối với backup: dữ liệu
bị xóa hoặc bị ghi sai có thể được replicate sang node khác.

## ACID trong một transaction OLTP

Ví dụ mua sản phẩm cuối cùng trong kho. Các bước giảm kho, tạo `order_item`, đổi trạng thái order và ghi lịch sử cần được xử lý đúng transaction boundary. Nếu tạo item thất bại thì việc giảm kho cũng phải rollback.

```text
BEGIN
  kiểm tra và giảm available_quantity
  tạo order_item
  cập nhật trạng thái order
  ghi order history
COMMIT
```

- **Atomicity**: giảm kho, tạo item và ghi history cùng thành công hoặc cùng rollback.
- **Consistency**: số lượng kho không âm, order và item tham chiếu đúng nhau.
- **Isolation**: hai request đồng thời không cùng reserve một sản phẩm cuối cùng.
- **Durability**: sau `COMMIT`, order và thay đổi tồn kho vẫn tồn tại sau khi database restart.

## Giới hạn của ACID

Database chỉ rollback được thao tác bên trong database. Nó không tự rollback được email đã gửi, payment đã thực hiện hoặc request đã gửi sang service khác. Vì vậy không nên giữ database transaction để chờ external service.

Khi một business operation liên quan đến nhiều service, thường cần kết hợp state
machine, idempotency, retry, outbox pattern hoặc reconciliation. Đây là cách xử lý
phần công việc nằm ngoài transaction boundary của database.

## Tóm tắt

```text
Atomicity  = Làm hết hoặc không làm gì
Consistency = Dữ liệu luôn hợp lệ
Isolation   = Các transaction không phá lẫn nhau
Durability  = Đã COMMIT thì không mất
```

ACID tăng độ tin cậy cho OLTP nhưng đi kèm chi phí về transaction log, lock,
concurrency và cơ chế recovery. Vì vậy transaction nên chứa đúng các thao tác cần
được commit hoặc rollback cùng nhau, không nên giữ transaction trong lúc chờ
external service.
