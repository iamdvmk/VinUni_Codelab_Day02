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
│ QUICK PROBLEM CARD #___                                     │
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
│    2. Nhân viên QC kiểm tra ngoại quan từng vị trí.         │
│    3. Ghi nhận lỗi vào biểu mẫu hoặc hệ thống.              │
│    4. Phân loại mức độ lỗi và xác định phương án xử lý.     │
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
│ QUICK PROBLEM CARD #___                                     │
│                                                             │
│ Bài toán (1 câu): ________________________________________  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? ______________________________________ │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. ___ ──> 2. ___ ──> 3. ___ ──> 4. ___                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? ___ (⏱ ___ phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? _____________________ │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```


```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #___                                     │
│                                                             │
│ Bài toán (1 câu): ________________________________________  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? ______________________________________ │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. ___ ──> 2. ___ ──> 3. ___ ──> 4. ___                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? ___ (⏱ ___ phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? _____________________ │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```