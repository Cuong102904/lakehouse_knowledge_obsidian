# Ôn phỏng vấn TPBank – Fresh DE 2026 (vòng Interview)

> Đã qua vòng IQ và Technical test. Vòng này tập trung vào **câu hỏi cơ bản / HR**, hỏi bằng tiếng Việt.
> Nắm **khung + ý chính**, đừng học thuộc từng chữ. Mỗi câu nói khoảng **60–90 giây**.
> Nguyên tắc: **không nói quá** vai trò của mình. Mình *tham gia / phụ trách một phần*, có sếp hướng dẫn. Hội đồng hỏi đào sâu một câu là lộ ngay nếu nói quá.

---

## Tiến độ

| # | Câu hỏi | Trạng thái |
|---|---------|------------|
| 1 | Vì sao em nghỉ ở công ty cũ? | Có bản trả lời |
| 2 | Tại sao em ứng tuyển vị trí này? | Có bản trả lời, cần điền ví dụ về TPBank |
| 3 | Mục tiêu 3–5 năm tới? | Có bản trả lời |
| 4 | Điểm mạnh và điểm yếu? | Có bản trả lời |
| 5 | *(chưa có câu hỏi)* | Chờ bổ sung |

---

## Câu 1: Vì sao em nghỉ ở công ty cũ?

**HR muốn biết gì:** HR không chỉ hỏi lý do nghỉ. Họ quan sát:
- em nhìn nhận công ty cũ như thế nào,
- em xử lý sự bất mãn ra sao,
- em có thích nghi được với môi trường mới không.

**Khung trả lời:** không chê công ty cũ → nói điểm tốt trước → giải thích lý do nghỉ một cách khách quan, tập trung vào điều *không còn phù hợp với định hướng của mình* → chốt bằng điều mình đang tìm kiếm ở công việc mới.

**Câu trả lời:**
> "Em rất biết ơn Amira. Em vào từ khi còn là thực tập sinh và được tham gia trực tiếp vào hệ thống dữ liệu thật, từ crawl, chuẩn hoá đến kiểm tra chất lượng dữ liệu. Em học được nhiều nhất là nhờ các anh đi trước hướng dẫn. Ví dụ khi tối ưu bước chuẩn hoá bằng Daft, em tự làm phần thử nghiệm và triển khai, còn sếp định hướng cho em cách tiếp cận.
>
> Từ tháng 6, công ty ưu tiên sản phẩm AI nên công việc của em chuyển sang AI và backend. Đây là hướng đi hợp lý của công ty, và em vẫn hoàn thành phần việc được giao. Nhưng qua giai đoạn đó, em xác định rõ hơn là mình muốn đi sâu vào data engineering.
>
> Vì vậy em tìm một môi trường DE chuyên sâu, có quy trình bài bản và có người hướng dẫn để phát triển lâu dài."

**Ghi chú:**
- Gọi việc công ty đổi hướng là "hợp lý" cho thấy mình nhìn nhận khách quan. Ý "vẫn hoàn thành việc được giao" cho thấy mình thích nghi được.
- Trong form mình ghi lý do nghỉ là "môi trường mới, thử thách hơn". Khi nói, hiểu đó là *thử thách về chuyên môn DE*.
- ❌ Tránh nhắc đến lương, sếp, "công ty không có việc cho em".
- ⚠️ Form ghi lương ở Amira: khởi điểm 15.5tr, cuối cùng 14tr. Cần chuẩn bị 1 câu giải thích ngắn và thật nếu HR hỏi vì sao lương giảm. *(chưa có, cần bổ sung)*

---

## Câu 2: Tại sao em ứng tuyển vị trí này?

**HR muốn biết gì:**
- Em có thực sự hiểu mình đang apply vào vị trí gì không?
- Em có kinh nghiệm hoặc kỹ năng nào liên quan đến JD?
- Em có hiểu công việc này khác gì những vị trí tương tự không?
- Quan trọng nhất: em đã tìm hiểu công ty chưa?

**Khung trả lời:** hiểu vị trí → kinh nghiệm → yêu cầu JD → khác biệt so với vị trí tương tự → hiểu công ty → vì sao hợp với định hướng của mình.

**Câu trả lời:**
> "Theo em hiểu, Fresh DE là người xây dựng và vận hành các luồng dữ liệu: thu thập, xử lý và tích hợp dữ liệu vào kho để Data Analyst và Data Scientist khai thác. Khác với DA hay DS, DE không trực tiếp phân tích hay xây mô hình. Việc của DE là đảm bảo dữ liệu đến **đúng, đủ, kịp thời và đáng tin cậy**.
>
> Kinh nghiệm của em khớp với một số ý trong JD:
> - Em **tham gia xây dựng và bảo trì** pipeline ETL lấy dữ liệu từ nhiều nguồn vào lakehouse, chạy bằng Airflow. Phần em làm nhiều là **chuẩn hoá và khử trùng lặp dữ liệu**.
> - Em làm **kiểm tra chất lượng dữ liệu** và theo dõi qua dashboard trên Trino.
> - Em có tham gia **tối ưu hiệu năng**: dưới sự hướng dẫn của sếp, em chuyển một số bước từ PySpark sang Daft, giảm khoảng 60% thời gian của bước đó.
> - Em đã dùng Spark, Airflow, Hive/Trino trong công việc. Kafka thì em dùng trong dự án cá nhân LearnLake.
>
> Nhưng DE ở ngân hàng khác DE ở startup: dữ liệu tài chính đòi hỏi độ chính xác tuyệt đối, có đối soát, bảo mật và tuân thủ. Đây là phần em chưa có và muốn được học bài bản.
>
> Về TPBank, em biết ngân hàng định hướng là ngân hàng số, *[điền ví dụ cụ thể]*. Với ngân hàng số thì dữ liệu là hạ tầng cốt lõi. Chương trình Fresh DE lại có đào tạo từ nền tảng, có mentor và có lộ trình lên DE chính thức, đúng với mong muốn phát triển lâu dài trong mảng dữ liệu tài chính của em."

**Ghi chú:**
- 📌 **Cần làm:** lên website TPBank, lấy 1–2 sản phẩm hoặc giải thưởng về ngân hàng số để điền vào chỗ *[điền ví dụ cụ thể]*. *(chưa kiểm chứng, phải tự kiểm tra)*
- Câu này rất dễ kéo theo câu hỏi phụ: **"Em đã làm DE hơn 1 năm, sao lại vào chương trình tập sự?"** Ý trả lời: kinh nghiệm của em là ở startup với dữ liệu crawl web. Ngân hàng là domain khác hẳn, nên em cần nền tảng bài bản và chấp nhận bắt đầu lại để đi đường dài.

---

## Câu 3: Mục tiêu của em trong 3–5 năm tới là gì?

**HR muốn biết gì:** đây không đơn giản là câu hỏi về tham vọng. HR muốn hiểu em định hướng sự nghiệp thế nào, và định hướng đó có phù hợp với vị trí, với cấu trúc của công ty không.
- Vị trí chuyên môn sâu cần người muốn phát triển expertise lâu dài.
- Vị trí có lộ trình quản lý rõ thì nói về leadership sẽ hợp hơn.

→ Fresh DE là **lộ trình chuyên môn** (đào tạo → DE chính thức), nên trả lời theo hướng **chuyên sâu lâu dài**. Câu trả lời cũng phải khớp với lựa chọn trong form: *"Ổn định và phát triển theo công việc chuyên môn hiện tại"*.

**Câu trả lời:**
> "Em xác định gắn bó lâu dài với data engineering, và chia mục tiêu thành 3 giai đoạn:
> - **Năm đầu:** hoàn thành tốt chương trình đào tạo, nắm hệ thống dữ liệu và nghiệp vụ ngân hàng như tài khoản, giao dịch, tín dụng, và trở thành DE chính thức.
> - **Năm 2–3:** tự chịu trách nhiệm trọn vẹn một số luồng dữ liệu, từ thiết kế ETL, mô hình hoá dữ liệu trong kho đến đảm bảo chất lượng và xử lý sự cố khi vận hành.
> - **Năm 3–5:** trở thành một DE vững cả kỹ thuật lẫn nghiệp vụ, có thể đảm nhận một mảng kỹ thuật và hướng dẫn lại các bạn Fresh DE khoá sau."

**Ghi chú:**
- ❌ Tránh: "3 năm lên quản lý", "đi du học", "chuyển sang AI". Những câu này làm HR lo em học xong rồi nghỉ.
- Ý "hướng dẫn khoá sau" thể hiện leadership kỹ thuật mà không lệch khỏi hướng chuyên môn.

---

## Câu 4: Điểm mạnh và điểm yếu của em là gì?

**HR muốn biết gì:** em hiểu mình có thể đóng góp mạnh nhất ở đâu, và biết mình cần nguồn lực gì để làm tốt hơn.

**Khung trả lời:** điểm mạnh kèm bằng chứng → đóng góp được gì cho JD; điểm yếu → đang khắc phục thế nào → cần nguồn lực gì.

**Câu trả lời:**
> "**Điểm mạnh:** em thấy có hai điểm.
>
> Thứ nhất, em đã có hơn một năm làm việc với hệ thống dữ liệu thật, quen với Airflow, Spark, lakehouse và quy trình vận hành. Vì vậy khi vào chương trình em có thể bắt nhịp nhanh.
>
> Thứ hai, em học nhanh khi có định hướng. Daft là công cụ em chưa dùng bao giờ. Sau khi sếp gợi ý hướng làm, em tự tìm hiểu, thử nghiệm, đo kết quả trước và sau rồi mới đưa vào chạy thật. Em nghĩ cách học này phù hợp với một chương trình đào tạo như Fresh DE.
>
> **Điểm cần cải thiện:** em cũng có hai điểm.
>
> Một là em muốn trình bày ngắn gọn hơn khi trao đổi vấn đề kỹ thuật với người không cùng chuyên môn. Hiện em tập nói theo thứ tự: vấn đề, nguyên nhân, hướng xử lý, rồi mới đi vào chi tiết nếu người nghe cần.
>
> Hai là kiến thức nghiệp vụ ngân hàng của em còn mỏng. Đây là phần em mong được hỗ trợ nhiều nhất qua chương trình đào tạo và mentor. Song song đó em đang tự học thêm về mô hình hoá kho dữ liệu theo Kimball."

**Ghi chú:**
- Điểm yếu thứ nhất **trùng với điều đã ghi trong form**, giữ được tính nhất quán.
- Điểm yếu thứ hai trả lời đúng ý "cần nguồn lực gì", và nguồn lực đó chính là thứ chương trình cung cấp.
- Chỉ nhắc đến Kimball nếu thực sự đang đọc (xem [[03-Data-Warehousing/Kimball - Data Warehouse Toolkit]]).

---

## Câu 5: *(chưa có, cần bổ sung)*

---

## Lưu ý chung

- **Không nói quá:** CV viết "Built…", "Refactored 60+ DAGs…", "Cut… by ~60%". Khi bị hỏi sâu, nói rõ phạm vi: *"Phần này em làm cùng team, em phụ trách phần…"*.
- Không dùng lại câu C4 trong note VNPT-IT (*"không ai giao, em tự phát hiện"*), vì không đúng sự thật.
- Các thông tin trong form TPBank cần nói khớp: lương mong muốn 15tr (thấp nhất 13tr), ngày bắt đầu 01/11/2026, kế hoạch "ổn định, phát triển chuyên môn".
- Câu hỏi ngược cho nhà tuyển dụng (chọn 2):
  - Chương trình đào tạo kéo dài bao lâu, tiêu chí lên DE chính thức là gì?
  - Fresh DE được phân về team nào, team đang dùng stack gì?
  - Team dữ liệu hiện đang gặp thách thức lớn nhất ở đâu?

## Liên quan

- [[On-thi-TPBank-MCQ]]: ôn phần kỹ thuật (vòng test).
