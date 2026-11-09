┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Phân loại và định tuyến tự động hàng ngàn yêu cầu/│
│ khiếu nại của cư dân tới đúng bộ phận phụ trách.            │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Nhân viên CSKH (quá tải), Cư dân (chờ đợi)     │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi yêu cầu qua App/Hotline/Group cộng đồng     │
│   → 2. Nhân viên CSKH đọc và phân tích ngữ nghĩa thủ công   │
│   → 3. Gắn thẻ loại lỗi và chọn thủ công người phụ trách    │
│   → 4. Chuyển thông tin cho kỹ thuật/an ninh hiện trường    │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 3-5 phút/lượt)               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (Đọc hiểu ngữ nghĩa -> Gắn thẻ tự động -> Định tuyến)       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian xử lý và điều phối từ 3-5 phút ──> dưới 60s. │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Trích xuất ngữ nghĩa)  │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Điều khiển linh hoạt công suất điều hòa, đèn sảnh │
│ theo dữ liệu môi trường thực tế thay vì lịch vận hành cứng. │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Trưởng BQL (áp lực chi phí), Kỹ thuật (vận hành│
│ rập khuôn)                                                  │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Nhận lịch vận hành cố định theo khung giờ mỗi ngày     │
│   → 2. Bật/tắt thiết bị bằng tay hoặc cài đặt timer cứng    │
│   → 3. Đi tuần tra kiểm tra nhiệt độ/độ sáng sảnh thực tế   │
│   → 4. Điều chỉnh lại thủ công nếu có hao phí quá lớn       │
│                                                             │
│ Bước nào tốn nhất? Bước 2 (⏱ Vận hành sai lệch gây lãng phí)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4              │
│ (Đọc cảm biến IoT -> Đếm người -> Ra quyết định tăng/giảm)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm lượng điện năng tiêu thụ khu vực chung từ 10% ──> 20%. │
│                                                             │
│ Quick Architecture: [x] AI Agent (RL điều khiển tự động)    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Phát hiện tự động vi phạm nội khu (đỗ xe, xả rác) │
│ qua hệ thống camera hiện hữu không cần sức người nhìn.      │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Bảo vệ trực camera (dễ bỏ sót vi phạm)         │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. An ninh nhìn màn hình video wall liên tục              │
│   → 2. Bỏ sót vi phạm do nhiều nguyên nhân                  │
│   → 3. Đi tuần tra hiện trường thủ công để rà soát bù       │
│   → 4. Lập biên bản thủ công khi phát hiện sự việc          │
│                                                             │
│ Bước nào tốn nhất? Bước 1-3 (⏱ 30 phút/lượt tuần tra)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-2              │
│ (CV tự động bắt bất thường -> Push cảnh báo tới máy bảo vệ) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Tăng tỷ lệ phát hiện vi phạm thực tế từ <10% ──> trên 70%.  │
│                                                             │
│ Quick Architecture: [x] Computer Vision (Phát hiện hành vi) │
└─────────────────────────────────────────────────────────────┘