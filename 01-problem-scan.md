## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.


# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **VinFast** | AI-upgrade | Chẩn đoán mã lỗi kỹ thuật sơ bộ dựa trên mô tả triệu chứng bằng ngôn ngữ tự nhiên của khách hàng (Tiếng Việt). |
| 2 | **Xanh SM** | Lặp lại | Tự động phân loại và tóm tắt lý do khách hàng hủy chuyến từ ghi âm cuộc gọi và ghi chú tài xế để tối ưu vận hành. |
| 3 | **Vinhomes** | AI-upgrade | Hệ thống tự động phân loại và điều hướng (routing) khiếu nại cư dân trên App Vinhomes Resident đến đúng ban quản lý tòa nhà. |
| 4 | **Vinmec** | Tốn thời gian | Trích xuất dữ liệu từ EHR để soạn thảo bản tóm tắt hồ sơ xuất viện (Discharge Summary) cho bác sĩ. |
| 5 | **Vinpearl** | Tốn thời gian | Tự động hóa quy trình check phòng trống và soạn báo giá cho các yêu cầu đặt phòng đoàn (Group Booking) qua Email. |
| 6 | **VinFast** | Lặp lại | Đối soát hóa đơn sạc điện từ các đối tác trạm sạc thứ ba (Roaming partners) với dữ liệu hệ thống nội bộ. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn top 3 bài toán tiềm năng nhất: **#1 (VinFast Fault Diagnosis), #4 (Vinmec Summary), #5 (Vinpearl Booking Agent).**

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Chẩn đoán mã lỗi kỹ thuật sơ bộ từ mô tả triệu    │
│ chứng của khách hàng bằng tiếng Việt.                       │
│ Công ty thành viên: [x] VinFast (EV)                        │
│                                                             │
│ Ai đang đau? Kỹ thuật viên (Service Advisor), Khách hàng    │
│                                                             │
│ Workflow thủ công hiện tại (3 bước):                        │
│   1. Tiếp nhận mô tả triệu chứng cảm tính từ KH             │
│   → 2. Ghi chép bệnh trạng vào hệ thống quản lý             │
│   → 3. Tra cứu thủ công mã lỗi DTC & hệ thống liên quan     │
│                                                             │
│ Bước nào tốn nhất? Bước 3 (⏱ 12 phút/lượt)                  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│ (Tự động mapping triệu chứng -> mã lỗi DTC tiềm năng)       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian chẩn đoán ban đầu từ 22 phút ──> dưới 3 phút;│
│ Tỉ lệ gợi ý đúng phân vùng lỗi kỹ thuật đạt >85%.           │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Classification/Mapping)│
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Trích xuất dữ liệu EHR để soạn thảo "Tóm tắt hồ sơ│
│ xuất viện" (Discharge Summary) tự động.                     │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ điều trị (Quá tải thủ tục hành chính)   │
│                                                             │
│ Workflow thủ công hiện tại (3 bước):                        │
│   1. Review lại toàn bộ bệnh án điện tử (EHR) của bệnh nhân │
│   → 2. Lọc các chỉ số cận lâm sàng và diễn tiến quan trọng  │
│   → 3. Soạn thảo văn bản tóm tắt bằng ngôn ngữ dễ hiểu      │
│                                                             │
│ Bước nào tốn nhất? Bước 2 & 3 (⏱ 20 phút/ca)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│ (Trích xuất thông tin key -> Soạn thảo bản nháp tóm tắt)    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian bác sĩ soạn hồ sơ từ 20p ──> dưới 4p/ca.     │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Extraction & Drafting) │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Tự động xử lý yêu cầu đặt phòng đoàn (Group       │
│ Booking) qua Email và soạn báo giá nháp.                    │
│ Công ty thành viên: [x] Vinpearl / VinWonders               │
│                                                             │
│ Ai đang đau? Nhân viên Sales Admin & Reservation            │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Đọc Email yêu cầu → 2. Tra cứu quỹ phòng trống trên PMS│
│   → 3. Tính toán giá theo chính sách → 4. Soạn Email báo giá│
│                                                             │
│ Bước nào tốn nhất? Bước 2 & 3 (⏱ 30 phút/lượt)              │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Toàn bộ 4 bước        │
│ (Trích xuất thông tin -> Check API PMS -> Draft báo giá)    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ 90% email được xử lý nháp trong < 5 phút;                   │
│ Tỉ lệ sai sót thông tin phòng/giá giảm xuống dưới 2%.       │
│                                                             │
│ Quick Architecture: [x] Agentic Loop (Reasoning & Tool Use) │
└─────────────────────────────────────────────────────────────┘

---

# 🗳️ Quyết định lựa chọn của nhóm:
Nhóm quyết định chọn bài toán **"Card #1 — VinFast: Chẩn đoán lỗi xe từ mô tả của khách hàng"** để thực hiện Deep-Dive.

## Lý do lựa chọn và loại bỏ các thẻ khác:
*   **Lý do chọn Card #1:** Đây là bài toán có "AI-fit" cao nhất. Việc hiểu các mô tả cảm tính của khách hàng (ví dụ: *"xe đi vào chỗ xóc kêu lục cục"*) là thế mạnh tuyệt đối của LLM so với code truyền thống. Nó trực tiếp cải thiện chỉ số hài lòng khách hàng (CSI) tại các xưởng dịch vụ VinFast.
*   **Card #2 (Vinmec Summary):** Rất tiềm năng nhưng độ rủi ro cao (Mission Critical). Trong môi trường y tế, sai sót nhỏ trong tóm tắt có thể dẫn đến hậu quả pháp lý nghiêm trọng. Cần nhiều thời gian hơn để thiết lập các lớp kiểm chứng (Validation Layers).
*   **Card #3 (Vinpearl Booking):** Bài toán mang tính chất vận hành nội bộ (Back-office). Mặc dù tiết kiệm thời gian nhưng không tạo ra tác động trực tiếp lên trải nghiệm người dùng cuối mạnh mẽ bằng việc chẩn đoán lỗi xe thời gian thực.

---
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---