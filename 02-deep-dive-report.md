# 📄 Phase 3 & Phase 5 — DEEP-DIVE & EVALUATION REPORT

**Tên Nhóm:** Alibaba
**Thành viên tham gia:**
1. Tạ Văn Huấn (MSSV: 2A202600984)
2. Phan Anh Thắng (MSSV: 2A202600844)

---

## 🏗️ Phase 3 — DEEP-DIVE

### Quyết định lựa chọn:
Nhóm quyết định chọn bài toán **"QUICK PROBLEM CARD #1 — Xanh SM Xử lý sự cố sạc pin thực địa"** để thực hiện phân tích sâu (Deep-Dive).

#### Lý do lựa chọn và loại bỏ các thẻ khác:
* **Loại bỏ Card #2 (Vinhomes CSKH):** Mặc dù quy trình phân loại ý kiến cư dân Vinhomes tốn nhiều thời gian của ban quản lý, nhưng mức độ rủi ro thông tin liên quan đến các vấn đề pháp lý, tranh chấp căn hộ hoặc sự cố kỹ thuật hạ tầng là rất lớn. Nếu LLM phân loại sai hoặc trả lời nhầm lẫn có thể gây ra khủng hoảng truyền thông. Cần tích lũy thêm dữ liệu sạch và chuẩn bị giải pháp kết hợp Rule-based kỹ lưỡng hơn.
* **Loại bỏ Card #3 (Vinmec Tóm tắt Hồ sơ):** Tóm tắt thông tin y khoa đòi hỏi độ chính xác tuyệt đối (100% không sai lệch thông tin lâm sàng). Việc ứng dụng LLM trực tiếp có nguy cơ xảy ra lỗi ảo giác (hallucination) về liều lượng thuốc hoặc thuật ngữ bệnh lý, cực kỳ nguy hiểm cho tính mạng bệnh nhân. Do đó, đây là dự án có độ rủi ro rất cao ở thời điểm hiện tại.
* **Lựa chọn Card #1 (Xanh SM Sự cố sạc):** Bài toán có quy trình nghiệp vụ rõ ràng, dữ liệu định vị xe và trạm sạc có cấu trúc tốt, có thể kiểm soát hoàn toàn rủi ro thông qua giải pháp **Human-in-the-loop (HITL)** (Điều phối viên duyệt tin nháp trước khi gửi) và quy tắc dự phòng **Fallback** (Xe cứu hộ sạc di động khi pin dưới 5%).

---

### 3.2. Problem Statement (6-field) — Tiêu chuẩn Vin Smart Future

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Điều vận Xanh SM. |
| **2. Current Workflow** | Khi tài xế báo hết pin, điều phối viên tra cứu vị trí xe trên bản đồ GPS nội bộ, mở Dashboard trạm sạc VinFast để tìm trụ sạc trống gần nhất, viết tin nhắn chỉ dẫn/định vị gửi qua App tài xế, và gọi cứu hộ nếu pin dưới 5%. Quy trình 5 bước hoàn toàn thủ công, mất trung bình 15 phút/lượt. |
| **3. Bottleneck** | Bước 3 & 4 (mất 10 phút): Tra cứu thủ công trụ sạc trống phù hợp với dòng xe (VF5/VFe34/VF8) và soạn thảo tin nhắn hướng dẫn đường đi chi tiết bằng Tiếng Việt thân thiện, rõ ràng. |
| **4. Business Impact** | Hà Nội trung bình có ~80 sự cố sạc pin thực địa/ngày. Lãng phí 20 giờ làm việc/ngày của team điều phối. Việc tài xế phải chờ đợi lâu làm tăng tỉ lệ hủy chuyến, gây rò rỉ doanh thu ước tính ~15% và gây ức chế tâm lý cho tài xế thực địa. |
| **5. Success Metric** | 1. Giảm tổng thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút (Tăng 80% hiệu suất).<br>2. Tỉ lệ hướng dẫn đúng địa điểm và đúng loại trụ sạc phù hợp đạt 98%. |
| **6. Operational Boundary** | AI được phép truy xuất API định vị xe, API trạng thái trụ sạc VinFast, tự động soạn thảo tin nhắn chỉ đường ở dạng bản nháp (draft). **CẤM:** AI không được tự động gửi tin đi cho tài xế mà không có điều phối viên phê duyệt (Bắt buộc phải có HITL); không được đề xuất trạm sạc xa hơn 5km nếu lượng pin hiện tại dưới 5% (bắt buộc kích hoạt Cứu Hộ Di Động). |

---

### 3.3. Future-State Flow & AI Fit

* **AI Fit:** Phân loại thuộc **LLM Feature** (Hỗ trợ đọc hiểu thông tin và soạn thảo văn bản nháp thông minh). Quy trình nghiệp vụ có cấu trúc cố định và rủi ro điều phối sai trạm sạc lớn nên cần kiểm soát chặt chẽ bằng Rule-based kết hợp LLM, không áp dụng Agent tự trị hoàn toàn.

#### Quy trình tương lai (Future-State Workflow):
```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ 🔵 Tự động   │     │ 🔵 AI soạn   │     │ 🟢 Điều phối │
│ gọi sự cố    │ ──→ │ lấy vị trí   │ ──→ │ tin nhắn nháp│ ──→ │ viên click   │
│ từ tài xế    │     │ & trạm trống │     │ (gắn thẻ nháp)│    │ duyệt & gửi  │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                       │
                                                                       ▼
                                                                ↩️ Fallback:
                                                                Nếu AI soạn lỗi,
                                                                điều phối viên
                                                                soạn tay lại như cũ.
```

---

## 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? (Có log GPS và lịch sử trạng thái trạm sạc VinFast).
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát? (Hoàn toàn kiểm soát được nhờ cơ chế Human-in-the-loop duyệt tin nhắn nháp và Fallback sạc cứu hộ di động).
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? (Đội ngũ điều phối viên cực kỳ hoan nghênh vì giúp giảm tải 80% áp lực giờ cao điểm).

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển phiên bản MVP với scope hẹp phục vụ cho khu vực nội thành Hà Nội.

#### Luận điểm kỹ thuật và chi phí (Justification):
1. **Tính khả thi kỹ thuật:** Việc tích hợp LLM làm nhiệm vụ tóm tắt dữ liệu định vị và soạn văn bản tiếng Việt là thế mạnh cốt lõi của Gemini 2.5 Flash, thời gian phản hồi dưới 3 giây đảm bảo tính thời gian thực (real-time).
2. **Hiệu quả kinh tế (ROI):** Ước tính chi phí vận hành API Gemini cực kỳ thấp (~$0.01/cuốc sự cố), trong khi giúp tiết kiệm 20 giờ làm việc/ngày của nhân sự và giảm thiểu 15% thất thoát doanh thu do xe nằm chờ hết pin. Dự án có tiềm năng thu hồi vốn cực kỳ nhanh chóng.
