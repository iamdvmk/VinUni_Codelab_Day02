### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Vinfast | Repetitive + Time-consuming | Kiểm tra chất lượng ngoại quan xe |
| 2 | Vinfast | Repetitive + AI-upgrade | Bảo trì thiết bị sản xuất |
| 3 | Vinfast | Time-consuming + Stakeholder Pain | Quản lý chất lượng nhà cung cấp |
| 4 | Vinfast | Time-consuming + AI-upgrade | Xử lý bảo hành và phân tích lỗi xe |
| 5 | Vinfast | Repetitive + Time-consuming | Tra cứu tài liệu kỹ thuật nội bộ |

---


```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Nhân viên kiểm tra chất lượng ngoại quan  │
│ xe, phát hiện lỗi sơn, vết xước, móp méo và khe hở giữa     │
│ các chi tiết sau lắp ráp.                                   │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên QC(quá tải), Quản lý chất    │
│  lượng(gặp khó khăn), Bộ phận sản xuất(phải sửa chữa)       │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Xe hoàn thành công đoạn sản xuất.                      │
│   2. Nhân viên QC kiểm tra ngoại quan từng vị trí.          │
│   3. Ghi nhận lỗi vào biểu mẫu hoặc hệ thống.               │
│   4. Phân loại mức độ lỗi và xác định phương án xử lý.      │
│   5. Chuyển thông tin cho bộ phận sửa chữa và kiểm tra lại. │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4(⏱ 15-20 phút/xe) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–4: Camera chụp │
│  ảnh → Computer Vision phát hiện lỗi → Phân loại lỗi → Tự   │
│  động tạo báo cáo QC.                                       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian kiểm tra từ 15–20 phút xuống dưới 8        │
│   phút/xe.                                                  │
│   Giảm ít nhất 30% thời gian kiểm tra thủ công.             │
│   Đạt độ chính xác phát hiện lỗi tối thiểu 90% trên bộ      │
│   dữ liệu kiểm thử.                                         │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tự động hóa xử lý yêu cầu bảo hành,       │
│ phân tích nguyên nhân lỗi xe và đề xuất phương án           │
│ khắc phục.                                                  │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên bảo hành, kỹ thuật viên,     │
│ chuyên viên phân tích chất lượng                            │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Tiếp nhận yêu cầu                                      │ 
│   2. Thu thập dữ liệu xe                                    │ 
│   3. Kiểm tra lịch sử lỗi                                   │
│   4. Chẩn đoán và xử lý bảo hành.                           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Phân tích nguyên nhân lỗi  │
│ (⏱ 30 - 60 phút/lượt)                                      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Phân tích mã lỗi,     │
│ lịch sử sửa chữa, đề xuất nguyên nhân và hướng xử lý.       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian chẩn   │
│ đoán từ 30–60 phút xuống dưới 10 phút; tăng độ chính xác    │
│ chẩn đoán lên >90%.                                         │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```


```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Xây dựng trợ lý AI tra cứu tài liệu kỹ    │
│ thuật nội bộ, trả lời câu hỏi dựa trên nguồn tài liệu chính │ 
│ thức.                                                       │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Kỹ sư R&D, kỹ thuật viên, nhân viên    │
│ bảo hành và đội ngũ hỗ trợ kỹ thuật.                        │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│ 1. Xác định thông tin cần tìm                               │
│ 2. Tìm thư mục tài liệu                                     │
│ 3. Mở và đọc tài liệu                                       │
│ 4. Tổng hợp câu trả lời.                                    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Tìm kiếm và đối chiếu      │
│ tài liệu (⏱ 15 - 30 phút/lượt)                              │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tìm kiếm ngữ nghĩa,   │ 
│ hỏi đáp tài liệu (RAG),trích dẫn nguồn và số trang liên     │
│ quan.                                                       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian tra    │
│ cứu từ 15–30 phút xuống dưới 2 phút; tỷ lệ câu trả lời      │
│ có nguồn dẫn >95%.                                          │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```