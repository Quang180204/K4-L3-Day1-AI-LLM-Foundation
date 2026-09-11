# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**
temperature 0.0 : Chắc chắn rồi! Đây là một
temperature 0.5 : Chắc chắn rồi! Đây là một sự
temperature 1.0 : Tuyệt vời! Đây là một sự thật
temperature 1.5 : Một sự thật thú vị về Việt Nam là:

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi tăng temperature từ 0.0 lên 1.5, nhận thấy cách mở đầu và cách diễn đạt của model có sự thay đổi rõ hơn, từ câu trả lời an toàn, quen thuộc sang đa dạng và sáng tạo hơn. Temperature thấp giúp model tạo ra kết quả ổn định và ít thay đổi, trong khi temperature cao cho phép model có nhiều lựa chọn hơn khi tạo nội dung, khiến câu trả lời có tính sáng tạo hơn.
### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Cần chọn temperature thấp vì cần câu trả lời ổn định, ít sáng tạo không cần thiết, tránh trả lời sai, cùng 1 câu hỏi nên cần trả lời gần giống nhau.
### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Với cùng một workload, GPT-4o có chi phí cao hơn khoảng 25 lần so với GPT-4o-mini. GPT-4o phù hợp với các trường hợp cần chất lượng reasoning cao, xử lý vấn đề phức tạp hoặc yêu cầu độ chính xác cao, trong khi GPT-4o-mini phù hợp hơn cho các tác vụ số lượng lớn như chatbot thông thường hoặc trả lời câu hỏi cơ bản để tối ưu chi phí.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Khi thay đổi system prompt, model thay đổi cách diễn đạt dù câu hỏi đầu vào giống nhau. Với persona giáo viên tiểu học, câu trả lời có xu hướng đơn giản, gần gũi và phù hợp với trẻ em, trong khi persona chuyên gia tài chính tạo ra phong cách chuyên nghiệp và sử dụng nhiều thuật ngữ chuyên môn hơn. Điều này cho thấy system prompt có ảnh hưởng lớn đến hành vi, phong cách và mức độ chi tiết của model.
### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với đoạn văn 70 từ, count_tokens bằng tiktoken trả về 78 token, trong khi cách ước lượng số từ / 0.75 cho kết quả khoảng 93 token, chênh lệch khoảng 16%. Sự khác biệt xảy ra vì tokenizer không đếm theo số từ mà chia văn bản thành các đơn vị nhỏ hơn gọi là token. Đối với tiếng Việt, số token có thể khác nhiều so với tiếng Anh do đặc điểm dấu, âm tiết và cách tokenizer phân tách từ.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi model tạo phản hồi dài hoặc cần thời gian xử lý lớn, vì người dùng có thể nhìn thấy kết quả ngay khi từng phần được sinh ra thay vì phải chờ toàn bộ câu trả lời hoàn thành. Điều này giúp cải thiện trải nghiệm với chatbot hoặc trợ lý AI. Non-streaming phù hợp hơn khi cần xử lý toàn bộ kết quả trước, ví dụ khi cần phân tích hoặc kiểm tra một output hoàn chỉnh.
### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp hệ thống tăng dần thời gian chờ sau mỗi lần gọi API thất bại, từ đó giảm áp lực lên server khi hệ thống đang quá tải. Nếu dùng delay cố định, hàng nghìn client có thể cùng retry tại một thời điểm và tạo ra một đợt tải mới. Exponential backoff giúp phân tán các lần retry và tăng khả năng request thành công.
---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona trợ lý học tập AI bằng tiếng Việt. System prompt yêu cầu trợ lý giải thích rõ ràng, có ví dụ thực tế và điều chỉnh mức độ chi tiết phù hợp với người dùng. Cụm "giải thích từng bước" giúp model ưu tiên trình bày quá trình suy nghĩ dễ hiểu thay vì chỉ đưa ra câu trả lời ngắn.
### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất của trợ lý hiện tại là history chỉ lưu được một số lượt hội thoại gần nhất nên có thể mất thông tin trong các cuộc trò chuyện dài. Một cải thiện có thể triển khai là lưu lại bản tóm tắt của các đoạn hội thoại cũ hoặc sử dụng database kết hợp RAG để tạo bộ nhớ dài hạn cho trợ lý.
---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
