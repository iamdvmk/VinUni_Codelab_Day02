# 03 — Nhật ký tương tác AI

## 1. Tôi dùng AI như thế nào

Tôi dùng hai AI với hai vai trò khác nhau:

- **Claude:** giúp tôi hiểu đề bài đang yêu cầu gì, dựng "xương sườn" cho các file `.md`, và rà soát các con số trong bài. Phần đọc repo GitHub và tạo branch thì tôi tự làm, vì project đã ghi chú chi tiết.
- **ChatGPT:** dùng để thảo luận ý tưởng riêng của tôi. Nó trả lời nhiều, có dẫn nguồn và có hình ảnh, rất hợp để mở rộng góc nhìn.

Khi đưa nội dung vào file, tôi copy từng dòng thay vì dán cả khối, để có thể kiểm tra và sửa nội dung ngay trong lúc làm.

## 2. AI giúp được gì

- Claude giúp tôi nắm nhanh yêu cầu của từng file trong bộ nộp bài và đưa ra cấu trúc rõ ràng để tôi điền nội dung của mình vào.
- Với số liệu, Claude hỏi lại khá chặt, ví dụ "số liệu này bạn có đồng tình không, bạn có biết đây là giả định không". Nhờ vậy các con số như số lượt hỏi/ngày hay ngưỡng thành công đều được ghi rõ là **giả định, cần đo thực tế**, thay vì trình bày như dữ liệu thật. Claude cũng trả lời ngắn gọn nên việc làm khá mượt.
- ChatGPT giúp tôi mở rộng ý tưởng và so sánh các hướng bài toán.
- Tôi cũng dùng AI để hỏi thêm cách các dev thực tế xử lý edge case, ví dụ dữ liệu cũ (outdated) trong hệ thống RAG phải xóa hoặc cập nhật ra sao. Câu trả lời giúp tôi viết lại tiêu chí "dữ liệu sạch" trong phần đánh giá sâu hơn: cần tài liệu có version và có người chịu trách nhiệm cập nhật.

## 3. Giới hạn của AI tôi đã gặp

Trong quá trình làm, tôi **không gặp trường hợp AI bịa thông tin (hallucination) rõ ràng**. Nhưng tôi gặp một số giới hạn khác:

- **ChatGPT nói quá dài.** Dù tôi đã yêu cầu ngắn gọn, nó vẫn trả lời một tràng từ trên xuống dưới, kèm nhiều ví dụ và hình ảnh, rất mỏi mắt khi đọc và dễ làm đầy context window.
- **Số liệu do AI tự đặt ra không có nguồn.** Các con số ước tính (thời gian từng bước, số lượt hỏi, ngưỡng %) chỉ là giả định để phân tích, không phải dữ liệu thật.

## 4. Tôi điều chỉnh prompt và ranh giới như thế nào

- Với ChatGPT, chỉ nói "ngắn gọn" là chưa đủ. Tôi phải ghi rõ định dạng và độ dài mong muốn thì mới ra nội dung dễ đọc.
- Với số liệu, tôi chốt nguyên tắc: AI có thể đề xuất, nhưng mọi con số đều ghi là giả định, và tôi tự tính lại khi có dữ liệu thật.
- Tôi tách việc giữa hai AI: ChatGPT cho ý tưởng, Claude cho cấu trúc và số liệu.
- Tôi tự kiểm tra từng dòng trước khi đưa vào file nộp.

## 5. Tôi không nhận gợi ý của AI một cách thụ động

Tôi từng hỏi ChatGPT vì sao nó nghiêng về bài toán tự động trả lời câu hỏi khách hàng thay vì phân đoạn viêm phổi trên X-quang, dù bài toán sau có ý nghĩa thực tiễn lớn hơn. Đại ý nó giải thích rằng, bài toán trả lời khách hàng phổ biến và dễ mở rộng hơn, trong khi bài toán X-quang liên quan trực tiếp đến chẩn đoán nên rủi ro cao hơn. Tôi cân nhắc lập luận này cùng với phạm vi và thời gian của bài, rồi tự quyết định hướng làm.

## 6. Bài học

AI rất hữu ích để dựng cấu trúc, mở rộng ý tưởng và rà soát số liệu, nhưng cần ranh giới rõ ràng: yêu cầu cụ thể về độ dài và định dạng, ghi rõ giả định, và luôn tự kiểm tra trước khi dùng.
