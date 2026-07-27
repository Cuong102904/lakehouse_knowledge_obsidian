---
type: concept
status: learning
domain: data-modeling
tags:
  - data-engineering
  - data-modeling
  - oltp
---

# OLTP Modeling

Các note trong folder này được tách theo từng concept để dễ học và tra cứu.

## Lộ trình đọc

1. [[01 - State Model và History Event Model]]
2. [[02 - Business Invariant và Constraint]]
3. [[03 - Transaction Boundary]]
4. [[04 - ACID trong OLTP]]
5. [[05 - Concurrent Update và Race Condition]]
6. [[06 - Locking trong OLTP]]
7. [[07 - Deadlock]]
8. [[08 - Idempotency và Retry-safe Operation]]
9. [[09 - Audit Trail]]
10. [[10 - Soft Delete]]
11. [[11 - Temporal Data]]
12. [[12 - Multi-tenancy]]
13. [[13 - Row-level Security]]
14. [[14 - Tổng hợp OLTP Modeling]]

## Bức tranh tổng quát

OLTP modeling bắt đầu từ việc mô hình hóa state hiện tại, xác định invariant cần bảo vệ, rồi xác định transaction boundary. Sau đó học cách xử lý concurrency, retry, audit, lịch sử dữ liệu và dữ liệu của nhiều tenant.

## Nhóm concept

### Transaction correctness

- [[01 - State Model và History Event Model]]
- [[02 - Business Invariant và Constraint]]
- [[03 - Transaction Boundary]]
- [[04 - ACID trong OLTP]]

### Concurrency và reliability

- [[05 - Concurrent Update và Race Condition]]
- [[06 - Locking trong OLTP]]
- [[07 - Deadlock]]
- [[08 - Idempotency và Retry-safe Operation]]

### Data lifecycle và access isolation

- [[09 - Audit Trail]]
- [[10 - Soft Delete]]
- [[11 - Temporal Data]]
- [[12 - Multi-tenancy]]
- [[13 - Row-level Security]]



