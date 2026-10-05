# Ôn phỏng vấn TPBank – Fresh DE 2026 (vòng Interview)

> Đã qua vòng IQ và Technical test. Vòng này tập trung vào **câu hỏi cơ bản / HR**, hỏi bằng tiếng Việt.
> Nắm **khung + ý chính**, đừng học thuộc từng chữ. Nói kiểu nói chuyện bình thường, câu ngắn, không bay bổng.
> Nguyên tắc: **không nói quá** vai trò của mình. Hội đồng hỏi đào sâu một câu là lộ ngay nếu nói quá.

---

## Tiến độ

| # | Câu hỏi | Trạng thái |
|---|---------|------------|
| 0 | Giới thiệu bản thân | ✅ Đã chốt (2026-10-05) |
| 0b | Có kinh nghiệm rồi, sao vào chương trình cho người mới? | ✅ Đã chốt (2026-10-05) |
| 1 | Vì sao em nghỉ ở công ty cũ? | Có bản trả lời |
| 2 | Tại sao em ứng tuyển vị trí này? | Có bản trả lời, cần điền ví dụ về TPBank |
| 3 | Mục tiêu 3–5 năm tới? | Có bản trả lời |
| 4 | Điểm mạnh và điểm yếu? | Có bản trả lời |
| – | Câu hỏi đào sâu sau phần giới thiệu (CV, Daft, AI) | Có gợi ý, cần tự bổ sung chi tiết thật |

---

## Bối cảnh đã xác nhận (dùng để trả lời cho khớp)

- **Học vấn:** Đại học Bách khoa Hà Nội, Công nghệ thông tin (Global ICT), 10/2022 – 7/2026.
- **Amira:** công ty phần mềm có làm outsource, **không gọi là startup**. Nhưng **SalesSmart là sản phẩm của chính công ty**, không phải dự án làm cho khách.
- **Dự án 1, SalesSmart** (DE Intern → DE, 05/2025 – 06/2026): cơ sở dữ liệu doanh nghiệp và nhân sự Nhật Bản cho bán hàng B2B. Khoảng 60 nguồn crawl bằng Airflow → Delta Lake trên MinIO → Spark/Daft chuẩn hoá → hợp nhất theo `corporate_number` → upsert vào Postgres + Elasticsearch. **Phạm vi của mình dừng ở bước đưa vào Postgres/ES**, không làm web app.
- **Dự án 2, phân tích bằng sáng chế bằng AI** (từ 06/2026): làm cả backend lẫn một phần AI, gồm **embedding cho vector search** (Azure AI Search) và **viết prompt cho LLM**.
- **Daft:** chỉ chuyển **một số bước chuẩn hoá** dùng nhiều Python UDF và hàm pandas từ PySpark sang Daft. Con số **giảm ~60% thời gian chuẩn hoá mỗi lần chạy là thật**. PySpark vẫn giữ cho merge/upsert và kiểm tra chất lượng dữ liệu. Daft chạy trong 1 pod với Ray local.
- **Lương ở Amira:** 15.5tr → **14.7tr** (không phải 14tr). Chênh lệch là **do thuế**.
- **Hạ tầng:** K8s; Airflow tự host, mỗi task 1 pod; Spark chạy bằng pod executor; deploy qua ArgoCD/Helm.

---

## Câu 0: Giới thiệu bản thân ✅

**HR muốn biết gì:** em tóm tắt bản thân có mạch lạc không, hồ sơ có khớp vị trí không. Họ sẽ chọn điểm nào đó trong phần giới thiệu để hỏi tiếp.

**Khung:** học ở đâu → làm ở đâu → dự án 1 làm gì → dự án 2 làm gì → 1–2 câu về TPBank. Nói khoảng 70–80 giây.

**Câu trả lời:**
> "Dạ em chào anh chị. Em là Cường, em vừa tốt nghiệp Bách khoa, ngành Công nghệ thông tin.
>
> Hiện em đang làm ở Amira, được hơn một năm rồi, em vào từ hồi còn thực tập. Ở đây em làm hai dự án.
>
> Dự án đầu, cũng là dự án em làm lâu nhất, là SalesSmart, sản phẩm của công ty. Nói đơn giản thì nó là một cơ sở dữ liệu về các công ty và nhân sự ở Nhật, để đội sales tìm khách hàng. Em làm Data Engineer ở đây: lấy dữ liệu từ nhiều nguồn như trang tuyển dụng, tin tức, thông tin công ty, rồi làm sạch, chuẩn hoá, gộp dữ liệu của cùng một công ty lại, sau đó đẩy vào Postgres với Elasticsearch cho app dùng. Em làm chủ yếu với Airflow và Spark, hằng ngày em cũng theo dõi xem dữ liệu có bị lỗi hay thiếu không.
>
> Từ tháng 6 thì em chuyển sang dự án thứ hai, là một hệ thống dùng AI để phân tích bằng sáng chế. Ở đây em làm cả backend lẫn một phần AI: em làm phần embedding để tìm các bằng sáng chế liên quan bằng vector search, viết prompt cho AI phân tích từng bằng sáng chế, và viết API để đưa kết quả lên cho người dùng.
>
> Em muốn đi lâu dài với nghề DE. Em biết TPBank làm ngân hàng số khá mạnh, nên dữ liệu ở đây chắc chắn rất quan trọng. Vì thế em ứng tuyển chương trình Fresh DE ạ."

**Ghi chú:**
- Không kể chuyện Daft ở đây. Để dành cho câu "Kể về một khó khăn" hoặc "Em đã tối ưu gì?".
- Không nhắc chuyện "team có sẵn framework". Chỉ nói việc mình đã làm.
- Câu "viết API để đưa kết quả lên cho người dùng" được suy ra từ CV (FastAPI, SSE). Nếu không đúng thì bỏ.
- Phần về TPBank chỉ nói 1–2 câu. Lý do chi tiết để dành cho Câu 2. Nếu đã kiểm chứng được 1 sản phẩm số cụ thể của TPBank thì thêm vào. Nếu xác nhận phòng tuyển liên quan đến **dữ liệu nhân sự**, có thể nối với kinh nghiệm xử lý dữ liệu chức vụ, vai trò, tin tuyển dụng ở SalesSmart.
- Rút ngắn khi cần: thu đoạn dự án AI còn 1 câu.

---

## Câu 0b: Em có kinh nghiệm rồi, sao lại vào chương trình cho người mới? ✅

**HR lo điều gì:** em có thấy mình "thừa trình độ" rồi chán và nghỉ sớm không; có chịu học lại theo quy trình của ngân hàng không.

**Câu trả lời:**
> "Dạ, em làm DE được hơn một năm thật, nhưng dữ liệu em làm là dữ liệu công ty lấy từ các nguồn công khai. Ở ngân hàng thì khác hẳn: là dữ liệu khách hàng, giao dịch, có nghiệp vụ riêng, rồi yêu cầu về bảo mật với độ chính xác cũng cao hơn nhiều. Mấy cái đó em chưa làm bao giờ.
>
> Nên em nghĩ vào chương trình này để học lại từ đầu theo cách làm của ngân hàng là hợp lý. Còn những gì em đã biết như Airflow, Spark thì sẽ giúp em theo kịp nhanh hơn thôi ạ."

**Ghi chú:**
- ❌ Tránh: chê công ty cũ hay outsource; "với kinh nghiệm của em thì chương trình này sẽ dễ"; "em chưa tìm được vị trí chính thức".
- Nếu bị hỏi "Amira là công ty gì?": "Là công ty phần mềm, vừa làm outsource vừa có sản phẩm riêng. Em làm DE cho SalesSmart, là sản phẩm của công ty."
- Câu hỏi tiếp theo có thể gặp, **"Học lại cái đã biết có chán không?"**: "Cùng là pipeline ETL nhưng ở ngân hàng yêu cầu đối soát và bảo mật cao hơn nhiều, nên em coi đó là dịp hệ thống lại kiến thức theo chuẩn ngân hàng."

---

## Câu 1: Vì sao em nghỉ ở công ty cũ?

**HR muốn biết gì:** HR không chỉ hỏi lý do nghỉ. Họ quan sát:
- em nhìn nhận công ty cũ như thế nào,
- em xử lý sự bất mãn ra sao,
- em có thích nghi được với môi trường mới không.

**Khung trả lời:** không chê công ty cũ → nói điểm tốt trước → giải thích lý do nghỉ một cách khách quan, tập trung vào điều *không còn phù hợp với định hướng của mình* → chốt bằng điều mình đang tìm kiếm ở công việc mới.

**Câu trả lời:**
> "Em rất biết ơn Amira. Em vào từ khi còn là thực tập sinh và được tham gia trực tiếp vào hệ thống dữ liệu thật, từ crawl, chuẩn hoá đến kiểm tra chất lượng dữ liệu. Em học được nhiều nhất là nhờ các anh đi trước hướng dẫn.
>
> Từ tháng 6, công ty ưu tiên sản phẩm AI nên công việc của em chuyển sang AI và backend. Đây là hướng đi hợp lý của công ty, và em vẫn hoàn thành phần việc được giao. Nhưng qua giai đoạn đó, em xác định rõ hơn là mình muốn đi sâu vào data engineering.
>
> Vì vậy em tìm một môi trường DE chuyên sâu, có quy trình bài bản và có người hướng dẫn để phát triển lâu dài."

**Ghi chú:**
- Gọi việc công ty đổi hướng là "hợp lý" cho thấy mình nhìn nhận khách quan. Ý "vẫn hoàn thành việc được giao" cho thấy mình thích nghi được.
- Trong form mình ghi lý do nghỉ là "môi trường mới, thử thách hơn". Khi nói, hiểu đó là *thử thách về chuyên môn DE*.
- ❌ Tránh nhắc đến lương, sếp, "công ty không có việc cho em".
- **Nếu HR hỏi vì sao lương giảm (15.5tr → 14.7tr):** chênh lệch là do thuế. Trả lời 1–2 câu rồi dừng, ví dụ: "Dạ thực ra lương của em không giảm, 15.5 triệu là trước thuế, còn 14.7 triệu là thực nhận sau thuế ạ." ⚠️ Chỉ nói "không giảm" nếu lương gross thật sự không đổi. Kiểm tra lại form đã nộp ghi con số nào.

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
> - Em **xây dựng và vận hành** các luồng ETL đưa dữ liệu từ nhiều nguồn vào lakehouse rồi vào Postgres/Elasticsearch, chạy bằng Airflow và Spark. Phần em làm nhiều là **chuẩn hoá và khử trùng lặp dữ liệu**.
> - Em làm **kiểm tra chất lượng dữ liệu** và theo dõi qua dashboard trên Trino.
> - Em có tham gia **tối ưu hiệu năng**: chuyển các bước chuẩn hoá dùng nhiều Python UDF từ PySpark sang Daft, giảm khoảng 60% thời gian chuẩn hoá của những bước đó.
> - Em đã dùng Spark, Airflow, Hive/Trino trong công việc. Kafka thì em dùng trong dự án cá nhân LearnLake.
>
> Nhưng DE ở ngân hàng khác với những gì em đã làm: dữ liệu là của chính ngân hàng và khách hàng, đòi hỏi độ chính xác tuyệt đối, có đối soát, bảo mật và tuân thủ. Đây là phần em chưa có và muốn được học bài bản.
>
> Về TPBank, em biết ngân hàng định hướng là ngân hàng số, *[điền ví dụ cụ thể]*. Với ngân hàng số thì dữ liệu là hạ tầng cốt lõi. Chương trình Fresh DE lại có đào tạo từ nền tảng, có mentor và có lộ trình lên DE chính thức, đúng với mong muốn phát triển lâu dài của em."

**Ghi chú:**
- 📌 **Cần làm:** lên website TPBank, lấy 1–2 sản phẩm hoặc giải thưởng về ngân hàng số để điền vào chỗ *[điền ví dụ cụ thể]*. *(chưa kiểm chứng, phải tự kiểm tra)*
- Không nói lại nguyên văn phần giới thiệu. Ở câu này đi sâu hơn vào việc khớp JD.

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
> Hai là kiến thức nghiệp vụ ngân hàng của em còn mỏng. Đây là phần em mong được hỗ trợ nhiều nhất qua chương trình đào tạo và mentor. Song song đó em đang tự học thêm về mô hình hoá kho dữ liệu."

**Ghi chú:**
- Điểm yếu thứ nhất **trùng với điều đã ghi trong form**, giữ được tính nhất quán.
- Điểm yếu thứ hai trả lời đúng ý "cần nguồn lực gì", và nguồn lực đó chính là thứ chương trình cung cấp.
- Chỉ nhắc đến Kimball nếu thực sự đang đọc (xem [[03-Data-Warehousing/Kimball - Data Warehouse Toolkit]]).

---

## Câu hỏi đào sâu sau phần giới thiệu

### Về dự án SalesSmart
- **"Pipeline đi qua những bước nào?"** Thu thập (Airflow crawl ~60 nguồn) → nạp thô vào Delta Lake trên MinIO → Spark/Daft chuẩn hoá → hợp nhất theo mã số doanh nghiệp → upsert vào Postgres + Elasticsearch.
- **"Làm sao biết 2 bản ghi là cùng một công ty?"** Hợp nhất chủ yếu theo `corporate_number`. Nguồn nào không có mã thì chuẩn hoá tên, địa chỉ, số điện thoại để đối chiếu.
- **"Chạy lại có bị trùng không?"** Không. Mỗi batch có `_ingest_id`; chạy lại thì xoá dữ liệu của batch đó rồi MERGE theo khoá chính (idempotent). Giá trị mới khác null thì ghi đè, null thì giữ giá trị cũ.
- **"Kiểm tra chất lượng thế nào?"** Dashboard Trino/Hive theo dõi tỉ lệ ghép được mã doanh nghiệp giữa bảng thô và bảng đã chuẩn hoá. Tỉ lệ tụt bất thường thì có thể nguồn đã đổi cấu trúc hoặc bước chuẩn hoá bị lỗi.
- **"Hạ tầng?"** K8s, Airflow mỗi task 1 pod, Spark chạy bằng pod executor, deploy qua ArgoCD/Helm. Nếu phần này do team khác cấu hình thì nói rõ.

### Về Daft (dùng cho câu "khó khăn / tối ưu")
- **Vì sao Spark chậm?** Python UDF và hàm pandas buộc dữ liệu phải chuyển qua lại giữa JVM và tiến trình Python (serialize/deserialize). Spark cũng không tối ưu được logic bên trong UDF.
- **Vì sao Daft nhanh hơn?** Engine viết bằng Rust, chạy UDF Python trực tiếp, không qua JVM. Logic UDF giữ gần như nguyên.
- **Sao không đổi hết?** Chỉ các bước dùng nhiều UDF mới bị nghẽn. Merge/upsert và kiểm tra chất lượng dùng hàm có sẵn của Spark vẫn chạy tốt, hệ thống xung quanh cũng gắn với Spark. Đổi hết thì tốn công và rủi ro mà không được lợi bao nhiêu.
- **Sao không dùng pandas UDF (Arrow) hoặc viết lại bằng hàm có sẵn của Spark?** Nói thật đã thử hay chưa. Nếu chưa: "Lúc đó em được định hướng thử Daft vì giữ được phần lớn logic UDF. Nếu làm lại em sẽ so sánh cả các phương án đó."
- ⚠️ Daft hiện chạy trong **1 pod với Ray local**, không phải cụm Ray. Nói đúng nếu bị hỏi.

### Về dự án AI
- **"Embedding là gì, em làm thế nào?"** Biến văn bản thành dãy số, văn bản cùng nghĩa thì có dãy số gần nhau. Chuyển nội dung bằng sáng chế thành embedding và lưu vào Azure AI Search. Yêu cầu của người dùng cũng được chuyển thành embedding để tìm các bằng sáng chế gần nghĩa nhất.
- **"Làm AI rồi sao lại muốn quay về DE?"** "Làm AI em mới thấy kết quả phụ thuộc rất nhiều vào dữ liệu. Phần embedding với tìm kiếm cũng là làm với dữ liệu. Em thích phần làm cho dữ liệu đúng và sạch hơn, nên em muốn đi sâu vào DE ạ."

### Điểm rủi ro trong CV (phải nói đúng phạm vi)
- *"Built and maintained ETL pipelines"*: nói theo việc mình đã làm, không nhận là xây toàn bộ hệ thống.
- *"Refactored 60+ Airflow DAGs"*: 📌 cần tự xác định đã làm bao nhiêu DAG, phần nào. Nếu không chắc thì sửa CV.
- *"Rust (PyO3 bindings)"*: tự viết hay chỉ tích hợp code có sẵn? Nói đúng.
- *"2TB+"*, *"Kubernetes"*: chỉ trả lời phần mình biết.
- *Global ICT program*: có thể bị hỏi về tiếng Anh. Nên chuẩn bị bản giới thiệu ngắn bằng tiếng Anh.

---

## Các câu hỏi khác hay gặp (chưa chốt)

- Em biết gì về TPBank? *(cần tự tra website, chỉ dùng thông tin đã kiểm chứng)*
- Mức lương mong muốn? *(khớp với form: 15tr, thấp nhất 13tr; không tự nói ra mức thấp nhất)*
- Kể về một khó khăn đã vượt qua (STAR) → dùng chuyện Daft. ⚠️ Xác nhận là "được giao" hay "tự đề xuất".
- Kể về một lần mắc lỗi → 📌 cần một câu chuyện thật.
- Làm việc nhóm, bất đồng; làm việc dưới áp lực.
- Giải thích dự án cho người không chuyên: "giống một danh bạ doanh nghiệp thông minh cho đội sales".
- Sẵn sàng OT / trực sự cố? Đang ứng tuyển nơi khác? Ngày bắt đầu (01/11/2026)?
- Câu hỏi ngược: xem [[On-thi-TPBank-Vong2]].

---

## Lưu ý chung

- **Không nói quá:** CV viết "Built…", "Refactored 60+ DAGs…", "Cut… by ~60%". Khi bị hỏi sâu, nói rõ phạm vi: *"Phần này em làm cùng team, em phụ trách phần…"*.
- Không dùng lại câu C4 trong note VNPT-IT (*"không ai giao, em tự phát hiện"*), vì không đúng sự thật.
- Các thông tin trong form TPBank cần nói khớp: lương mong muốn 15tr (thấp nhất 13tr), ngày bắt đầu 01/11/2026, kế hoạch "ổn định, phát triển chuyên môn".
- Câu hỏi ngược cho nhà tuyển dụng, phân tích JD và hợp đồng: xem [[On-thi-TPBank-Vong2]].

---

## Nhật ký ôn

### 2026-10-05
**Đã ôn / đã làm:**
- Chốt **Câu 0 (giới thiệu bản thân)** sau nhiều lần sửa: nói tự nhiên, không bay bổng; chỉ nói việc mình làm; bỏ chuyện Daft (để dành cho câu khác); thêm dự án AI (embedding + prompt + backend); kết bằng 1–2 câu về TPBank.
- Chốt **Câu 0b (có kinh nghiệm sao vào Fresh)**: lấy khác biệt về dữ liệu và nghiệp vụ ngân hàng làm lý do, không dựa vào "outsource và in-house".
- Đối chiếu câu trả lời với **JD** và **CV** (`CV/main.tex`): 2 giai đoạn ở Amira, các điểm rủi ro trong CV.
- Chuẩn bị **câu hỏi đào sâu** về SalesSmart, Daft (JVM ↔ Python UDF), embedding, lý do quay lại DE.
- Sửa các thông tin sai: Amira không phải startup; SalesSmart là sản phẩm của công ty; lương 14.7tr do thuế (không phải 14tr); Daft chỉ ở một số bước nhưng con số 60% là thật.

**Còn phải làm:**
- [ ] Tra website TPBank, lấy 1–2 thông tin đã kiểm chứng (Câu 0, Câu 2, câu "biết gì về TPBank").
- [ ] Xác nhận phòng ban tuyển (có liên quan đến dữ liệu nhân sự không).
- [ ] Kiểm tra form đã nộp ghi lương cuối ở Amira là bao nhiêu.
- [ ] Xác định phạm vi thật của "Refactored 60+ DAGs" và "Rust/PyO3"; sửa CV nếu cần.
- [ ] Chuẩn bị 1 câu chuyện lỗi thật và 1 ví dụ phát hiện lỗi dữ liệu.
- [ ] Viết bản giới thiệu ngắn bằng tiếng Anh.
- [ ] Tập nói Câu 0 → 0b → 1 → 2 → 3 → 4, bấm giờ.

## Liên quan

- [[On-thi-TPBank-MCQ]]: ôn phần kỹ thuật (vòng test).
- [[On-thi-TPBank-Vong2]]: tổng quan vòng 2, JD, hợp đồng, câu hỏi ngược.
