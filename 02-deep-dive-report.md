## 3.1. Current-State Workflow Mapping (25 min)
```
┌──────────────────┐    ┌──────────────────┐
│ BƯỚC 1           │    │ BƯỚC 2           │
│ Tiếp nhận xe     │    │ Kiểm tra ngoại   │
│ sau sản xuất     │───→│ quan tổng thể    │
│                  │    │                  │
│ Ai: QC            │    │ Ai: Nhân viên QC │
│ ⏱ 2 phút         │    │ ⏱ 5 phút 🔴     │
│ In: Xe hoàn thiện│    │ In: Xe thực tế   │
│ Out: Xe chờ kiểm │    │ Out: Lỗi phát hiện│
└──────────────────┘    └──────────────────┘
                                  │
                                  ▼ 🔄
                       ┌──────────────────┐
                       │ BƯỚC 3           │
                       │ Chụp ảnh và ghi  │
                       │ nhận lỗi         │
                       │                  │
                       │ Ai: Nhân viên QC │
                       │ ⏱ 3 phút 🔴     │
                       │ In: Lỗi ngoại quan│
                       │ Out: Ảnh + Log lỗi│
                       └──────────────────┘
                                  │
                                  ▼ 🔄
                       ┌──────────────────┐
                       │ BƯỚC 4           │
                       │ Đánh giá mức độ  │
                       │ và phân loại lỗi │
                       │                  │
                       │ Ai: QC / Tổ trưởng│
                       │ ⏱ 4 phút 🔴     │
                       │ In: Ảnh + Log lỗi│
                       │ Out: Kết quả QC  │
                       └──────────────────┘
                                  │
                                  ▼ 🔄
                       ┌──────────────────┐
                       │ BƯỚC 5           │
                       │ Quyết định xử lý │
                       │ và lập báo cáo   │
                       │                  │
                       │ Ai: QC / Sản xuất│
                       │ ⏱ 3 phút         │
                       │ In: Kết quả QC   │
                       │ Out: Đạt / Sửa lỗi│
                       └──────────────────┘
                                  │
                                  ▼
                       ┌──────────────────┐
                       │ BƯỚC 6           │
                       │ Chuyển xe sửa    │
                       │ hoặc xác nhận đạt│
                       │                  │
                       │ Ai: QC / Sản xuất│
                       │ ⏱ 3 phút         │
                       │ In: Quyết định QC│
                       │ Out: Xe đạt chuẩn│
                       └──────────────────┘

🔴 = Bottleneck
🔄 = Handoff giữa người/bộ phận

⏱ TỔNG THỜI GIAN: 20 PHÚT/LƯỢT
```

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên QC (Quality Control), tổ trưởng QC và nhân viên sản xuất tại dây chuyền kiểm định xe sau lắp ráp. |
| **2. Current Workflow** | 	1. Tiếp nhận xe sau sản xuất. 2. Nhân viên QC kiểm tra ngoại quan bằng mắt thường. 3. Chụp ảnh, ghi nhận lỗi thủ công. 4. Tổ trưởng đánh giá và phân loại lỗi. 5. Quyết định đạt hoặc yêu cầu sửa chữa. 6. Chuyển xe sửa lỗi hoặc xác nhận đạt chuẩn. Công cụ: Mắt thường, checklist QC, camera, Excel và hệ thống quản lý chất lượng nội bộ (nếu có). |
| **3. Bottleneck** | Bước 2, 3, 4: Kiểm tra bằng mắt, chụp ảnh và phân loại lỗi thủ công. Phụ thuộc kinh nghiệm nhân viên, dễ bỏ sót vết xước nhỏ, lỗi sơn, móp méo hoặc đánh giá không đồng nhất. Thời gian giả định: 12 phút/20 phút cho 3 bước. |
| **4. Business Impact** | - Tăng thời gian kiểm định mỗi xe, giảm năng suất dây chuyền. 

- Lỗi ngoại quan lọt qua kiểm định gây phát sinh chi phí sửa chữa, bảo hành và khiếu nại khách hàng. 

- Tăng chi phí nhân sự QC và kiểm tra lại. 

- Ảnh hưởng chất lượng đầu ra và tiến độ bàn giao xe. |
| **5. Success Metric** | - Phát hiện đúng ≥95% lỗi ngoại quan thuộc danh mục AI được huấn luyện. 

- Tỷ lệ bỏ sót lỗi nghiêm trọng ≤2%. 

- Giảm thời gian kiểm tra từ 20 phút xuống ≤10 phút/xe. 

- ≥90% lỗi được tự động định vị và phân loại đúng. |
| **6. Operational Boundary** | AI được phép: Phân tích hình ảnh, phát hiện và khoanh vùng lỗi, phân loại lỗi, đề xuất mức độ nghiêm trọng, tự động tạo báo cáo QC. 

 AI tuyệt đối không được: Tự quyết định cho xe xuất xưởng, tự kết luận xe đạt tiêu chuẩn an toàn, tự bỏ qua lỗi nghiêm trọng hoặc tự đóng hồ sơ lỗi. 

 Cần con người duyệt: Xác nhận lỗi, quyết định mức độ nghiêm trọng, yêu cầu sửa chữa và phê duyệt cuối cùng trước khi xe xuất xưởng. |


## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [X] Rule / State-Machine [X] LLM Feature [ ] Agentic Loop.
Lựa chọn: Computer Vision là AI chính; Rule-based kiểm tra tiêu chuẩn; LLM hỗ trợ báo cáo.

```
┌──────────────────────┐
│ BƯỚC 1               │
│ Tiếp nhận xe sau     │
│ sản xuất             │
│                      │
│ Ai: Nhân viên QC     │
│ ⏱ 2 phút             │
│ In: Xe hoàn thiện    │
│ Out: Xe chờ kiểm tra │
└──────────────────────┘
           │
           ▼
┌──────────────────────┐
│ BƯỚC 2               │
│ 🔵 Chụp ảnh tự động  │
│ các góc ngoại quan   │
│                      │
│ Ai: Camera / Hệ thống│
│ ⏱ 1 phút             │
│ In: Hình ảnh xe      │
│ Out: Bộ ảnh chuẩn hóa│
└──────────────────────┘
           │
           ▼
┌──────────────────────┐
│ BƯỚC 3               │
│ 🔵 AI Computer Vision│
│ Phát hiện, khoanh    │
│ vùng và phân loại lỗi│
│                      │
│ Ai: AI System        │
│ ⏱ 2 phút             │
│ In: Bộ ảnh xe        │
│ Out: Danh sách lỗi   │
└──────────────────────┘
           │
           ▼
┌──────────────────────┐
│ BƯỚC 4               │
│ 🔵 Rule + LLM        │
│ Đối chiếu tiêu chuẩn │
│ và tạo báo cáo QC    │
│                      │
│ Ai: AI System        │
│ ⏱ 1 phút             │
│ In: Danh sách lỗi    │
│ Out: Báo cáo đề xuất │
└──────────────────────┘
           │
           ▼
┌──────────────────────┐
│ BƯỚC 5               │
│ 🟢 Nhân viên QC      │
│ Review ảnh và kết quả│
│ AI, xác nhận lỗi     │
│                      │
│ Ai: Nhân viên QC     │
│ ⏱ 2 phút             │
│ In: Báo cáo AI       │
│ Out: Kết quả xác nhận│
└──────────────────────┘
           │
           ▼
┌──────────────────────┐
│ BƯỚC 6               │
│ 🟢 Tổ trưởng QC      │
│ Phê duyệt đạt chuẩn  │
│ hoặc yêu cầu sửa lỗi │
│                      │
│ Ai: Tổ trưởng QC     │
│ ⏱ 2 phút             │
│ In: Kết quả QC       │
│ Out: Quyết định cuối │
└──────────────────────┘
           │
           ▼
     XE ĐẠT / SỬA LỖI

↩️ FALLBACK:
Nếu AI không nhận diện được ảnh, độ tin cậy
dưới ngưỡng (ví dụ 85%) hoặc hệ thống lỗi:
- Chuyển ảnh và kết quả sang hàng chờ review.
- Nhân viên QC kiểm tra thủ công như quy trình cũ.
- Nếu camera lỗi: chụp lại hoặc kiểm tra trực tiếp.
- Không cho phép AI tự động kết luận xe đạt chuẩn.

🔵 = AI Step
🟢 = Human Step (HITL)
↩️ = Fallback

⏱ TỔNG THỜI GIAN MỤC TIÊU: 10 PHÚT/XE
```

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [X] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[X] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
Bài toán có khả năng ứng dụng Computer Vision rõ ràng, tác động trực tiếp đến chất lượng sản phẩm và có KPI định lượng. Rủi ro được kiểm soát thông qua QC phê duyệt và quy trình fallback.
