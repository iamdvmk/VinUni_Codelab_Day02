# Báo cáo phân tích sâu — Điều phối chuyến Xanh SM

## 1. Phạm vi và bài toán được chọn

**Bài toán:** Hỗ trợ ghép tài xế với khách hàng và phân bổ chuyến theo vị trí, trạng thái sẵn sàng và nhu cầu khu vực.

Báo cáo này phát triển từ Quick Problem Card #1 trong worksheet. Khoảng thời gian **1–3 phút/lượt** và mục tiêu **giảm 10% tỷ lệ hủy, tăng 5% tỷ lệ ghép thành công** là ước tính/giả thuyết ban đầu do worksheet nêu; chưa có log vận hành xác nhận. Không xem các con số này là baseline hay kết quả đã đạt.

## 2. Problem Statement (6-field)

| Trường | Nội dung |
|---|---|
| **1. Actor / Operator** | Khách hàng đặt chuyến; tài xế đang trực và có thể nhận chuyến; hệ thống điều vận; nhân viên điều phối xử lý ngoại lệ khi không ghép được chuyến. |
| **2. Current Workflow** | (1) Khách hàng đặt xe trên ứng dụng. (2) Hệ thống nhận vị trí đón và thông tin chuyến. (3) Hệ thống tìm tài xế sẵn sàng trong vùng phù hợp. (4) Hệ thống tính khoảng cách/độ phù hợp và gửi đề nghị nhận chuyến. (5) Tài xế chấp nhận hoặc từ chối/hết thời gian; nếu thất bại, hệ thống thử tài xế khác hoặc chuyển ngoại lệ cho điều phối viên. |
| **3. Bottleneck** | Theo quick card, khâu tìm và ghép tài xế (bước 3–4) mất khoảng **1–3 phút/lượt**. Đây là ước tính chưa đo bằng timestamp. Các nguyên nhân cần kiểm tra gồm dữ liệu vị trí không mới/chính xác, tài xế không còn sẵn sàng, từ chối hoặc không phản hồi, và nhu cầu tập trung theo khu vực. |
| **4. Business Impact** | Ghép chậm có thể kéo dài thời gian chờ của khách, tăng nguy cơ hủy; tài xế có thể nhận chuyến không thuận tiện hoặc di chuyển xa. Hiện chưa có dữ liệu để quy đổi tác động thành chi phí/doanh thu. Cần đo thời gian ghép, thời gian chờ đón, tỷ lệ hủy và quãng đường chạy rỗng. |
| **5. Success Metric** | **Mục tiêu thử nghiệm cần xác nhận sau khi lập baseline:** giảm 10% tương đối tỷ lệ hủy và tăng 5% tương đối tỷ lệ ghép thành công so với nhóm/khung thời gian đối chứng tương đương. Định nghĩa trước phép đo: tỷ lệ hủy = chuyến bị hủy / tổng yêu cầu hợp lệ; tỷ lệ ghép thành công = yêu cầu có tài xế nhận trong SLA / tổng yêu cầu hợp lệ. Theo dõi thêm p50/p90 thời gian ghép, tỷ lệ tài xế từ chối và quãng đường chạy rỗng để tránh cải thiện một chỉ số bằng cách làm xấu chỉ số khác. |
| **6. Operational Boundary** | Chỉ dùng dữ liệu vận hành được cấp quyền (vị trí, trạng thái sẵn sàng, loại dịch vụ và thông tin chuyến) để lọc/xếp hạng theo quy tắc đã phê duyệt. Không dùng thuộc tính nhạy cảm không liên quan, thay đổi giá/điều kiện chuyến, hoặc coi nội dung LLM là lệnh điều xe. Hệ thống gửi đề nghị theo chính sách hiện hành; nhân viên điều phối xử lý ngoại lệ. Nếu dữ liệu thiết yếu thiếu/cũ, dịch vụ lỗi hoặc không có ứng viên hợp lệ, không suy đoán và chuyển về quy trình hiện hành. |

## 3. Current-State Workflow

```text
Khách đặt chuyến
      |
      | 🔄 Bàn giao: ứng dụng khách -> hệ thống điều vận
      v
Hệ thống nhận vị trí đón và thông tin chuyến
      |
      v
Tìm tài xế đang sẵn sàng trong vùng phù hợp
      |
      v
🔴 Xếp hạng theo khoảng cách/độ phù hợp
   Bước tìm và ghép: ước tính 1–3 phút/lượt (cần đo)
      |
      | 🔄 Bàn giao: hệ thống điều vận -> ứng dụng tài xế
      v
Gửi đề nghị nhận chuyến
      |
      v
Tài xế chấp nhận?
   /                  \
 Có                    Từ chối / quá thời gian
 |                             |
 v                             v
Xác nhận chuyến          Thử ứng viên tiếp theo
                                |
                                +-- vẫn thất bại --> Điều phối viên xử lý
```

### Cách lập baseline

- Ghi timestamp tại lúc đặt chuyến, bắt đầu tìm, gửi từng đề nghị, nhận phản hồi, xác nhận hoặc hủy.
- Tách thời gian xử lý của hệ thống khỏi thời gian chờ tài xế phản hồi.
- Ghi nhận kết quả và lý do từ chối/hủy theo danh mục thống nhất; không suy luận lý do khi không có dữ liệu.
- Phân tích theo giờ cao điểm/thấp điểm, khu vực và loại dịch vụ để so sánh các nhóm tương đồng.

## 4. Future-State Flow & AI Fit

### So sánh phương án

| Phương án | Mức phù hợp | Đánh giá |
|---|---|---|
| **Rule / state machine** | Cao, nên là nền tảng | Dùng để lọc điều kiện bắt buộc, xác định timeout/retry và fallback. Hành vi xác định, kiểm thử được. |
| **Tối ưu hóa / xếp hạng** | Có thể thử nghiệm | Xếp hạng các tài xế đã qua điều kiện hợp lệ theo tiêu chí vận hành được phê duyệt như ETA/khoảng cách. Cần đánh giá tác động, độ công bằng giữa khu vực/nhóm tài xế và ảnh hưởng tới quãng đường chạy rỗng. |
| **LLM feature** | Thấp cho quyết định ghép chuyến | Không nên dùng LLM để tính ETA hoặc tự chọn tài xế từ dữ liệu cấu trúc. Có thể cân nhắc tóm tắt ngoại lệ cho điều phối viên, nhưng đầu ra không được điều khiển dispatch. |
| **Agentic loop** | Chưa cần | Luồng có bước và điều kiện rõ ràng; tự trị làm tăng độ phức tạp nhưng chưa có bằng chứng về giá trị bổ sung. |

### Luồng tương lai đề xuất

1. **Tiếp nhận:** ứng dụng gửi yêu cầu; hệ thống kiểm tra trường bắt buộc và ghi timestamp.
2. **Lọc bằng rule:** loại tài xế không sẵn sàng, ngoài vùng phục vụ hoặc không đáp ứng loại dịch vụ. Nếu thiếu dữ liệu thiết yếu, không suy đoán.
3. **Xếp hạng:** thuật toán điều phối hiện hữu hoặc phiên bản thử nghiệm xếp hạng ứng viên hợp lệ theo tiêu chí đã phê duyệt; lưu vết lý do và kết quả.
4. **Gửi đề nghị:** tiếp tục dùng cơ chế gửi, timeout và retry đã cấu hình; LLM không gửi lệnh và không thay đổi thứ tự ứng viên.
5. **Nhận phản hồi:** nếu tài xế chấp nhận, xác nhận chuyến; nếu từ chối/quá thời gian, thử ứng viên tiếp theo trong giới hạn cấu hình.
6. **🟢 HITL và fallback:** khi hết ứng viên, dữ liệu bất thường hoặc dịch vụ xếp hạng lỗi, đưa yêu cầu cho điều phối viên và quay về quy trình hiện hành. Nhân viên xử lý ngoại lệ.
7. **Đánh giá:** so sánh với baseline/nhóm đối chứng trên các chỉ số đã định nghĩa; theo dõi đồng thời chất lượng dịch vụ và tác động đến tài xế.

## 5. Ranh giới vận hành và kiểm soát rủi ro

- Không thêm tiêu chí xếp hạng hoặc dữ liệu cá nhân mới nếu chưa được chủ sở hữu vận hành và bộ phận quản trị dữ liệu phê duyệt.
- Điểm xếp hạng/ETA là tín hiệu hỗ trợ, không phải cam kết chính xác.
- Lưu phiên bản quy tắc/mô hình, dữ liệu đầu vào cần thiết, ứng viên được chọn và kết quả phản hồi để có thể kiểm tra sai lệch.
- GPS cũ/thiếu, hệ thống lỗi hoặc không có ứng viên hợp lệ: không phát lệnh dựa trên dữ liệu suy đoán; chuyển sang điều phối viên/quy trình hiện hành.
- Thống nhất trước ngưỡng dừng thử nghiệm và người chịu trách nhiệm xử lý sự cố.

## 6. Evaluate — AI Readiness Checklist

| Câu hỏi | Đánh giá hiện tại | Việc cần làm |
|---|---|---|
| Có dữ liệu mẫu/log sạch để kiểm thử chưa? | **Chưa xác nhận.** Worksheet không cung cấp log hoặc định nghĩa trường dữ liệu. | Kiểm kê quyền truy cập và chất lượng GPS, trạng thái tài xế, timestamp, phản hồi và lý do hủy; lập baseline theo từng nhóm điều kiện. |
| Rủi ro khi hệ thống sai có kiểm soát được không? | **Có thể thiết kế kiểm soát nhưng chưa kiểm chứng.** Có thể giới hạn bằng rule, retry/timeout, giám sát, HITL và fallback. | Kiểm thử dữ liệu thiếu/sai, dịch vụ gián đoạn và trường hợp không có tài xế; phê duyệt ngưỡng dừng và quy trình xử lý. |
| Stakeholders sẵn sàng thay đổi quy trình không? | **Chưa xác nhận.** Chưa có phản hồi từ điều phối viên/tài xế hoặc chủ sở hữu vận hành. | Phỏng vấn người dùng, rà soát luồng ngoại lệ, thống nhất phạm vi thử nghiệm và hướng dẫn vận hành. |

### Quyết định đề xuất: **NOT YET**

Chưa nên đưa hệ thống mới vào điều phối chuyến thật vì chưa có baseline, log đã xác nhận hoặc ý kiến của các bên vận hành. Có thể tiếp tục ở quy mô prototype/đánh giá ngoại tuyến: kiểm tra tính đầy đủ của dữ liệu và so sánh cách xếp hạng với quy trình hiện tại mà không ảnh hưởng chuyến thật.

Chỉ cân nhắc pilot có kiểm soát sau khi đã xác lập baseline và định nghĩa metric; xác nhận chất lượng/quyền sử dụng dữ liệu; kiểm thử fallback/ngưỡng dừng; và được vận hành cùng các bên liên quan phê duyệt. Mục tiêu giảm 10% tỷ lệ hủy, tăng 5% tỷ lệ ghép thành công vẫn là giả thuyết cần kiểm chứng, không phải cam kết kết quả.
