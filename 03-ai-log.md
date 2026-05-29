# 📝 Phase 6 — AI Reflection Log: VinFast Intelligent Fault Diagnosis

**Người thực hiện:** Phan Anh Thắng
**Dự án:** Hệ thống hỗ trợ chẩn đoán lỗi xe VinFast

---

### 1. AI đã giúp ích gì cho tôi? (AI as Thought Partner)
Trong suốt buổi Lab, AI (Gemini/Claude) đã đóng vai trò là một chuyên gia tư vấn chiến lược và kỹ thuật:
*   **Định lượng Business Impact:** AI giúp tôi tính toán chỉ số ROI (1:30) một cách logic bằng cách ước tính chi phí API so với giá trị giờ công của Service Advisor. Điều này giúp bài toán thuyết phục hơn rất nhiều về mặt kinh tế.
*   **Cấu trúc hóa dữ liệu:** AI hỗ trợ chuyển đổi các ý tưởng sơ khai về quy trình sửa chữa tại xưởng thành sơ đồ Workflow chuyên nghiệp và bảng 6-field đúng tiêu chuẩn.
*   **Brainstorm Ranh giới an toàn:** AI đã gợi ý cho tôi các kịch bản "tấn công" (adversarial inputs) rất thực tế, ví dụ như khách hàng cố tình ép hệ thống đặt hàng linh kiện ngay lập tức để bỏ qua quy trình kiểm soát.

### 2. AI đã "sai" ở đâu hoặc gây khó khăn gì? (Hallucination & AI Risks)
Tôi đã quan sát thấy một vài điểm yếu của AI trong quá trình làm việc:
*   **Xu hướng "Over-automation":** Ban đầu, AI liên tục đề xuất xây dựng một **Autonomous Agent** có thể tự động ra lệnh sửa chữa và đặt hàng linh kiện. Điều này cực kỳ nguy hiểm trong ngành ô tô vì có thể dẫn đến sai sót kỹ thuật gây mất an toàn hoặc lãng phí chi phí thay thế linh kiện không cần thiết.
*   **Thiếu ngữ cảnh thực địa:** AI ban đầu không tính đến việc Service Advisor tại xưởng thường làm việc trong môi trường ồn ào và cần giao diện "Quick-approve" hơn là đọc những đoạn văn dài.
*   **Lỗi logic ranh giới:** Khi tôi yêu cầu thiết lập ranh giới, AI đôi khi vẫn "quên" thẻ `[DRAFT_ONLY]` nếu câu lệnh của người dùng quá khẩn thiết (ví dụ: "Cứu hỏa! Gửi ngay đi!").

### 3. Tôi đã điều chỉnh và "dạy" AI như thế nào? (Human-in-the-loop)
Để kiểm soát AI và đảm bảo chất lượng đầu ra, tôi đã thực hiện:
*   **Hạ cấp độ tự trị (Downgrading):** Tôi kiên quyết ép AI vào khung **LLM Feature** (chỉ soạn nháp) thay vì cho phép nó thực thi hành động (Agentic Loop).
*   **Kỹ thuật Few-shot Prompting:** Tôi đã cung cấp các ví dụ mẫu về cách chuyển đổi từ triệu chứng "lục cục" sang mã lỗi hệ thống treo để AI hiểu rõ độ sâu kỹ thuật cần thiết.
*   **Thiết lập Fallback nghiêm ngặt:** Tôi đã bổ sung bước "Fallback" vào quy trình: Nếu độ tự tin của AI dưới 80%, hệ thống phải yêu cầu Advisor tra cứu thủ công thay vì đưa ra gợi ý mơ hồ.

---
**Bài học rút ra:** AI là một "trợ lý thông minh" nhưng không phải là "người quyết định". Trong các lĩnh vực liên quan đến an toàn tính mạng và chi phí cao như VinFast, vai trò của con người (Service Advisor) là không thể thay thế. Tôi học được cách dùng AI để tăng năng suất 10 lần nhưng vẫn giữ được 100% độ an toàn thông qua việc thiết lập ranh giới (Operational Boundary) chặt chẽ.