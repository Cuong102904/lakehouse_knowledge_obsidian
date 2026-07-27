---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# Temporal Data

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
