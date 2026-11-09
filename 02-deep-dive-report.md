# 02 — Deep-Dive Report: VinFast Service Triage Assistant

> **Bài toán:** Phân loại sơ bộ lỗi xe từ mô tả tiếng Việt của khách hàng, hỗ trợ Cố vấn dịch vụ (CVDV) tại xưởng dịch vụ VinFast.
> **Phạm vi pilot đề xuất:** 1 xưởng dịch vụ VinFast tại Hà Nội, các dòng xe VF5 / VF e34 / VF8.
> **Lưu ý về số liệu:** Các con số là **ước tính giả định** để scoping, đánh dấu *(giả định)*. Mục 5 nêu rõ cách đo lại baseline thật trước khi build.

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow

```text
 KHÁCH HÀNG            CSKH TỔNG ĐÀI              CỐ VẤN DỊCH VỤ (CVDV) TẠI XƯỞNG                         KỸ THUẬT VIÊN
 ──────────            ─────────────              ────────────────────────────────                        ─────────────
┌────────────┐     ┌──────────────┐  🔄    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  🔄   ┌──────────────┐
│ Gọi hotline│     │ B1. Tiếp nhận│ HAND-  │ B2. Đọc ghi  │    │ B3. Gọi lại  │    │ B4. Tra tài  │    │ B5. Tạo phiếu│ HAND- │ B6. Kiểm tra │
│ / chat app │ ──→ │ ghi chú tự do│ OFF #1 │ chú + lịch sử│ ──→│ khách hỏi rõ │ ──→│ liệu kỹ thuật│ ──→│ tiếp nhận RO │ OFF#2 │ thực tế xe   │
│ mô tả lỗi  │     │ vào CRM      │ ─────→ │ xe (VIN, km) │    │ triệu chứng  │    │ gán nhóm lỗi │    │ đặt lịch /   │ ────→ │ 25% phải đổi │
│            │     │              │ chờ    │              │    │ 🔴           │    │ + ưu tiên 🔴 │    │ gọi cứu hộ   │       │ nhóm lỗi ↩️  │
│            │     │ ⏱ 3'         │ ~45'   │ ⏱ 3'         │    │ ⏱ 8'         │    │ ⏱ 6'         │    │ ⏱ 3'         │       │ rework ~20'  │
└────────────┘     └──────────────┘        └──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘       └──────────────┘
                   In: lời kể khách        In: phiếu CRM       In: ghi chú thiếu   In: triệu chứng     In: nhóm lỗi
                   Out: ghi chú tự do      Out: hiểu sơ bộ     Out: triệu chứng    Out: nhóm lỗi,      Out: RO, lịch hẹn
                                                                     đầy đủ hơn          mức ưu tiên
```

| Bước | Ai | Công cụ | Thời gian thao tác | Vấn đề |
|---|---|---|---:|---|
| B1. Tiếp nhận | CSKH tổng đài | Điện thoại, CRM | 3' | Ghi chú tự do, thiếu thông tin kỹ thuật (khi nào xảy ra, tốc độ, vị trí âm thanh, đèn báo) |
| 🔄 Handoff #1 | CSKH → CVDV | Hàng đợi CRM | *(chờ ~45')* | Ca nguy hiểm (phanh, pin cao áp) nằm chung hàng đợi với ca thường |
| B2. Đọc phiếu | CVDV | CRM, DMS | 3' | |
| **B3. Gọi lại khách** 🔴 | CVDV | Điện thoại | **8'** | ~60% phiếu phải gọi lại *(giả định)*; khách khó chịu vì bị hỏi lại |
| **B4. Gán nhóm lỗi** 🔴 | CVDV | Tài liệu kỹ thuật, TSB | **6'** | Phụ thuộc kinh nghiệm CVDV; CVDV mới hay gán sai |
| B5. Tạo RO, đặt lịch | CVDV | DMS | 3' | |
| 🔄 Handoff #2 | CVDV → KTV | RO | | ~25% RO bị đổi nhóm lỗi → xếp lại khoang/KTV, rework ~20' *(giả định)* |

**⏱ Tổng thời gian thao tác = 23 phút/yêu cầu** (3' CSKH + 20' CVDV), chưa tính ~45' chờ trong hàng đợi và rework tại xưởng.

---

## 3.2. Problem Statement (6-field)

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | **Cố vấn dịch vụ (CVDV)** tại xưởng dịch vụ VinFast. Bên liên quan: CSKH tổng đài (đầu vào), Kỹ thuật viên (nhận RO), khách hàng (chờ phản hồi). |
| **2. Current Workflow** | Khách mô tả lỗi qua hotline/chat → CSKH ghi chú tự do vào CRM → CVDV đọc phiếu, gọi lại khách để hỏi rõ triệu chứng, tra tài liệu kỹ thuật/TSB để gán nhóm lỗi và mức ưu tiên → tạo RO, đặt lịch hoặc gọi cứu hộ. 5 bước thủ công, 2 lần chuyển giao, ~23 phút thao tác/yêu cầu. |
| **3. Bottleneck** | **B3 + B4 (14'/yêu cầu):** chuyển lời kể tiếng Việt tự do, nhiều từ lóng/tượng thanh (*"kêu cụp cụp"*, *"giật giật khi đề pa"*, *"có mùi khét"*) thành triệu chứng có cấu trúc và nhóm lỗi kỹ thuật. Chất lượng phụ thuộc kinh nghiệm cá nhân CVDV. |
| **4. Business Impact** | *(giả định, 1 xưởng)* ~150 yêu cầu/ngày × 20' CVDV = **~50 giờ CVDV/ngày (~6 FTE)**. ~37 RO/ngày bị đổi nhóm lỗi × 20' = **~12 giờ KTV/ngày** lãng phí. Khách chờ 1–2 giờ mới có lịch hẹn → ảnh hưởng CSAT/NPS. **Rủi ro an toàn:** ca nguy hiểm không được ưu tiên vì nằm chung hàng đợi. |
| **5. Success Metric** | **Efficiency:** thời gian thao tác CVDV median 20' → **≤ 8'**/yêu cầu.<br>**Quality:** nhóm lỗi gợi ý top-1 khớp nhóm KTV chốt **≥ 85%**, top-3 **≥ 95%**; tỉ lệ RO bị đổi nhóm 25% → **≤ 10%**.<br>**Safety:** recall cờ an toàn **≥ 99%** trên bộ test 200 ca an toàn có nhãn; **0** trường hợp AI khuyên "có thể tiếp tục lái".<br>**Adoption:** ≥ 70% bản nháp được CVDV chấp nhận (sửa ≤ 1 trường). |
| **6. Operational Boundary** | **AI ĐƯỢC:** đọc mô tả của khách + thông tin xe (dòng xe, km, lịch sử dịch vụ) qua API *read-only*; trích xuất triệu chứng; gợi ý top-3 nhóm lỗi **từ danh mục đóng**; sinh câu hỏi làm rõ; gắn cờ an toàn; soạn nháp tin nhắn cho khách với tiền tố `[DRAFT_ONLY]`.<br>**AI TUYỆT ĐỐI KHÔNG:** đưa ra chẩn đoán cuối cùng hay nói "xe không sao"; khuyên khách tiếp tục lái khi có dấu hiệu an toàn; báo giá, hứa bảo hành/miễn phí; tạo nhóm lỗi ngoài danh mục; tự gửi tin cho khách; tự tạo RO.<br>**CẦN DUYỆT:** mọi tin gửi khách và mọi gán nhóm lỗi đều do CVDV xác nhận (HITL). Ca có cờ an toàn được **Rule** chuyển ngay sang hotline cứu hộ, không chờ CVDV đọc. |

---

## 3.3. AI Fit & Future-State Flow

### So sánh Rule vs LLM vs Agent

| Tiêu chí | Rule / Keyword | **LLM Feature** ✅ | Agentic Loop |
|---|---|---|---|
| Hiểu tiếng Việt tự do, không dấu, từ lóng, tượng thanh | Kém: phải liệt kê hàng nghìn biến thể | Tốt | Tốt |
| Output có cấu trúc (JSON nhóm lỗi, câu hỏi làm rõ) | Chỉ map được từ khóa → nhóm | Tốt (structured output + danh mục đóng) | Tốt |
| Tính tất định, dễ audit | **Rất tốt** | Trung bình (cần eval set + log) | Kém |
| Rủi ro hành động sai | Thấp | Thấp (chỉ draft, có HITL) | **Cao**: có thể tự đặt lịch, tự gửi tin |
| Chi phí | ~0 | Thấp (vài USD–vài chục USD/tháng ở 150 lượt/ngày) | Cao hơn, nhiều lượt gọi |
| Cần thiết? | Đủ cho **cờ an toàn từ khóa** | Đủ cho phân loại + câu hỏi | Không cần: quy trình cố định, không cần tự lập kế hoạch nhiều bước |

**Kết luận AI Fit: `[ ] Rule  [x] LLM Feature  [ ] Agentic Loop`**, cụ thể là **LLM Feature + lớp Rule guard**.
- Rule làm phần cần tất định: bắt từ khóa an toàn (*phanh, mất phanh, khói, cháy, mùi khét, pin nóng, mất lái, đèn đỏ cảnh báo*) và kiểm tra JSON hợp lệ / nhóm lỗi thuộc danh mục.
- LLM làm phần cần hiểu ngôn ngữ: trích xuất triệu chứng, gợi ý nhóm lỗi, sinh câu hỏi làm rõ.
- Không dùng Agent vì không có bước nào cần AI tự quyết định chuỗi hành động; thêm quyền hành động chỉ tăng rủi ro.

### Future-State Flow

```text
┌──────────────┐    ┌───────────────────┐    ┌───────────────────┐    ┌───────────────────┐    ┌──────────────┐
│ B1. CSKH /   │    │ B2. 🔵 RULE GUARD  │    │ B3. 🔵 LLM TRIAGE  │    │ B4. 🟢 CVDV REVIEW │    │ B5. CVDV tạo │
│ App tiếp nhận│ ─→ │ Bắt từ khóa an toàn│ ─→ │ - Trích triệu chứng│ ─→ │ Xem nháp, sửa,     │ ─→ │ RO + gửi tin │
│ lời kể khách │    │ + lấy info xe (API)│    │ - Top-3 nhóm lỗi   │    │ bấm Duyệt          │    │ cho khách    │
│ ⏱ 3'         │    │ ⏱ < 1s            │    │ - Câu hỏi làm rõ   │    │ ⏱ ~4'             │    │ ⏱ ~2'        │
└──────────────┘    └─────────┬─────────┘    │ - Cờ an toàn       │    └───────────────────┘    └──────────────┘
                              │              │ - Nháp [DRAFT_ONLY]│
                              │ có cờ        │ ⏱ ~5s              │
                              ▼ an toàn      └─────────┬─────────┘
                    ┌───────────────────┐              │ JSON lỗi / timeout > 10s /
                    │ 🟢 ESCALATE NGAY   │              │ confidence < 0.6 /
                    │ Hotline cứu hộ gọi │◄─── LLM ────┤ nhóm lỗi ngoài danh mục
                    │ khách trong 5'     │  gắn cờ      ▼
                    │ (không chờ hàng đợi)│      ┌───────────────────┐
                    └───────────────────┘      │ ↩️ FALLBACK        │
                                               │ Phiếu về hàng đợi  │
                                               │ thủ công, gắn nhãn │
                                               │ "AI không chắc";   │
                                               │ CVDV làm như cũ    │
                                               └───────────────────┘
```

**Thời gian thao tác CVDV dự kiến: ~6–8'/yêu cầu** (giảm từ 20').

**Nguyên tắc an toàn:** cờ an toàn = **Rule OR LLM**. Chỉ cần một trong hai gắn cờ là escalate. Hệ thống chấp nhận báo động nhầm để không bỏ sót ca nguy hiểm.

### Output JSON của bước LLM

```json
{
  "draft_tag": "[DRAFT_ONLY]",
  "symptoms": {
    "description_normalized": "Tiếng kêu lạch cạch ở bánh trước khi đi qua gờ giảm tốc",
    "location": "bánh trước",
    "condition": "qua gờ giảm tốc, tốc độ thấp",
    "warning_lights": "không đề cập"
  },
  "fault_group_candidates": [
    {"code": "SUSP", "label": "Hệ thống treo / giảm xóc", "confidence": 0.78},
    {"code": "STEER", "label": "Hệ thống lái / rotuyn", "confidence": 0.14},
    {"code": "BRAKE", "label": "Phanh", "confidence": 0.05}
  ],
  "safety_flag": false,
  "safety_reason": null,
  "clarifying_questions": [
    "Tiếng kêu xuất hiện khi đánh lái hay chỉ khi đi qua gờ?",
    "Xe có bị lệch lái hoặc rung vô lăng không?"
  ],
  "customer_message_draft": "[DRAFT_ONLY] Chào anh/chị, VinFast đã ghi nhận..."
}
```

`fault_group_candidates[].code` bắt buộc thuộc danh mục đóng (VD: `SUSP, STEER, BRAKE, HV_BATT, LV_ELEC, ADAS, HVAC, INFOTAIN, BODY, CHARGING, OTHER`). Mã ngoài danh mục thì đi vào Fallback.

---

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist

| # | Tiêu chí | Đánh giá |
|---|---|---|
| 1 | Có sẵn dữ liệu mẫu / logs sạch để test? | **[~] Một phần.** Về nguyên tắc VinFast có RO lịch sử với nhóm lỗi do KTV chốt, đây là nhãn tự nhiên. Nhưng cần trích xuất, ẩn danh (bỏ SĐT, biển số) và ghép với ghi chú CRM gốc. Chưa có trong tay. |
| 2 | Rủi ro khi AI sai nằm trong tầm kiểm soát? | **[x] Có.** AI chỉ tạo bản nháp, mọi output qua CVDV duyệt. Cờ an toàn có Rule độc lập với LLM. Fallback về quy trình cũ khi AI không chắc. |
| 3 | Stakeholders sẵn sàng thay đổi quy trình? | **[~] Chưa xác minh.** CVDV có động lực (giảm gọi lại), nhưng cần phỏng vấn trưởng xưởng. Rủi ro: CVDV kỳ cựu không tin gợi ý AI. Giảm rủi ro bằng chạy shadow mode trước. |

### Quyết định

**[x] GO (scope hẹp, shadow mode)**
[ ] NOT YET
[ ] NO-GO

### Justification

1. **Bài toán đúng thế mạnh LLM:** đầu vào là tiếng Việt tự do, nhiều từ lóng và tượng thanh. Rule-based thuần không phủ được, nhưng Rule vẫn được giữ cho phần cần tất định (cờ an toàn, validate danh mục).
2. **Rủi ro kiểm soát được:** AI không có quyền hành động (không gửi tin, không tạo RO, không báo giá). Lỗi tệ nhất là gợi ý sai nhóm lỗi, và lỗi này CVDV sửa được, không tệ hơn hiện trạng (đang sai ~25%).
3. **Chi phí thấp, lợi ích rõ:** chi phí gọi LLM ở mức vài USD đến vài chục USD/tháng cho 1 xưởng, so với ~50 giờ CVDV/ngày *(giả định)*. Kể cả khi chỉ đạt một nửa mục tiêu (20' → 14') vẫn tiết kiệm ~15 giờ CVDV/ngày.
4. **Vì sao không phải GO toàn diện:** checklist #1 và #3 mới đạt một phần, và số liệu baseline đang là giả định. Vì vậy GO chỉ áp dụng cho pilot có điều kiện:

| Giai đoạn | Việc cần làm | Điều kiện đi tiếp |
|---|---|---|
| Tuần 0–2 | Trích 500 RO lịch sử + ghi chú CRM, ẩn danh. Đo baseline thật (thời gian CVDV, % đổi nhóm lỗi). Gán nhãn 200 ca an toàn. | Baseline thật xác nhận B3+B4 ≥ 10'/yêu cầu |
| Tuần 3–6 | **Shadow mode** tại 1 xưởng: AI chạy song song, CVDV làm như cũ và chấm điểm gợi ý AI. | Top-1 ≥ 85%, recall an toàn ≥ 99% |
| Tuần 7–10 | Bật HITL thật: CVDV dùng bản nháp AI. | Thời gian CVDV ≤ 8', adoption ≥ 70% |

**Kill criteria:** sau shadow mode, nếu top-1 < 75% hoặc recall an toàn < 99%, dừng LLM và triển khai **Rule-based router + form chọn triệu chứng có cấu trúc** trên app làm giải pháp thay thế.

---

## Phụ lục: Đặc tả sơ đồ `04-workflow-diagram`

Sơ đồ current-state cần thể hiện:
- **4 swimlane:** Khách hàng · CSKH tổng đài · CVDV · Kỹ thuật viên
- **6 bước** B1–B6 như mục 3.1, mỗi bước ghi ⏱ thời gian
- **🔄 Handoff #1** CSKH → CVDV (chờ ~45') và **🔄 Handoff #2** CVDV → KTV
- **🔴 Bottleneck** tại B3 (8') và B4 (6')
- Mũi tên rework ↩️ từ B6 quay về B4 (25% đổi nhóm lỗi)
- Dòng tổng: **Tổng = 23 phút thao tác/yêu cầu** (+ ~45' chờ)
