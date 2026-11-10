| # | Xanh SM | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Xanh SM | Repetitive | Điều phối chuyến, ghép tài xế với khách và phân bổ xe theo khu vực có nhu cầu cao liên tục trong ngày. |
| 2 | Xanh SM | Time-consuming | Xử lý thủ công các khiếu nại của tài xế, khách hàng. |
| 3 | Xanh SM | AI-upgrade | Hệ thống gợi ý điểm đón và điều hướng chưa tối ưu trong một số tình huống đường đông, điểm đón khó tiếp cận hoặc thay đổi giao thông. |
| 4 | Xanh SM | Stakeholder Pain | Tài xế phải chờ lâu giữa các chuyến hoặc di chuyển không có khách; cần dự báo nhu cầu và tối ưu vị trí chờ.|
| 5 | Xanh SM | Pain từ người khác | Phân tích lý do hủy chuyến của khách hàng |

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): ________________________________________ 
| Điều phối chuyến, ghép tài xế với khách và phân bổ xe theo khu 
|vực có nhu cầu cao liên tục trong ngày.                      |
│ Công ty thành viên: Xanh SM    
│                                                             │
│ Ai đang đau (Actor)? ______________________________________ │
│- Tài xế :  Nhận chuyến ở xa
| -Khách hàng: Đợi lâu
| -Bộ điều hành: xử lý trường hợp thất bại
|                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. ___ ──> 2. ___ ──> 3. ___ ──> 4. ___ 
|    1.Khách hàng đặt xe trên ứng dụng.
|    2.Hệ thống xác định vị trí khách hàng.
|    3.Tìm kiếm tài xế đang sẵn sàng nhận chuyến.
|    4.Tính toán khoảng cách và gửi yêu cầu nhận chuyến.
|    5.Tài xế nhận chuyến hoặc hệ thống tìm tài xế thay thế.  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? ___ (⏱ ___ phút/lượt)  
│ bước 3-4( khoảng 1-3 phút)
│ AI có thể nhảy vào hỗ trợ ở bước nào? _____________________ │
│ Tính điểm phù hợp của tài xế
| Tự ghép cặp tài xế- khách hàng                                                            │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│ -Tỷ lệ huỷ chuyến: Giảm 10%
| -Tỷ lệ ghép chuyến thành công: Tăng 5%                      |         
│ Quick Architecture: Rule,LLM                                │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): ________________________________________
│ Xử lý khiếu nại của khách hàng, tài xế
│ Công ty thành viên:  Xanh SM  
│                                                             │
│ Ai đang đau (Actor)? ______________________________________ │
│- Tài xế : Chờ đợi khi cần giải quyết vấn đề về thu nhập hoặc giao dịch.
|- Nhân viên CSKH: Phải đọc và xử lý nhiều yêu cầu tương tự.
|- Quản lý vận hành: Khó theo dõi lượng yêu cầu tồn đọng.
                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. ___ ──> 2. ___ ──> 3. ___ ──> 4. ___   
|    1.Tài xế gửi yêu cầu qua kênh hỗ trợ.
|    2.Nhân viên tiếp nhận và phân loại khiếu nại.
|    3.Tra cứu thông tin chuyến đi, ví hoặc bảng thu nhập.
|    4.Kiểm tra chính sách và xác minh nguyên nhân.
|    5.Soạn phản hồi và gửi kết quả cho tài xế.           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? ___ (⏱ ___ phút/lượt)
| Bước 2-4( 5-10 phút)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? _____________________ │
|    .LLM tạo bản nháp phản hồi cho nhân viên.
|    .Tự động chuyển trường hợp phức tạp cho nhân viên phụ trách. 
|                                                              │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
|    .Tỷ lệ yêu cầu xử lý tự động: Trên 40%
|    .Thời gian xử lý yêu cầu thông thường: Dưới 1% 
|                                                              │
│ Quick Architecture: Rule, LLM, Agent                         │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): ________________________________________
| Phân tích lý do hủy chuyến của khách hàng                   |
│ Công ty thành viên:  Xanh SM  
│                                                             │
│ Ai đang đau (Actor)? ______________________________________ │
│- Tài xế: Mất thời gian di chuyển đến điểm đón nhưng khách hàng| 
|hủy chuyến.                                                  | 
|- Khách hàng: Không hài lòng khi phải chờ lâu hoặc không tìm |
|được tài xế phù hợp.                                         |  
|- Quản lý vận hành: Khó xác định nguyên nhân chính khiến tỷ lệ| 
|hủy chuyến tăng.                                             |  
|                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. ___ ──> 2. ___ ──> 3. ___ ──> 4. ___   
|   1.Khách hàng đặt chuyến trên ứng dụng.                    |
|   2.Hệ thống ghép tài xế và gửi thông tin chuyến đi.        |
|   3.Khách hàng hoặc tài xế hủy chuyến nếu không tiếp tục thực hiện.
|   4.Hệ thống ghi nhận trạng thái hủy và lý do hủy (nếu có).|
|   5.Nhân viên vận hành tổng hợp dữ liệu, phân tích báo     |
| cáo và tìm nguyên  nhân.                                    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? ___ (⏱ ___ phút/lượt)
| Bước 4-5( 5-15 phút)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? _____________________ │
|    .NLP phân loại lý do hủy từ phản hồi văn bản của khách hàng.
|    .Clustering phát hiện các nhóm nguyên nhân phổ biến.      |
|    .XGBoost dự đoán xác suất hủy chuyến trước khi chuyến bị hủy. 
|                                                              │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
|    .Tỷ lệ hủy chuyến: Giảm 10%
|    .Recall nhóm chuyến có nguy cơ hủy: >80%                  |
|    .Tỷ lệ hủy do thời gian chờ cao: 15%                      |
|                                                              │
│ Quick Architecture: Rule, LLM, Agent                         │
└─────────────────────────────────────────────────────────────┘
```