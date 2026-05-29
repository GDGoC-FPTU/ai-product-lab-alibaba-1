# 🏗️ Phase 3 — DEEP-DIVE: VinFast Intelligent Fault Diagnosis

**Nhóm:** Vin Smart Future - Team 1
**Thành viên:** Phan Anh Thắng - [2A202600844]

---

## 🗳️ Quyết định lựa chọn của nhóm
Nhóm quyết định chọn bài toán **"Card #1 — VinFast: Chẩn đoán lỗi xe từ mô tả của khách hàng"** để thực hiện Deep-Dive.

### Lý do lựa chọn và loại bỏ các thẻ khác:
*   **Lý do chọn Card #1:** Đây là bài toán có "AI-fit" cao nhất. Việc hiểu các mô tả cảm tính của khách hàng (ví dụ: *"xe đi vào chỗ xóc kêu lục cục"*) là thế mạnh tuyệt đối của LLM so với code truyền thống. Nó trực tiếp cải thiện chỉ số hài lòng khách hàng (CSI) tại các xưởng dịch vụ VinFast.
*   **Card #2 (Vinmec Summary):** Rất tiềm năng nhưng độ rủi ro cao (Mission Critical). Trong môi trường y tế, sai sót nhỏ trong tóm tắt có thể dẫn đến hậu quả pháp lý nghiêm trọng. Cần nhiều thời gian hơn để thiết lập các lớp kiểm chứng (Validation Layers).
*   **Card #3 (Vinpearl Booking):** Bài toán mang tính chất vận hành nội bộ (Back-office). Mặc dù tiết kiệm thời gian nhưng không tạo ra tác động trực tiếp lên trải nghiệm người dùng cuối mạnh mẽ bằng việc chẩn đoán lỗi xe thời gian thực.

---

## 3.1. Current-State Workflow Mapping
*Chi tiết quy trình được mô tả trực quan trong file: **04-workflow-diagram.png***
*   **Tổng thời gian quy trình:** 22 phút/lượt xe.
*   **Nút thắt cổ chai (Bottleneck):** Khâu tra cứu mã lỗi DTC từ Wiki nội bộ (chiếm 12 phút ~ 55% tổng thời gian).

---

## 3.2. Problem Statement (6-field Standard) — VinFast Intelligent Fault Diagnosis

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Cố vấn dịch vụ (Service Advisor - SA) tại hệ thống xưởng dịch vụ VinFast toàn quốc. |
| **2. Current Workflow** | SA tiếp nhận xe -> Nghe khách hàng mô tả triệu chứng bằng ngôn ngữ phi kỹ thuật -> Ghi chép vào DMS -> Tra cứu Wiki kỹ thuật/Sổ tay sửa chữa để tìm mã lỗi DTC liên quan -> Giải thích cho Kỹ thuật viên (KTV). Toàn bộ quá trình tra cứu và mapping là thủ công (⏱ 22 phút/xe). |
| **3. Bottleneck** | **Semantic Mapping Gap (12 phút):** Khó khăn nhất là chuyển đổi các mô tả cảm tính (ví dụ: "xe đi vào gờ giảm tốc kêu lục cục", "vô lăng sượng") sang các mã DTC chính xác. |
| **4. Business Impact** | Lãng phí ~18 giờ lao động/ngày tại mỗi xưởng lớn (50 xe/ngày). Chẩn đoán sai dẫn đến chuẩn bị sai vật tư, tăng thời gian xe nằm xưởng (Lead-time) thêm 30% và lãng phí chi phí bảo hành. |
| **5. Success Metric** | 1. **Efficiency:** Giảm thời gian chẩn đoán sơ bộ từ 22 phút ──> dưới 3 phút.<br>2. **Quality:** Tỉ lệ AI gợi ý đúng phân vùng hệ thống lỗi đạt >90%. |
| **6. Operational Boundary** | **ĐƯỢC PHÉP:** Truy xuất database DTC để soạn nháp (draft) nội dung Job Card. **CẤM:** Tuyệt đối không tự ý kích hoạt lệnh đặt hàng phụ tùng (Part Order) tự động; không được đưa ra kết luận cuối cùng về an toàn xe nếu chưa có chữ ký xác nhận của SA (Bắt buộc HITL). |

---

## 3.3. Future-State Flow & AI Fit

*   **AI Fit:** Chọn **LLM Feature** (Lý do: Khả năng xử lý ngôn ngữ tự nhiên đa vùng miền của khách hàng Việt Nam là thế mạnh cốt lõi của LLM. Chúng tôi không dùng Agent tự trị để đảm bảo tính kiểm soát tuyệt đối (Predictability), tránh rủi ro AI tự đưa ra kết luận kỹ thuật sai gây nguy hiểm cho người dùng).

*   **Quy trình tương lai (Future-State):**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận mô tả   │     │ 🔵 AI Draft  │     │ 🟢 Advisor   │     │ Hoàn tất     │
│ từ khách hàng│ ──→ │ chẩn đoán    │ ──→ │ click duyệt  │ ──→ │ Job Card &   │
│ (Tiếng Việt) │     │ & mã lỗi DTC │     │ & bổ sung    │     │ chuyển thợ   │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                   │
                                            ↩️ Fallback:
                                            Nếu AI báo độ tin cậy < 80%,
                                            hệ thống yêu cầu Advisor
                                            tra cứu Wiki thủ công 
                                            như quy trình hiện tại.
```

---

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:
1. [x] **Data Availability:** Đã có bộ dữ liệu chuẩn hóa gồm 5,000+ mã DTC, tài liệu hướng dẫn sửa chữa (Service Manual) và nhật ký mô tả triệu chứng lịch sử từ hệ thống DMS.
2. [x] **Risk Management:** Rủi ro chẩn đoán sai được kiểm soát chặt chẽ thông qua cơ chế **Human-in-the-loop (HITL)**. AI chỉ đóng vai trò gợi ý (Smart Copilot), Cố vấn dịch vụ (SA) là người phê duyệt và chịu trách nhiệm cuối cùng.
3. [x] **Technical Infrastructure:** Hệ thống Quản lý xưởng (DMS) hiện tại có kiến trúc hiện đại, sẵn sàng tích hợp module AI thông qua RESTful API.
4. [x] **Organizational Readiness:** Đội ngũ SA tại các xưởng trọng điểm đồng thuận cao vì công cụ giúp giảm tải áp lực công việc trong giờ cao điểm.
5. [x] **Safety Compliance:** Các ranh giới vận hành (Operational Boundaries) đã được thiết lập để ngăn chặn AI tự ý đưa ra kết luận về an toàn hoặc đặt hàng linh kiện mà không có sự kiểm tra thực tế.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
**[x] GO (Bắt đầu xây dựng Prototype)**

**Justification:**
1. **Đột phá về kỹ thuật:** LLM (Gemini 1.5/2.5 Flash) kết hợp với kỹ thuật **RAG (Retrieval-Augmented Generation)** giải quyết triệt để bài toán "Semantic Mapping". Khả năng hiểu ngôn ngữ tự nhiên đa vùng miền (như "kêu lục cục", "rơ lái", "mất lạnh") giúp ánh xạ chính xác triệu chứng vào mã lỗi DTC mà các hệ thống tra cứu từ khóa truyền thống thường bỏ sót.
2. **Hiệu quả kinh tế (ROI):** 
    *   **Chi phí vận hành:** Dự kiến < 2,000 VNĐ/lượt chẩn đoán (API tokens).
    *   **Lợi ích trực tiếp:** Giảm thời gian tra cứu và mapping lỗi từ 12 phút xuống < 1 phút. Tiết kiệm trung bình 60,000 VNĐ chi phí nhân công mỗi lượt xe.
    *   **Tỉ lệ ROI:** Ước tính đạt **1:40**. 
    *   **Giá trị chiến lược:** Giảm thời gian xe nằm xưởng (Lead-time), tăng công suất phục vụ của xưởng thêm ~15-20% mà không cần tuyển thêm nhân sự, đồng thời nâng cao chỉ số hài lòng khách hàng (CSI).
3. **Tính khả thi & An toàn:** Dự án có tính thực tiễn cao, triển khai theo mô hình "Assistant Layer" nên không làm xáo trộn quy trình sửa chữa hiện có. Các ranh giới cấm đã được kiểm chứng, đảm bảo tính an toàn tuyệt đối cho người dùng cuối.

---
*Báo cáo được thực hiện bởi nhóm AI Product Lab - Vin Smart Future.*