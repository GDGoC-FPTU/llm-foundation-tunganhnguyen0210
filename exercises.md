# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature = 0.0, model luôn cho ra phản hồi gần như giống hệt nhau qua nhiều lần gọi — rất nhất quán nhưng thiếu sáng tạo. Khi tăng dần lên 0.5, 1.0, 1.5, câu trả lời ngày càng đa dạng và đôi khi sáng tạo hơn, nhưng ở mức 1.5 có thể xuất hiện nội dung không liên quan hoặc không chính xác do mô hình "phỏng đoán" quá mức.

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature khoảng 0.2 – 0.3. Chatbot hỗ trợ khách hàng cần trả lời chính xác, nhất quán và đáng tin cậy; mức temperature thấp đảm bảo model bám sát thông tin thực tế, tránh "sáng tạo" ra câu trả lời sai lệch gây mất lòng tin của người dùng.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> Mỗi lần gọi trung bình ~350 token, giả sử input 300 token + output 50 token. Tổng lượt gọi/ngày = 10.000 × 3 = 30.000 lượt.
> - GPT-4o: (300 × 5.0 + 50 × 20.0) / 1.000.000 × 30.000 ≈ **$75/ngày**
> - GPT-4o-mini: (300 × 0.15 + 50 × 0.60) / 1.000.000 × 30.000 ≈ **$2.25/ngày**
>
> GPT-4o đắt hơn GPT-4o-mini khoảng **33 lần** cho workload này.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> **GPT-4o xứng đáng:** Hệ thống phân tích hợp đồng pháp lý hoặc báo cáo tài chính, nơi độ chính xác và khả năng suy luận phức tạp ảnh hưởng trực tiếp đến quyết định kinh doanh — một lỗi nhỏ có thể gây thiệt hại lớn hơn nhiều so với chi phí API.
> **GPT-4o-mini phù hợp hơn:** Chatbot phân loại ticket hỗ trợ khách hàng hoặc tóm tắt email đơn giản, nơi tác vụ lặp lại với độ phức tạp thấp — GPT-4o-mini đủ chính xác và tiết kiệm chi phí hơn 33 lần.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi người dùng phải chờ phản hồi dài — ví dụ chatbot hội thoại, trợ lý viết văn bản, hoặc giải thích code — vì hiển thị từng token ngay khi có giúp giảm cảm giác "đứng màn hình", cải thiện trải nghiệm người dùng đáng kể dù tổng thời gian xử lý không đổi. Ngược lại, non-streaming phù hợp hơn khi ứng dụng cần toàn bộ kết quả trước khi xử lý — chẳng hạn pipeline phân tích dữ liệu hàng loạt, tạo báo cáo tự động, hoặc các tác vụ backend mà output được lưu vào database; trong những trường hợp này, streaming không mang lại lợi ích UX và làm phức tạp logic xử lý hơn.


## Danh Sách Kiểm Tra Nộp Bài
- [x] Tất cả tests pass: `pytest tests/ -v`
- [x] `call_openai` đã triển khai và kiểm thử
- [x] `call_openai_mini` đã triển khai và kiểm thử
- [x] `compare_models` đã triển khai và kiểm thử
- [x] `streaming_chatbot` đã triển khai và kiểm thử
- [x] `retry_with_backoff` đã triển khai và kiểm thử
- [x] `batch_compare` đã triển khai và kiểm thử
- [x] `format_comparison_table` đã triển khai và kiểm thử
- [x] `exercises.md` đã điền đầy đủ
- [x] Sao chép bài làm vào folder `solution` và đặt tên theo quy định 
