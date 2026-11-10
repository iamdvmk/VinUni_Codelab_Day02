# 02 — Deep-Dive Report: Trợ lý AI giải đáp câu hỏi thường gặp của bệnh nhân (Vinmec)

> Lưu ý: các con số (thời gian/bước, số lượt hỏi/ngày, ngưỡng metric) là **giả định để phân tích**, cần đo thực tế trước khi build prototype.

---

## Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow

Quy trình xử lý câu hỏi của bệnh nhân hiện tại tại bộ phận CSKH Vinmec:

| Bước | Hoạt động                                         | Ai        | Thời gian | Input              | Output                |
| ---- | ------------------------------------------------- | --------- | --------- | ------------------ | --------------------- |
| 1    | Bệnh nhân gửi câu hỏi (điện thoại/kênh online) 🔄 | Bệnh nhân | ~1 phút   | Câu hỏi            | Ticket / cuộc gọi     |
| 2    | Tiếp nhận, đọc câu hỏi                            | CSKH      | ~1 phút   | Ticket             | Hiểu nội dung câu hỏi |
| 3    | Tìm thông tin trong tài liệu hướng dẫn 🔴🔄       | CSKH      | ~5 phút   | Câu hỏi            | Đoạn thông tin đúng   |
| 4    | Soạn câu trả lời và phản hồi bệnh nhân 🔴🔄       | CSKH      | ~3 phút   | Thông tin tìm được | Tin nhắn trả lời      |

- 🔴 Bottleneck: Bước 3 và 4.
- 🔄 Handoff: bệnh nhân → CSKH; CSKH → tài liệu; CSKH → bệnh nhân.
- **Tổng thời gian xử lý thủ công: ~9-10 phút/lượt** (ước tính, cần đo thực tế).

### 3.2. Problem Statement (6-field)

| Field                   | Nội dung                                                                                                                                                                                                                                                                                                  |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Actor / Operator     | Nhân viên Chăm sóc khách hàng (CSKH) của Vinmec. Bệnh nhân là người chờ phản hồi.                                                                                                                                                                                                                         |
| 2. Current Workflow     | Bệnh nhân gửi câu hỏi qua điện thoại hoặc kênh trực tuyến. Nhân viên đọc câu hỏi, tìm thông tin trong tài liệu hướng dẫn, soạn câu trả lời và phản hồi. 4 bước, hoàn toàn thủ công, ~8 phút/lượt.                                                                                                         |
| 3. Bottleneck           | Bước 3 và 4 (~8 phút): tìm thông tin trong tài liệu rải rác và soạn lại câu trả lời cho các câu hỏi lặp đi lặp lại.                                                                                                                                                                                       |
| 4. Business Impact      | Giả định ~200 lượt hỏi/ngày × 10 phút = ~33.3 giờ công/ngày của bộ phận CSKH. Bệnh nhân chờ lâu, dễ bỏ kênh hoặc gọi lại nhiều lần, làm tăng tải cho tổng đài.                                                                                                                                            |
| 5. Success Metric       | 1. Giảm thời gian xử lý câu hỏi phổ biến từ ~10 phút xuống dưới 2 phút (Efficiency).<br>2. Tỉ lệ bản nháp đúng, nhân viên duyệt không cần sửa lớn đạt 90% (Quality).                                                                                                                                      |
| 6. Operational Boundary | **Được phép:** truy xuất tài liệu chính thức của Vinmec; soạn bản nháp trả lời kèm nguồn trích dẫn.<br>**CẤM:** tự động gửi khi chưa có nhân viên duyệt; chẩn đoán bệnh hoặc tư vấn thuốc; báo giá hoặc cam kết thay bệnh viện.<br>**Cần duyệt:** mọi câu trả lời đều qua nhân viên CSKH (bắt buộc HITL). |

### 3.3. Future-State Flow & AI Fit

**AI Fit:** [ ] Rule / State-Machine **[x] LLM Feature (RAG)** [ ] Agentic Loop

Lý do: câu hỏi của bệnh nhân là ngôn ngữ tự nhiên, nhưng quy trình có cấu trúc cố định (hỏi → tìm tài liệu → trả lời), không cần agent tự trị. Nội dung liên quan y tế nên bắt buộc có người duyệt.

**Future-State Flow:**

```
B1  Bệnh nhân gửi câu hỏi (giữ nguyên)
 ↓
B2  🔵 AI: phân loại câu hỏi, tìm đoạn liên quan trong tài liệu chính thức (RAG)
 ↓
B3  🔵 AI: soạn bản nháp trả lời, kèm nguồn trích dẫn và ngày cập nhật tài liệu
 ↓
B4  🟢 CSKH: duyệt / sửa nhanh, rồi gửi cho bệnh nhân (HITL)

↩️ Fallback: nếu không tìm thấy tài liệu phù hợp hoặc AI không đủ tự tin,
   chuyển câu hỏi cho CSKH tự xử lý như quy trình cũ.
```

**Quản lý dữ liệu tri thức (tránh trả lời bằng thông tin cũ):**

- Mỗi đoạn tài liệu lưu kèm metadata: `doc_id`, `version`/`updated_at`, `status` (active/expired).
- Khi tài liệu cập nhật: xóa toàn bộ đoạn có `doc_id` đó rồi nhúng lại bản mới (upsert). Khi truy vấn chỉ lấy `status = active`.
- Mỗi tài liệu có người chịu trách nhiệm cập nhật.
- Bản nháp kèm nguồn và ngày cập nhật để CSKH phát hiện ngay nếu AI lấy từ tài liệu cũ.

---

## Phase 5 — EVALUATE

### AI Readiness Checklist

| #   | Câu hỏi                                   | Đánh giá       | Ghi chú                                                                                                                    |
| --- | ----------------------------------------- | -------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 1   | Có sẵn dữ liệu mẫu/logs sạch để test?     | ⚠️ Giả định có | Giả định Vinmec có tài liệu FAQ chính thức kèm version và người sở hữu, cần xác nhận. Cần thêm log chat cũ để làm bộ test. |
| 2   | Rủi ro khi AI sai có trong tầm kiểm soát? | ✅ Có          | Nhờ HITL (CSKH duyệt mọi câu) và Fallback (chuyển về xử lý thủ công).                                                      |
| 3   | Stakeholders sẵn sàng thay đổi quy trình? | ⚠️ Chưa rõ     | Thay đổi nhỏ: nhân viên chuyển từ soạn sang duyệt. Cần trao đổi với bộ phận CSKH để xác nhận.                              |

### Quyết định

- [x] **GO có điều kiện** (bắt đầu Prototype với scope hẹp)
- [ ] NOT YET
- [ ] NO-GO

### Justification:

- **Kỹ thuật:** LLM + RAG là giải pháp đã quen thuộc cho bài toán hỏi đáp trên tài liệu. Scope hẹp (chỉ nhóm câu hỏi phổ biến) giúp prototype nhỏ và dễ đo.
- **Rủi ro:** thấp nhờ HITL bắt buộc, AI không tự gửi, không chẩn đoán, không báo giá. Fallback đảm bảo quy trình cũ vẫn chạy khi AI không chắc.
- **Chi phí/lợi ích:** bài toán lặp lại nhiều, tiết kiệm tiềm năng ~27 giờ công/ngày (giả định 200 lượt/ngày).
- **Điều kiện trước khi build:**
  1. Xác nhận tài liệu FAQ chính thức có version và người sở hữu. Nếu chưa có, làm sạch và đánh version tài liệu trước (chuyển thành NOT YET).
  2. Đo baseline thực tế: số lượt hỏi/ngày và thời gian xử lý/lượt.
  3. Chốt nhóm câu hỏi phổ biến làm scope đầu tiên.
