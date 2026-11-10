# Nhật ký tương tác AI — Tự phản ánh trong quá trình làm Lab 02

Trong quá trình làm Lab 02, tôi sử dụng AI như một người đồng hành tư duy, không phải một người thay thế quyết định. Cách tôi làm là: AI cung cấp ý tưởng và định hình, còn tôi là người đánh giá, sửa lại, kiểm chứng và đưa ra quyết định cuối cùng

## 1. AI đã giúp tôi điều gì
- Làm rõ bài toán và phạm vi: Khi tôi chưa chắc định hình vấn đề nào phù hợp, AI giúp tôi tóm tắt bối cảnh kinh doanh và gợi ý các yếu tố cần có: actor, pain point, quy trình hiện tại và nơi AI có thể can thiệp.
- Chuyển ý tưởng thành cấu trúc: Tôi đã dùng AI để biến những suy nghĩ rời rạc thành "Quick Problem Card" rõ ràng hơn, gồm: vấn đề, người bị ảnh hưởng, workflow hiện tại, bottleneck, KPI và AI fit.
- Sửa câu chữ và làm rõ logic: Sau khi tôi có ý tưởng, AI giúp viết lại các đoạn văn cho mạch lạc, dễ hiểu hơn, đặc biệt là phần mô tả future-state flow, Human-in-the-loop và fallback.
- Gợi ý prompt và template: Khi cần tạo prompt prototype, AI hỗ trợ tôi soạn khung prompt có vai trò, yêu cầu đầu vào, ràng buộc, và dạng đầu ra mong muốn. Điều này giúp tôi tiết kiệm thời gian và tránh bỏ sót thông tin quan trọng.
- Hỗ trợ rà soát sơ bộ: AI cũng giúp tôi phát hiện những điểm còn thiếu trong báo cáo như thiếu dữ liệu, thiếu metric, hoặc logic chưa chặt chẽ.

## 2. AI trả lời sai / hallucination ở đâu
- Tạo KPI không có căn cứ: AI thường gợi ý các con số như "giảm 10% hủy chuyến", "tăng 5% ghép chuyến thành công" mà không dựa trên dữ liệu hoặc bối cảnh thực tế. Đây là kiểu hallucination rất rõ: AI làm cho con số nghe có vẻ hợp lý nhưng thiếu nền tảng.
- Nói quá chắc về AI fit: Ban đầu, tôi yêu cầu AI đề xuất mô hình AI cho toàn bộ quy trình, và nó thường trả lời quá rộng, ví dụ: "AI có thể xử lý mọi bước" mà không phân biệt đâu là bước nên dùng rule-based, đâu là bước phù hợp cho LLM, đâu là nơi cần human review.
- Sinh workflow quá trừu tượng: Khi tôi hỏi AI mô tả quy trình tương lai, nó có xu hướng viết chung chung, không đi vào chi tiết thực tế như đầu vào dữ liệu, trigger, exception handling, và fallback khi AI thất bại.

## 3. Tôi đã sửa prompt và đặt ranh giới như thế nào
- Thêm vai trò và mục tiêu rõ ràng: Ví dụ: "Bạn là một product analyst, không phải người ra quyết định cuối cùng. Chỉ dựa trên thông tin có sẵn, không suy đoán dữ liệu chưa có."
- Yêu cầu output theo template: Tôi bắt buộc AI trả lời theo cấu trúc cụ thể, ví dụ: problem statement, người đau, workflow hiện tại, bottleneck, KPI, AI fit, human-in-the-loop, fallback.
- Cấm suy đoán thiếu dữ kiện: Tôi yêu cầu AI ghi rõ "Nếu không có dữ liệu, hãy nói rõ là chưa đủ căn cứ, không đưa số liệu giả tưởng".
- Dùng AI như người phản biện, không phải người quyết định: Tôi hỏi AI để xem xét các điểm yếu trong logic, nhưng kết luận cuối cùng vẫn do tôi lấy từ dữ liệu và kinh nghiệm thực tế.
- Tách nhiệm giữa brainstorming và validation: AI phù hợp để sinh ý tưởng, nhưng tôi vẫn phải kiểm tra tính khả thi, rủi ro và độ an toàn.

## 4. Kết luận

Qua quá trình làm, tôi thấy AI là một công cụ mạnh, đặc biệt hữu ích để tăng tốc khâu lên ý tưởng, tổ chức thông tin và làm rõ khung suy nghĩ. Vì vậy, trong quy trình làm việc của tôi, AI chỉ đóng vai trò như một trợ lý tư duy, còn người thực hiện mới là người xác nhận tính đúng đắn, độ hợp lệ và khả năng triển khai.