# 01 — Problem Scan & Quick Cards (Vin Smart Future)

> **Người thực hiện:** Lê Anh Tuấn
> **Lưu ý về số liệu:** Các con số thời gian/khối lượng dưới đây là **ước tính giả định** dựa trên quan sát và brainstorm cùng AI, chưa được đo thực tế tại Vingroup. Chúng cần được xác minh bằng log vận hành trước khi dùng để ra quyết định đầu tư.

---

# 🔍 Phase 1 — SCAN

Dùng 4 lenses: **Lặp lại / Tốn thời gian / AI có thể tốt hơn / Pain từ người khác**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** | AI có thể tốt hơn | Khách mô tả lỗi xe bằng tiếng Việt tự do qua hotline/app (VD: *"qua gờ giảm tốc kêu cụp cụp bánh trước"*). Cố vấn dịch vụ (CVDV) phải gọi lại hỏi triệu chứng, tự tra tài liệu để gán nhóm lỗi & mức ưu tiên trước khi xếp lịch xưởng. ~20 phút/yêu cầu, ~25% phiếu bị kỹ thuật viên đổi nhóm lỗi khi xe vào xưởng. |
| 2 | **Vinpearl** | Tốn thời gian | Nhân viên Reservation đọc email đặt phòng theo đoàn từ công ty lữ hành (nhiều hạng phòng, ngày lệch nhau, file Excel/PDF đính kèm), kiểm tra quỹ phòng trên PMS rồi soạn phản hồi. ~35 phút/email, đối tác thường phải chờ đến cuối ngày. |
| 3 | **Vinhomes** | Lặp lại | Phân loại và chuyển phản ánh cư dân trên app Vinhomes Resident (mất nước, hỏng đèn, ồn, phí dịch vụ) đến đúng bộ phận. Ticket gửi ca đêm tồn đọng, phản hồi đầu tiên có thể mất ~12 giờ. |
| 4 | **Xanh SM** | Pain từ người khác | Tài xế phàn nàn điểm đón không chính xác vì khách ghi chú địa chỉ tự do (*"cổng sau chung cư, chỗ quán phở"*); tài xế phải gọi điện cho khách, mất 2–4 phút/cuốc và tăng tỉ lệ hủy. |
| 5 | **Vinmec** | Tốn thời gian | Điều dưỡng tổng hợp ghi chú bàn giao ca (handover) từ bệnh án điện tử, y lệnh và ghi chú tay; ~30–45 phút cuối mỗi ca, dễ sót thông tin. |
| 6 | **VinFast** | Lặp lại | Đối chiếu dữ liệu sạc hằng tuần từ trụ sạc đối tác với hóa đơn gửi về tài chính. *(Tự đánh giá: dữ liệu dạng bảng có cấu trúc → Rule/SQL phù hợp hơn LLM.)* |

---

# 🃏 Phase 2 — QUICK-ASSESS

Chọn top 3: **#1 (VinFast chẩn đoán sơ bộ)**, **#2 (Vinpearl group booking)**, **#3 (Vinhomes phản ánh cư dân)**.
Loại #6 vì Rule-based làm tốt hơn; loại #5 vì là mảng y tế, rủi ro cao và em chưa hiểu rõ quy trình điều dưỡng thực tế; #4 đáng làm nhưng phụ thuộc nhiều vào dữ liệu bản đồ/GPS hơn là xử lý ngôn ngữ.

## Card #1 — VinFast: Phân loại sơ bộ lỗi xe từ mô tả tiếng Việt

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                            │
│                                                                  │
│ Bài toán: Khách mô tả lỗi xe bằng tiếng Việt tự do; CVDV phải    │
│ gọi lại hỏi triệu chứng và tự gán nhóm lỗi + mức ưu tiên trước   │
│ khi xếp lịch xưởng.                                              │
│ Công ty thành viên: [x] VinFast                                  │
│                                                                  │
│ Ai đang đau? Cố vấn dịch vụ (quá tải), Kỹ thuật viên (nhận phiếu │
│ sai nhóm lỗi), Khách hàng (chờ lâu, bị hỏi lại nhiều lần)        │
│                                                                  │
│ Workflow thủ công hiện tại:                                      │
│   1. CSKH nhận cuộc gọi/chat, ghi chú tự do vào CRM (3')         │
│   → 2. Chuyển phiếu cho CVDV xưởng (chờ hàng đợi ~45')           │
│   → 3. CVDV gọi lại khách hỏi rõ triệu chứng (8')                │
│   → 4. Tra tài liệu kỹ thuật, gán nhóm lỗi + ưu tiên (6')        │
│   → 5. Tạo phiếu tiếp nhận (RO), đặt lịch / gọi cứu hộ (3')      │
│                                                                  │
│ Bước nào tốn nhất? Bước 3–4 (⏱ 14 phút/lượt)                     │
│ AI hỗ trợ ở bước nào? Bước 3–4: trích xuất triệu chứng có cấu    │
│ trúc, gợi ý top-3 nhóm lỗi (danh mục đóng), sinh câu hỏi làm rõ, │
│ gắn cờ an toàn (phanh, pin cao áp, mất lái...).                  │
│                                                                  │
│ Metric:                                                          │
│  - Thời gian thao tác CVDV: ~20' → ≤ 8'/yêu cầu                  │
│  - Tỉ lệ phiếu bị KTV đổi nhóm lỗi: ~25% → ≤ 10%                 │
│  - Recall cờ an toàn ≥ 99% trên bộ test có nhãn                  │
│                                                                  │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent         │
│ (LLM Feature + lớp Rule bắt từ khóa an toàn)                     │
└──────────────────────────────────────────────────────────────────┘
```

**Tự phản biện (CFO / Trưởng vận hành):**
- *"Sao không dùng form chọn sẵn triệu chứng trong app?"* → Đúng một phần: form giúp được kênh app, nhưng >50% yêu cầu đến qua hotline/Zalo bằng lời nói tự do; khách không biết gọi tên bộ phận.
- *"Nếu AI phân loại sai một ca phanh thành ca ồn nội thất thì sao?"* → Cờ an toàn không phụ thuộc riêng vào LLM: Rule bắt từ khóa **OR** LLM gắn cờ → chuyển thẳng cứu hộ. Thiên về báo động nhầm hơn bỏ sót.
- *"Số 25% đổi nhóm lỗi lấy từ đâu?"* → Hiện là giả định; phải đo lại bằng cách so nhóm lỗi CVDV gán với nhóm lỗi KTV chốt trên ~500 RO lịch sử.

## Card #2 — Vinpearl: Xử lý email đặt phòng theo đoàn

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                            │
│                                                                  │
│ Bài toán: Email group booking từ công ty lữ hành có cấu trúc lộn │
│ xộn; nhân viên phải đọc, bóc tách, kiểm tra quỹ phòng và soạn    │
│ phản hồi thủ công.                                               │
│ Công ty thành viên: [x] Khác: Vinpearl                           │
│                                                                  │
│ Ai đang đau? Nhân viên Reservation khách đoàn; đối tác lữ hành   │
│                                                                  │
│ Workflow thủ công hiện tại:                                      │
│   1. Nhận email + file Excel/PDF                                 │
│   → 2. Đọc & bóc tách: số phòng, hạng, ngày, yêu cầu đặc biệt    │
│   → 3. Tra quỹ phòng trên PMS                                    │
│   → 4. Soạn phản hồi / báo giá                                   │
│   → 5. Nhập booking tạm (tentative)                              │
│                                                                  │
│ Bước nào tốn nhất? Bước 2 & 4 (⏱ ~25 phút/email)                 │
│ AI hỗ trợ ở bước nào? Bước 2 (extract JSON) và bước 4 (draft     │
│ phản hồi). Bước 3 giữ nguyên Rule/API PMS.                       │
│                                                                  │
│ Metric: Thời gian xử lý ~35' → ≤ 10'/email; trích xuất đúng      │
│ ≥ 95% trường; phản hồi đối tác trong 2 giờ làm việc.             │
│                                                                  │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent         │
└──────────────────────────────────────────────────────────────────┘
```

## Card #3 — Vinhomes: Phân loại & điều hướng phản ánh cư dân

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                            │
│                                                                  │
│ Bài toán: Phản ánh cư dân trên app được phân loại và chuyển bộ   │
│ phận thủ công, tồn đọng ngoài giờ hành chính.                    │
│ Công ty thành viên: [x] Vinhomes                                 │
│                                                                  │
│ Ai đang đau? Nhân viên CSKH Ban quản lý; cư dân                  │
│                                                                  │
│ Workflow thủ công hiện tại:                                      │
│   1. Cư dân gửi phản ánh (text + ảnh)                            │
│   → 2. Nhân viên đọc, phân loại                                  │
│   → 3. Tạo ticket, chuyển bộ phận (kỹ thuật/an ninh/vệ sinh/phí) │
│   → 4. Phản hồi cư dân                                           │
│                                                                  │
│ Bước nào tốn nhất? Bước 2–3 (⏱ 3–5 phút/ticket, nhưng tồn đọng   │
│ ca đêm → phản hồi đầu tiên ~12 giờ)                              │
│ AI hỗ trợ ở bước nào? Bước 2: phân loại văn bản tự do.           │
│                                                                  │
│ Metric: 90% ticket được chuyển đúng bộ phận trong < 1 phút;      │
│ thời gian phản hồi đầu tiên 12h → ≤ 1h.                          │
│                                                                  │
│ Quick Architecture: [ ] No AI [x] Rule [x] LLM [ ] Agent         │
│ (Rule trước: bắt cư dân chọn danh mục; LLM chỉ cho ticket "Khác")│
└──────────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Lựa chọn cho Deep-Dive

**Chọn Card #1 — VinFast: Phân loại sơ bộ lỗi xe từ mô tả tiếng Việt.**

| Card | Lý do chọn / loại |
|---|---|
| **#1 VinFast** ✅ | Đầu vào là ngôn ngữ tự nhiên tiếng Việt (đúng thế mạnh LLM, Rule khó phủ). Có sẵn "nhãn tự nhiên" là nhóm lỗi KTV chốt trên RO lịch sử để đánh giá. Rủi ro an toàn kiểm soát được bằng Rule guard + HITL. |
| #2 Vinpearl | Giá trị tốt nhưng phụ thuộc tích hợp PMS; khối lượng email đoàn/ngày thấp hơn, ROI chậm hơn. Ứng viên tốt cho vòng sau. |
| #3 Vinhomes | Phần lớn có thể giải bằng Rule (bắt chọn danh mục trong app). Nên làm Rule trước, đo xem phần "Khác" còn bao nhiêu rồi mới cân nhắc LLM. |
