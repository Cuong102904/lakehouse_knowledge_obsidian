---
type: concept
status: learning
domain: distributed-systems
tags:
  - data-engineering
  - distributed-systems
  - cap-theorem
---

# CAP Theorem

## Định nghĩa

CAP Theorem mô tả trade-off của một hệ thống phân tán khi xảy ra network partition.

CAP gồm ba thuộc tính:

- **Consistency**: các node đọc dữ liệu thấy cùng một giá trị hợp lệ hoặc giá trị mới nhất theo guarantee của hệ thống.
- **Availability**: mọi request hợp lệ đều nhận được response, kể cả khi một phần hệ thống gặp lỗi.
- **Partition Tolerance**: hệ thống vẫn tiếp tục hoạt động khi các node không thể liên lạc ổn định với nhau.

## Ba thành phần

### Consistency

Sau khi một thao tác ghi thành công, các lần đọc tiếp theo nhìn thấy cùng dữ liệu theo guarantee consistency của hệ thống.

Ví dụ: sau khi số dư tài khoản được cập nhật, request đọc không được thấy số dư cũ ở một node khác nếu hệ thống yêu cầu strong consistency.

### Availability

Mỗi request gửi đến một node không bị lỗi đều nhận được response. Response có thể là dữ liệu cũ hơn nếu hệ thống chấp nhận eventual consistency.

### Partition Tolerance

Khi xảy ra lỗi mạng giữa các node, hệ thống vẫn phải tiếp tục xử lý ở một mức độ nào đó.

Trong hệ thống phân tán thực tế, network partition là sự cố có thể xảy ra. Vì vậy Partition Tolerance thường được xem là điều kiện cần.

## Trade-off của CAP

Khi không có network partition, hệ thống có thể cung cấp cả consistency và availability.

Khi network partition xảy ra, hệ thống phải ưu tiên một trong hai:

```text
Network partition
        ↓
Ưu tiên Consistency
hoặc
Ưu tiên Availability
```

- Chọn **Consistency**: có thể từ chối hoặc trì hoãn request để tránh trả về dữ liệu không nhất quán.
- Chọn **Availability**: tiếp tục trả response, nhưng dữ liệu giữa các node có thể tạm thời khác nhau.

## Liên hệ với Data Engineering

CAP giúp Data Engineer hiểu các trade-off khi thiết kế:

- Database phân tán.
- Replication.
- Data warehouse phân tán.
- Streaming system.
- CDC pipeline.
- Distributed storage.

Ví dụ, một hệ thống phân tích có thể chấp nhận dữ liệu đến trễ vài giây để giữ khả năng đọc và ingest dữ liệu. Ngược lại, hệ thống thanh toán thường cần ưu tiên consistency và transaction integrity.

## Không nên hiểu sai CAP

- CAP không có nghĩa hệ thống chỉ được chọn đúng hai thuộc tính trong mọi tình huống.
- Trade-off quan trọng nhất xuất hiện khi network partition xảy ra.
- Không nên gắn cứng mọi database OLTP vào một nhãn CAP cụ thể hoặc mọi database OLAP vào một nhãn khác.
- CAP khác với ACID. CAP nói về hệ thống phân tán; ACID nói về tính chất của transaction.

## Câu hỏi tự kiểm tra

1. CAP là viết tắt của ba thuộc tính nào?
2. Vì sao Partition Tolerance quan trọng trong hệ thống phân tán?
3. Khi network partition xảy ra, Consistency và Availability xung đột như thế nào?
4. Vì sao CAP khác với ACID?
5. Khi nào một hệ thống có thể ưu tiên Availability?
6. Khi nào một hệ thống cần ưu tiên Consistency?

## Ghi chú liên quan

- [[03-Data-Warehousing/OLTP-and-OLAP]]
