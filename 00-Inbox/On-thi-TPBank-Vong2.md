# TPBank Fresh DE 2026 – Vòng 2: Phỏng vấn

> **Vòng 1 (đã xong):** kiểm tra IQ + chuyên môn. Ôn tập ở [[On-thi-TPBank-MCQ]].
> **Vòng 2 (sắp tới):** phỏng vấn. Có thể chỉ có **đúng 1 vòng**, nên mọi điều cần làm rõ phải hỏi ngay trong buổi này.
> Câu trả lời cho các câu hỏi HR: xem [[On-thi-TPBank-Interview]].

---

## Checklist trước buổi phỏng vấn

- [ ] Ôn 4 câu HR trong [[On-thi-TPBank-Interview]] (nói ý chính, 60–90 giây mỗi câu).
- [ ] Điền ví dụ cụ thể về TPBank vào câu 2.
- [ ] Chuẩn bị câu giải thích vì sao lương giảm (15.5tr → 14tr).
- [ ] Thuộc 5 câu hỏi ngược theo thứ tự **Việc → Công nghệ → Đánh giá → Hợp đồng → Cam kết**.
- [ ] Mang sổ hoặc chuẩn bị ghi chú câu trả lời ngay sau buổi phỏng vấn.

---

## Fresh DE sẽ làm gì? (đọc từ JD)

**JD nói gì:**

| Mốc | Nội dung |
|---|---|
| 09/2026 | Tuyển dụng & offer (hạn CV 25/09) |
| 11/2026 | Onboard |
| 01/2027 | Bắt đầu **2 tháng đào tạo** Data Engineering |
| 03–05/2027 | **On-job training** (JD ghi 4 tháng) & đánh giá |
| 06/2027 | Hoàn thành chương trình |

Yêu cầu: SQL, cơ sở dữ liệu, tư duy logic. Lợi thế: Python/Java/Scala, Spark, Kafka, Airflow, Hadoop, Cloud.

**Suy ra từ JD** (thông lệ DE ngân hàng, *chưa kiểm chứng riêng với TPBank*):
- **SQL là công cụ chính hằng ngày.** JD đặt SQL và database là yêu cầu bắt buộc, còn Spark/Kafka chỉ là lợi thế.
- **Hadoop nằm trong danh sách** → có thể hệ thống đang chạy on-premise hoặc hybrid. Ngân hàng hay giữ dữ liệu on-prem vì quy định bảo mật.
- **2 tháng đào tạo:** có thể gồm nền tảng DE, stack nội bộ và nghiệp vụ ngân hàng (tài khoản, giao dịch, thẻ, tín dụng).
- **On-job training, việc fresher hay được giao:**
  - viết hoặc sửa job ETL đưa dữ liệu từ hệ thống nguồn (core banking, thẻ, kênh số) vào kho dữ liệu;
  - kiểm tra chất lượng dữ liệu và **đối soát số liệu** giữa nguồn và kho;
  - xây data mart hoặc bảng tổng hợp cho báo cáo (quản trị, rủi ro, báo cáo gửi cơ quan quản lý);
  - vận hành: theo dõi job chạy hằng ngày, xử lý job lỗi, viết tài liệu.
- **Khớp với kinh nghiệm của mình:** Airflow, Spark, data quality, chuẩn hoá dữ liệu dùng được ngay. Phần còn thiếu là **nghiệp vụ ngân hàng, đối soát và data modeling** (Kimball). Nên ôn trước 01/2027.

**JD chưa nói** (phải hỏi):
- Từ 11/2026 đến 01/2027 làm gì? Từ onboard đến hết chương trình khoảng 7–8 tháng, không phải 6.
- Ký loại hợp đồng gì, có BHXH không?
- Sau 06/2027, đạt thì chuyển sang vị trí gì? Có cam kết làm việc tối thiểu hay bồi hoàn chi phí đào tạo không?

---

## Kiến thức nền: các loại hợp đồng

> Để hiểu câu trả lời của HR. Các con số cần kiểm tra lại với văn bản hiện hành.

| | Hợp đồng thực tập | Hợp đồng cộng tác viên | Hợp đồng lao động có thời hạn |
|---|---|---|---|
| Bản chất | Đào tạo / học việc | Hợp đồng dịch vụ (Bộ luật Dân sự) | Quan hệ lao động (Bộ luật Lao động 2019) |
| BHXH, BHYT, BHTN | Thường không | Không bắt buộc | **Bắt buộc** |
| Thuế TNCN | Khấu trừ 10% (từ 2tr/lần) | Khấu trừ 10% (từ 2tr/lần) | Biểu lũy tiến, có giảm trừ gia cảnh |
| Phép năm, trợ cấp | Không | Không | Có |

**Chương trình Fresh DE có thể rơi vào 3 khả năng** (*suy đoán, chưa xác nhận*):
- **A.** Ký hợp đồng lao động ngay khi onboard, có thể kèm thử việc → đủ quyền lợi. Tốt nhất.
- **B.** Ký hợp đồng đào tạo/học nghề 6 tháng, đạt mới ký hợp đồng lao động. Khi đã trực tiếp làm việc thì công ty phải trả lương (BLLĐ Điều 61).
- **C.** Sinh viên năm cuối ký hợp đồng thực tập, người đã tốt nghiệp ký loại khác.

**Quyền lợi chính của hợp đồng lao động:**
- Lương ≥ lương tối thiểu vùng. Làm thêm: 150% ngày thường, 200% ngày nghỉ tuần, 300% lễ Tết.
- Tối đa 8 giờ/ngày, 48 giờ/tuần. Phép năm 12 ngày. 11 ngày lễ Tết.
- Bảo hiểm: mình đóng 10,5%, công ty đóng 21,5%. Được hưởng ốm đau, thai sản, hưu trí, BHYT, trợ cấp thất nghiệp.
- Tự nghỉ việc không cần lý do, chỉ cần báo trước: 30 ngày (hợp đồng 12–36 tháng), 3 ngày làm việc (dưới 12 tháng).
- Thử việc tối đa 60 ngày (trình độ cao đẳng trở lên), lương thử việc ≥ 85%.

⚠️ **Điểm cần soi kỹ:**
- **Cam kết sau đào tạo** (hợp đồng đào tạo, BLLĐ Điều 62): phải làm tối thiểu bao lâu, nghỉ sớm bồi hoàn bao nhiêu.
- **Lương đóng BHXH** có bằng lương thật không. Đóng trên lương thấp thì thai sản, thất nghiệp, lương hưu cũng thấp.

*(Cần kiểm tra lại: lương tối thiểu vùng 2026, mức giảm trừ gia cảnh 2026, biểu thuế TNCN sửa đổi.)*

---

## Câu hỏi ngược (cuối buổi)

> Thời gian hỏi ngược thường còn 5–10 phút, nên chọn 3–5 câu.
> Mẹo nhớ thứ tự: **Việc → Công nghệ → Đánh giá → Hợp đồng → Cam kết**. Hỏi về công việc trước, hợp đồng sau.

**5 câu chính:**

1. **Việc:** "Trong giai đoạn on-job training, em sẽ được phân về team nào và làm những việc gì ạ? Em làm task thật của team hay làm dự án riêng cho tập sự ạ?"
2. **Công nghệ:** "Hệ thống dữ liệu của team hiện đang dùng những công nghệ chính nào ạ? Chạy on-premise hay trên cloud, dùng Spark, Airflow hay công cụ ETL nào ạ?"
3. **Đánh giá** ⭐ quan trọng nhất: "Khi kết thúc chương trình vào 06/2027, tập sự được đánh giá theo tiêu chí nào ạ? Nếu đạt thì em sẽ chuyển sang vị trí và hình thức làm việc như thế nào ạ?"
4. **Hợp đồng:** "Trong thời gian tham gia chương trình, em sẽ ký loại hợp đồng nào ạ: hợp đồng lao động, hợp đồng đào tạo hay hợp đồng thực tập? Chế độ như BHXH được áp dụng thế nào ạ?"
5. **Cam kết:** "Chương trình có yêu cầu cam kết làm việc tối thiểu sau khi hoàn thành không ạ? Nếu có thì là bao lâu ạ?"

**Câu dự phòng:**
- **Khoảng trống:** "Từ khi onboard tháng 11 đến lúc bắt đầu đào tạo tháng 01, em sẽ làm gì ạ?"
- **Mentor:** "Mỗi tập sự có mentor riêng không ạ? Đợt này tuyển khoảng bao nhiêu bạn ạ?"
- **Thách thức:** "Team dữ liệu hiện đang gặp thách thức lớn nhất ở đâu ạ?"
- **Câu chốt:** "Trước khi chương trình bắt đầu, anh/chị khuyên em nên chuẩn bị thêm kiến thức gì ạ?"

**Chọn câu theo người phỏng vấn:**
- **Người kỹ thuật:** hỏi câu 1, 2, 3. Sau đó hỏi: "Phần hợp đồng và chế độ em có thể trao đổi thêm với HR không ạ?"
- **HR:** hỏi câu 3, 4, 5 và câu về mentor.
- **Cả hai:** hỏi câu 1, 2 với người kỹ thuật, câu 3, 4, 5 với HR.

**Lưu ý:**
- Nếu họ đã giải thích điều gì thì không hỏi lại, mà hỏi sâu thêm: "Chị vừa nhắc đến…, em muốn hỏi thêm…".
- Không hỏi con số lương trong phỏng vấn. Để đến lúc nhận offer.

---

## Sau buổi phỏng vấn

Ghi lại ngay câu trả lời nhận được:

| Câu hỏi | Câu trả lời |
|---|---|
| Team và công việc | |
| Stack công nghệ | |
| Tiêu chí đánh giá, chuyển chính thức | |
| Loại hợp đồng, BHXH | |
| Cam kết, bồi hoàn | |
| 11–12/2026 làm gì | |

Khi nhận offer: kiểm tra các điều trên có **ghi bằng văn bản** không. Nói miệng thì không có giá trị ràng buộc.

---

## Liên quan

- [[On-thi-TPBank-MCQ]]: vòng 1, phần kỹ thuật.
- [[On-thi-TPBank-Interview]]: câu trả lời cho câu hỏi HR.
