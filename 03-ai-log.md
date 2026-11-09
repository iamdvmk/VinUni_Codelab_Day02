# 03 — AI Log & Reflection

> **Người thực hiện:** Lê Anh Tuấn
> **Công cụ AI đã dùng:** Claude Code (Claude Opus) làm thought-partner đọc đề, scoping và soạn nháp báo cáo; Gemini (qua `prompt_prototype.py`) để stress-test ranh giới prompt.
> *Bản nháp được viết dựa trên quá trình làm việc thực tế. Các mục `[Bổ sung]` cần tự điền bằng trải nghiệm cá nhân.*

---

## 1. AI đã giúp gì?

| Việc | AI đóng góp | Giá trị thực |
|---|---|---|
| Đọc hiểu đề | Tóm tắt worksheet, ví dụ mẫu, inspiration kit thành bảng 4 file cần nộp + cách chấm | Tiết kiệm thời gian đọc |
| Đọc autograder | Phát hiện các "bẫy" chấm tự động: `SYSTEM_PROMPT` phải chứa ≥ 2/3 từ khóa (`draft_only`, `5%`, `dispatch_mobile_charger`); output script **không được** chứa chữ "Failed" ở bất kỳ đâu; script phải exit 0 trong 30s | Đây là phần hữu ích nhất. Tự đọc em dễ bỏ qua chi tiết regex "Failed" |
| Chọn bài toán | Đề xuất chọn bài VinFast thay vì Xanh SM để không trùng ví dụ mẫu | Hợp lý |
| Soạn nháp | Soạn khung `01-problem-scan.md` và `02-deep-dive-report.md`: bảng so sánh Rule/LLM/Agent, future flow, JSON output, kill criteria | Khung tốt, nhưng **số liệu phải kiểm lại** (xem mục 2) |

## 2. AI đã sai / có vấn đề ở đâu?

1. **Khẳng định sai là thiếu file:** AI nói repo "chưa có `starter-code/prompt_prototype.py`" trong khi file có sẵn. Nguyên nhân: lệnh liệt kê file của AI chỉ hiện 100 kết quả đầu, toàn là file trong `.venv`, và AI kết luận từ kết quả bị cắt cụt mà không kiểm tra lại. → **Bài học:** AI có thể tự tin kết luận từ dữ liệu không đầy đủ, cần đối chiếu với thực tế.
2. **Thông tin model lỗi thời:** AI khuyên đổi `GEMINI_MODEL` về `gemini-2.5-flash` vì worksheet ghi như vậy, nhưng model này đã đóng. AI bám theo tài liệu, không biết tình trạng thực tế của API.
3. **Hành động vượt quá yêu cầu:** AI tự ý định cài thêm thư viện `matplotlib` để vẽ sơ đồ mà không hỏi trước. Em đã từ chối và quyết định tự vẽ sơ đồ.
4. **Số liệu "trông có vẻ thật" nhưng là giả định:** các con số như *150 yêu cầu/ngày*, *25% RO bị đổi nhóm lỗi*, *8 phút gọi lại khách* do AI ước tính, không có nguồn. Nếu không ghi rõ, người đọc sẽ tưởng là số đo thực tế.

## 3. Em đã sửa / kiểm soát như thế nào?

- Chỉ ra cho AI file `starter-code/prompt_prototype.py` có tồn tại → AI đọc lại và review đúng nội dung file.
- Bỏ qua gợi ý đổi model, giữ cấu hình em đã có API key và chạy được.
- Từ chối lệnh cài thư viện, giới hạn phạm vi: AI viết 3 file `.md`, em tự vẽ `04-workflow-diagram`.
- Yêu cầu đánh dấu mọi con số là *(giả định)* và thêm bước **"Tuần 0–2: đo baseline thật"** vào kế hoạch. Đây cũng là lý do quyết định là **GO có điều kiện (shadow mode)** chứ không phải GO toàn diện.
- `[Bổ sung]` Kết quả chạy `prompt_prototype.py` với Gemini: test nào Passed, model có lần nào bỏ thẻ `[DRAFT_ONLY]` hay gợi ý trạm sạc xa không, em đã chỉnh `SYSTEM_PROMPT` ra sao.

## 4. Phản ánh cá nhân

- AI mạnh nhất ở việc **đọc nhanh và hệ thống hóa** (đề bài, autograder, khung báo cáo), yếu ở việc **biết sự thật hiện tại** (model nào còn hoạt động, quy trình xưởng VinFast thực tế ra sao, số liệu vận hành).
- Ranh giới em đặt cho AI khi làm bài cũng giống Operational Boundary trong báo cáo: AI được soạn nháp, nhưng **con người duyệt** số liệu, quyết định cuối cùng và mọi hành động thay đổi môi trường (cài đặt, commit).
- `[Bổ sung]` Điều em sẽ làm khác lần sau: ...
