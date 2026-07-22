---
type: concept
status: learning
domain: data-warehousing
tags:
  - data-engineering
  - data-warehouse
  - oltp
  - olap
---

# OLTP và OLAP

## Bức tranh tổng quát

OLTP và OLAP là hai kiểu workload khác nhau trong hệ thống dữ liệu:

```text
Ứng dụng / Website
        ↓
OLTP Database
        ↓ ETL / ELT / CDC
Data Warehouse / OLAP System
        ↓
Phân tích và báo cáo
```

- **OLTP** phục vụ giao dịch vận hành hằng ngày.
- **OLAP** phục vụ việc đọc, tổng hợp và phân tích dữ liệu lịch sử.
- Data Warehouse thường được xây cho workload OLAP.

## OLTP là gì?

OLTP là viết tắt của **Online Transaction Processing**.

OLTP xử lý các giao dịch nghiệp vụ nhỏ, thường xuyên và cần phản hồi nhanh.

Ví dụ trong hệ thống bán hàng:

- Tạo đơn hàng.
- Cập nhật trạng thái thanh toán.
- Trừ tồn kho.
- Cập nhật địa chỉ khách hàng.
- Lấy một khách hàng theo `customer_id`.

```sql
UPDATE inventory
SET quantity = quantity - 1
WHERE product_id = 101;
```

```sql
SELECT *
FROM orders
WHERE order_id = 5001;
```

Đặc điểm của OLTP:

- Đọc hoặc ghi một vài dòng mỗi lần.
- Có nhiều giao dịch đồng thời.
- Cần dữ liệu nhất quán ngay sau giao dịch.
- Dùng transaction và đảm bảo ACID.
- Thường dùng row-oriented storage.
- Thường dùng normalized schema.
- Tối ưu cho point lookup và single-row mutation.

Ví dụ database OLTP: PostgreSQL, MySQL, SQL Server và Oracle.

## OLAP là gì?

OLAP là viết tắt của **Online Analytical Processing**.

OLAP xử lý các truy vấn phân tích trên lượng lớn dữ liệu lịch sử.

Ví dụ:

- Tính doanh thu theo tháng.
- So sánh doanh thu giữa các quốc gia.
- Tìm sản phẩm bán chạy nhất.
- Phân tích xu hướng trong nhiều năm.

```sql
SELECT
    country,
    SUM(order_value) AS revenue
FROM orders
WHERE created_at >= '2026-01-01'
GROUP BY country
ORDER BY revenue DESC;
```

Đặc điểm của OLAP:

- Đọc rất nhiều dòng cùng lúc.
- Thực hiện `GROUP BY`, `SUM`, `AVG`, `COUNT` và các phép tổng hợp.
- Phục vụ dashboard, reporting và ad-hoc analysis.
- Tập trung vào dữ liệu lịch sử.
- Thường dùng column-oriented storage.
- Có thể dùng denormalized model hoặc star schema.
- Tối ưu cho bulk insert và analytical reads.

Ví dụ database OLAP: Snowflake, BigQuery, Redshift, Databricks SQL, ClickHouse và DuckDB.

## So sánh OLTP và OLAP

| Đặc điểm | OLTP | OLAP |
|---|---|---|
| Mục đích | Vận hành ứng dụng | Phân tích dữ liệu |
| Kiểu query | Đọc/ghi một vài dòng | Quét và tổng hợp nhiều dòng |
| Ví dụ | Tạo đơn hàng | Tính doanh thu theo tháng |
| Dữ liệu | Dữ liệu hiện tại | Dữ liệu lịch sử |
| Schema | Thường normalized | Thường denormalized hoặc star schema |
| Storage | Row-oriented | Column-oriented |
| Tối ưu cho | Giao dịch nhỏ, nhiều concurrency | Scan, filter và aggregation |
| Database ví dụ | PostgreSQL, MySQL | Snowflake, BigQuery, ClickHouse |

## Vì sao không dùng OLTP để làm phân tích?

Hai workload có tính chất khác nhau:

```text
OLTP:
Một query → một vài dòng → phản hồi ngay

OLAP:
Một query → hàng triệu dòng → filter, join, aggregate
```

Nếu chạy query phân tích nặng trên database của ứng dụng, query đó có thể chiếm CPU, memory và I/O, làm chậm các thao tác của người dùng.

Vì vậy hệ thống thường tách:

```text
OLTP Database  = phục vụ ứng dụng
OLAP Warehouse = phục vụ phân tích
```

Dữ liệu được đồng bộ từ OLTP sang OLAP bằng ETL, ELT hoặc CDC.

## CAP và OLTP/OLAP

CAP không phải là trọng tâm của note này. Xem phần lý thuyết đầy đủ tại [[02-Data-Engineering-Fundamentals/Distributed-Systems/CAP-Theorem]].

Ở đây chỉ cần nhớ cách CAP liên hệ với hai loại hệ thống:

- **OLTP** thường ưu tiên Consistency vì giao dịch nghiệp vụ phải chính xác ngay. Ví dụ chuyển tiền không thể chỉ trừ tiền tài khoản A mà chưa cộng tiền cho tài khoản B.
- **OLAP** trong một số hệ thống phân tán có thể chấp nhận dữ liệu trễ một khoảng thời gian ngắn để ưu tiên Availability, throughput và khả năng đọc dữ liệu lớn.

Ví dụ dashboard doanh thu có thể chưa hiển thị đơn hàng vừa phát sinh vài giây trước. Điều này có thể chấp nhận được nếu dữ liệu sẽ được đồng bộ sau đó.

Đây là một trade-off trong thiết kế hệ thống, không có nghĩa mọi OLTP luôn là CP hoặc mọi OLAP luôn là AP.

## Cách ghi nhớ

- **OLTP**: hệ thống cần làm gì ngay bây giờ?
- **OLAP**: dữ liệu lịch sử cho chúng ta biết điều gì?
- **CAP trong bối cảnh này**: hệ thống ưu tiên dữ liệu nhất quán ngay hay tiếp tục phục vụ khi có vấn đề về kết nối giữa các node?

## Câu hỏi tự kiểm tra

1. Vì sao tạo đơn hàng là workload OLTP?
2. Vì sao tính doanh thu theo quốc gia là workload OLAP?
3. Row-oriented và column-oriented phù hợp với hai workload này như thế nào?
4. Tại sao không nên chạy analytical query nặng trên database của ứng dụng?
5. Vì sao OLTP thường yêu cầu consistency mạnh hơn OLAP?
6. Vì sao một dashboard OLAP đôi khi có thể chấp nhận dữ liệu trễ vài giây?

## Nguồn tham khảo

- [OLTP vs OLAP - ClickHouse](https://clickhouse.com/resources/engineering/oltp-vs-olap)
