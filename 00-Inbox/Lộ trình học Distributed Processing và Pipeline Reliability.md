---
type: learning-roadmap
status: learning
topic: Distributed Processing, Streaming Reliability, Data Integration
---

# Lộ trình học Distributed Processing và Pipeline Reliability

## Mục tiêu

Hiểu vì sao dữ liệu phân tán và thay đổi liên tục lại sinh ra các vấn đề về performance, correctness và chi phí, và biết dùng đúng kỹ thuật để xử lý từng vấn đề đó.

## Danh sách khái niệm gốc

partitioning, shuffle, skew, backpressure, checkpoint, idempotency, exactly-once / at-least-once, CDC, schema evolution, SCD, data quality, late-arriving data, small files, compaction, incremental processing, query optimization, cost optimization.

## Phân biệt trục khái niệm

Đây không phải 17 khái niệm rời rạc mà là 4 nhóm nhân-quả, cộng thêm một lớp kiểm tra xuyên suốt:

1. **Distributed execution** — nguyên nhân gốc của vấn đề performance.
2. **Streaming reliability** — đảm bảo đúng khi dữ liệu chảy liên tục.
3. **Data integration & change over time** — dữ liệu nguồn không đứng yên.
4. **Storage & table maintenance** — hệ quả vật lý của streaming/incremental load.
5. **Data quality** — lớp kiểm tra xuyên suốt, không thuộc riêng nhóm nào.

## Giai đoạn 1 — Distributed execution

**Mức độ: học sâu**

Chuỗi nhân quả một chiều, học đúng thứ tự:

```text
partitioning → shuffle → skew → query optimization → cost optimization
```

- Partitioning sai (key phân bố lệch, quá ít/quá nhiều partition) → gây shuffle nhiều hơn cần thiết.
- Shuffle nặng + phân bố dữ liệu lệch → data skew (một số partition/task quá tải).
- Skew không xử lý → query chậm, tài nguyên chờ nhau.
- Query chậm → tốn compute → cost tăng.

Nội dung cần học:

- Cách engine (Spark) quyết định số partition và partition key.
- Wide transformation vs narrow transformation, khi nào gây shuffle.
- Cách phát hiện skew (task time lệch nhau trong Spark UI) và kỹ thuật giảm skew (salting, broadcast join, repartition).
- Đọc query plan, physical plan, các phép toán gây tốn chi phí.

Vị trí trong vault: `02-Data-Engineering-Fundamentals/Distributed-Systems`, `05-Big-Data/Apache-Spark`.

## Giai đoạn 2 — Streaming reliability

**Mức độ: học sâu**

```text
backpressure → checkpoint → exactly-once / at-least-once → idempotency
```

- Backpressure: engine tự điều tốc khi consumer chậm hơn producer.
- Checkpoint: lưu lại vị trí xử lý để phục hồi sau lỗi.
- Checkpoint + cách commit offset quyết định mức đảm bảo delivery: exactly-once hay at-least-once.
- Khi engine chỉ đảm bảo at-least-once (message có thể đến trùng), idempotency ở tầng ứng dụng là lưới an toàn cuối cùng để kết quả không bị sai do xử lý trùng.

Lưu ý: đã có note idempotency ở context OLTP ([[08 - Idempotency và Retry-safe Operation]]). Đây là idempotency ở context streaming/pipeline — concept giống nhau nhưng ví dụ và failure mode khác (duplicate message do retry/replay, không phải duplicate request từ client). Viết note riêng, link hai chiều.

Vị trí trong vault: `02-Data-Engineering-Fundamentals/Stream-Processing`, `05-Big-Data/Structured-Streaming`.

## Giai đoạn 3 — Data integration & change over time

**Mức độ: học sâu**

```text
CDC → schema evolution → late-arriving data → SCD
```

- CDC bắt thay đổi ở nguồn (insert/update/delete).
- Thay đổi ở nguồn có thể kéo theo schema evolution (cấu trúc đổi) — pipeline phải chịu được mà không vỡ.
- Dữ liệu có thể đến muộn theo thời gian sinh ra (late-arriving data) — ảnh hưởng đến fact/dimension đã load trước đó.
- SCD là cách model hoá lịch sử thay đổi ở tầng dimensional, đặc biệt khi nguồn là CDC.

Đây nối trực tiếp vào roadmap Data Modeling đã có ở [[Lộ trình học Data Modeling]] (Giai đoạn 3 và 4) — không học lại từ đầu, chỉ bổ sung góc nhìn "nguồn dữ liệu thay đổi liên tục" thay vì batch tĩnh.

Vị trí trong vault: `02-Data-Engineering-Fundamentals/ETL-ELT`, `04-Data-Modeling/Slowly-Changing-Dimensions`.

## Giai đoạn 4 — Storage & table maintenance

**Mức độ: hiểu rõ, thực hành cơ bản**

```text
small files → compaction → incremental processing
```

- Streaming/CDC ghi liên tục, mỗi lần ghi ít dữ liệu → tạo nhiều file nhỏ.
- Nhiều file nhỏ làm chậm read và tăng chi phí metadata → cần compaction để gộp lại.
- Incremental processing là chiến lược load chỉ phần dữ liệu thay đổi, tránh full-refresh — vừa giảm small files vừa giảm cost.

Vị trí trong vault: `03-Data-Warehousing/Data-Lakehouse`, `07-Databricks/Delta-Lake`.

## Lớp xuyên suốt — Data quality

**Học sau, xen vào cuối mỗi giai đoạn như bài tập viết check**

Data quality không thuộc riêng nhóm nào, nó là cái kiểm tra hậu quả của các nhóm trên:

- Skew xử lý sai → kết quả tổng hợp có thể sai lệch ở một số partition.
- Duplicate delivery không idempotent → dữ liệu bị đúp.
- Schema evolution không kiểm soát → pipeline vỡ hoặc âm thầm sai kiểu dữ liệu.
- Late-arriving data không xử lý → báo cáo ở kỳ trước bị thiếu rồi lại đổi sau khi đã report.

Vị trí trong vault: `10-Data-Platform-Practices/Data-Quality`.

## Trình tự học ngắn gọn

```text
Distributed execution (partitioning, shuffle, skew, query/cost optimization)
        ↓
Streaming reliability (backpressure, checkpoint, exactly-once, idempotency)
        ↓
Data integration & change (CDC, schema evolution, late-arriving data, SCD)
        ↓
Storage & table maintenance (small files, compaction, incremental processing)
        ↓
Data quality (viết check cho từng failure mode ở trên)
```

## Câu hỏi tự kiểm tra

- Partition key hiện tại có gây skew không, dấu hiệu nhận biết là gì?
- Pipeline này đảm bảo mức delivery nào, và operation có idempotent để bù cho mức đó không?
- Nếu source đổi schema, pipeline sẽ fail cứng hay tự thích nghi — cái nào là chủ đích?
- Dữ liệu đến muộn bao lâu thì ảnh hưởng đến báo cáo đã publish?
- Bảng này có đang bị small files không, compaction chạy theo lịch nào?
- Query này tốn chi phí ở bước nào trong physical plan?

## Liên kết liên quan

- [[Lộ trình học Data Modeling]]
- [[08 - Idempotency và Retry-safe Operation]]
