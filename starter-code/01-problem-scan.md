# Phase 1 — SCAN

| #   | Subsidiary (VinFast/Xanh SM...) | Lens                              | Mô tả ngắn bài toán                                                                                                                                                                    |
| --- | ------------------------------- | --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Vinmec                          | Repetitive + Time-consuming       | Patient Registration: Nhân viên phải nhập và xác minh thủ công thông tin bệnh nhân từ giấy tờ, hồ sơ và biểu mẫu, gây tốn thời gian và dễ phát sinh sai sót dữ liệu.                   |
| 2   | Vinmec                          | Repetitive + AI-upgrade           | Customer Support: Nhân viên CSKH phải liên tục trả lời các câu hỏi lặp lại về lịch khám, thủ tục, chi phí và hướng dẫn trước xét nghiệm.                                               |
| 3   | Vinmec                          | Time-consuming                    | Medical Documentation: Bác sĩ mất thời gian ghi chép, nhập liệu và hoàn thiện hồ sơ bệnh án sau mỗi lượt khám.                                                                         |
| 4   | Vinmec                          | Stakeholder Pain + Time-consuming | Patient Flow Management: Bệnh nhân phải chờ đợi và di chuyển giữa nhiều khoa, phòng xét nghiệm và khu vực chẩn đoán do việc điều phối chưa tối ưu.                                     |
| 5   | Vinmec                          | AI-upgrade + Stakeholder Pain     | Appointment Scheduling: Bệnh nhân có thể gặp khó khăn khi lựa chọn chuyên khoa, bác sĩ và khung giờ phù hợp; bệnh viện cũng cần tối ưu phân bổ lịch khám và hạn chế tình trạng bỏ hẹn. |

# Phase 2 — QUICK-ASSESS

```
┌───────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                         │
│                                                               │
│ Bài toán (1 câu): Tự động hóa việc giải đáp các câu hỏi       |
|                   thường gặp của bệnh nhân nhằm giảm tải cho  |
|                   bộ phận chăm sóc khách hàng.                │
│ Công ty thành viên: [x] Vinmec                                │
│                                                               │
│ Ai đang đau (Actor)? (1) Nhân viên chăm sóc khách hàng (CSKH).|
|                      (2) Bệnh nhân phải chờ phản hồi.         │
│ Workflow thủ công hiện tại (3-5 bước):                        │
│   1. Bệnh nhân gửi câu hỏi qua điện thoại hoặc kênh trực tuyến|
|   2. Nhân viên tiếp nhận và đọc câu hỏi.                      |
|   3. Nhân viên tìm kiếm thông tin trong tài liệu hướng dẫn.   |
|   4. Nhân viên soạn câu trả lời và phản hồi bệnh nhân.        │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất?                              |
|   Bước 3 và 4: Tìm kiếm thông tin và soạn phản hồi.           |
|   ( ví dụ mất 6-7 phút/lượt)                                  │
|                                                               |
│ AI có thể nhảy vào hỗ trợ ở bước nào?                         |
|   Sử dụng LLM + RAG để tìm thông tin trong tài liệu chính     |
|   thức và tự động tạo bản nháp câu trả lời cho nhân viên      |
|   kiểm duyệt                                                  │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                         |
|   Giảm thời gian xử lý câu hỏi phổ biến từ 6-7 phút xuống     |
|   dưới 1 phút/câu hỏi.                                        │
│                                                               │
│ Quick Architecture:   [x] LLM                                 │
└───────────────────────────────────────────────────────────────┘
```

```
┌───────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                         │
│                                                               │
│ Bài toán (1 câu): Tự động hóa việc ghi chép và hoàn thiện hồ  │
│                   sơ bệnh án sau mỗi lượt khám nhằm giảm      │
│                   thời gian nhập liệu cho bác sĩ.             │
│ Công ty thành viên: [x] Vinmec                                │
│                                                               │
│ Ai đang đau (Actor)?                                          │
│   (1) Bác sĩ.                                                 │
│   (2) Nhân viên quản lý hồ sơ bệnh án.                        │
│                                                               │
│ Workflow thủ công hiện tại (3-5 bước):                        │
│   1. Bác sĩ ghi nhận thông tin trong quá trình khám.          │
│   2. Bác sĩ tổng hợp triệu chứng và kết quả khám.             │
│   3. Bác sĩ nhập thông tin vào hệ thống bệnh án điện tử.      │
│   4. Bác sĩ kiểm tra và hoàn thiện hồ sơ.                     │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất?                              │
│   Bước 2 và 3: Tổng hợp thông tin và nhập liệu.               │
│   (Ví dụ mất 5-10 phút/lượt khám).                            │
│                                                               │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                         │
│   Sử dụng Speech-to-Text kết hợp LLM để chuyển đổi nội dung   │
│   trao đổi trong buổi khám thành bản nháp hồ sơ bệnh án.      │
│   Bác sĩ kiểm tra và xác nhận trước khi lưu chính thức.       │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                         │
│   Giảm thời gian hoàn thiện hồ sơ từ 5-10 phút xuống          │
│   dưới 2 phút/lượt khám.                                      │
│                                                               │
│ Quick Architecture: [x] LLM                                   │
└───────────────────────────────────────────────────────────────┘
```

```
┌───────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                         │
│                                                               │
│ Bài toán (1 câu): Tối ưu hóa việc phân bổ lịch khám nhằm      │
│                   giúp bệnh nhân chọn được lịch phù hợp và    │
│                   giảm tình trạng bỏ hẹn tại bệnh viện.       │
│ Công ty thành viên: [x] Vinmec                                │
│                                                               │
│ Ai đang đau (Actor)?                                          │
│   (1) Bệnh nhân.                                              │
│   (2) Nhân viên điều phối lịch khám.                          │
│   (3) Bác sĩ và bệnh viện.                                    │
│                                                               │
│ Workflow thủ công hiện tại (3-5 bước):                        │
│   1. Bệnh nhân liên hệ bệnh viện để đặt lịch khám.            │
│   2. Nhân viên hỏi nhu cầu và chuyên khoa cần khám.           │
│   3. Nhân viên kiểm tra lịch trống của bác sĩ.                │
│   4. Nhân viên đề xuất khung giờ phù hợp.                     │
│   5. Nhân viên xác nhận và cập nhật lịch hẹn.                 │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất?                              │
│   Bước 2, 3 và 4: Xác định chuyên khoa và
    tìm khung giờ phù hợp                                       │
│   (Ví dụ mất 5-8 phút/lượt đặt lịch).                         │
│                                                               │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                         │
│   Sử dụng LLM kết hợp hệ thống recommendation để phân tích    │
│   nhu cầu bệnh nhân, gợi ý chuyên khoa, bác sĩ và khung giờ   │
│   phù hợp dựa trên lịch trống thực tế.                        │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                         │
│   Giảm thời gian đặt lịch từ 5-8 phút xuống dưới 2 phút;      │
│   giảm tỷ lệ bệnh nhân bỏ hẹn ít nhất 15%.                    │
│                                                               │
│ Quick Architecture: [x] LLM  [x] RAG                          │
└───────────────────────────────────────────────────────────────┘
```
