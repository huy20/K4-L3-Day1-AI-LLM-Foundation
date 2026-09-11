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

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Với temperature 0.0, kết quả tương tự nhau ở các lần gọi. Khi tăng temperature lên, kết quả trả ra đa dạng hơn. Khi temperature quá cao (1.5) thì kết quả đôi khi vô nghĩa và không còn đúng sự thật.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Mình sẽ đặt temperature ở 0.3, để cân bằng giữa độ chính xác và độ sáng tạo. Độ sáng tạo sẽ giúp model không trả lời câu hỏi một cách rập khuôn, giúp khách hàng không cảm thấy chán khi sử dụng, còn độ chính xác sẽ đảm bảo câu trả lời không bị sai lệch với thực tế.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Sau khi tính toán, GPT-4o tốn khoảng 0.875$ và GPT-4o-mini tốn khoảng 0.0525$ \
=> Đắt hơn khoảng 17 lần. \
Trường hợp xứng đáng: Khi cần độ chính xác cao, ví dụ: Tra cứu thông tin, vấn đáp chuyên sâu.\
Trường hợp không xứng đáng: Khi cần tốc độ cao và chi phí thấp, ví dụ: Chatbot trả lời tự động 
---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Bạn là giáo viên: Phản hồi ngắn gọn, sử dụng ví dụ thực tế dễ hiểu.
> Bạn là chuyên gia: Phản hồi dài hơn, sử dụng từ ngữ chuyên ngành, cung cấp thông tin chi tiết hơn. 
System prompt ảnh hưởng đến hành vi của model bằng cách định hướng cách nó xử lý và phản hồi thông tin dựa trên vai trò và bối cảnh được giao.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài vì tiếng Việt sử dụng dấu câu, mã Unicode và khoảng trống để phân tách từ, trong khi tiếng Anh có thể kết hợp nhiều từ thành một token thông qua cấu trúc ngữ pháp.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming quan trọng nhất trong trường hợp chatbot trả lời dài, ví dụ: chatbot trả lời dài, trong khi non-streaming phù hợp hơn khi cần tốc độ cao và chi phí thấp, ví dụ: chatbot trả lời ngắn gọn.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Nếu hàng nghìn client cùng retry với delay cố định giống nhau, hàng nghìn yêu cầu sẽ dồn liên tục với khoảng thời gian như nhau, server sẽ bị quá tải và không thể xử lý hết các yêu cầu. Khi sử dụng backoff theo cấp số nhân, các yêu cầu sẽ được dồn với khoảng thời gian khác nhau (người đầu tiên sẽ có thời gian chờ lâu hơn người sau), giúp server có nhiều thời gian hơn để xử lý các yêu cầu.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> 

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất của trợ lý là tất cả lịch sử trò chuyện được lưu vào một mảng history. Nếu 1 phiên trả lời dài thì sau vài phiên, token đầu vào có thể vượt quá max_token, và có thể cutoff khiến cho câu trả lời bị sai.\
Phương hướng giải quyết: tóm tắt lại câu trả lời lại và lưu vào database, sau đó trợ lý sẽ truy cập vào database. Với hướng tiếp cận này thì chi phí token sẽ tối ưu hơn với các phiên trò chuyện dài nếu tăng số lượng phiên trả lời trong history lên.
---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
