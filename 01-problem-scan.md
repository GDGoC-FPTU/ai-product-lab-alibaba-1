# 📄 Phase 1 — SCAN & Phase 2 — QUICK-ASSESS

**Họ và tên:** Tạ Văn Huấn.
**MSSV:** 2A202600984  
**Lớp:** C401
**Công ty:** Vin Smart Future (Vingroup)  

---

## 🔍 Phase 1 — SCAN

Tôi đã sử dụng **4 Lenses** để quét qua hoạt động vận hành của các công ty thành viên Vingroup và xác định 5 bài toán thực tế sau:

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM (GSM)** | Tốn thời gian | Điều phối viên xử lý thủ công các báo cáo khẩn cấp từ tài xế về sự cố sạc pin hoặc hết pin thực địa (mất 15-20 min/lượt). |
| 2 | **VinFast** | Lặp lại | Bộ phận tài chính đối chiếu hóa đơn sạc điện và số liệu đối soát trạm sạc đối tác lớn hàng tuần hoàn toàn thủ công qua Excel. |
| 3 | **Vinhomes** | AI-upgrade | Phân loại và phân tuyến (routing) tự động các ý kiến, khiếu nại của cư dân trên App Vinhomes Resident để tránh tình trạng CSKH phản hồi chậm và rập khuôn. |
| 4 | **Vinmec** | Stakeholder Pain | Bác sĩ mất quá nhiều thời gian viết tóm tắt hồ sơ xuất viện (mất 20-30 phút/bệnh nhân), dẫn đến quá tải và giảm thời gian thăm khám trực tiếp. |
| 5 | **Vinpearl** | AI-upgrade | Chatbot AI thế hệ mới hỗ trợ tư vấn lịch trình tham quan chi tiết và cá nhân hóa cho du khách tại các tổ hợp VinWonders/Vinpearl. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Tôi đã chọn ra 3 bài toán tiềm năng nhất để thiết lập Quick Problem Cards chi tiết dưới đây:

### 1. QUICK PROBLEM CARD #1: Xanh SM Xử lý sự cố sạc pin thực địa

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo cáo sự cố sạc pin / hết pin    │
│ giữa đường cần điều phối cứu hộ hoặc trạm sạc gần nhất.     │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Tài xế (chờ đợi), Điều phối viên (quá tải)     │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài điều vận báo hết pin               │
│   → 2. Điều phối viên tra cứu thủ công vị trí xe trên bản đồ│
│   → 3. Tra cứu thủ công các trạm sạc VinFast còn trụ trống   │
│   → 4. Viết tin nhắn chỉ dẫn/đường đi gửi qua App tài xế    │
│   → 5. Liên hệ đội xe cứu hộ nếu xe đã cạn kiệt pin         │
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 12 phút/lượt)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (Tự động lấy vị trí -> Tra cứu trạm trống -> Soạn tin nháp)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút.      │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Tự động soạn chỉ dẫn)   │
└─────────────────────────────────────────────────────────────┘
```

### 2. QUICK PROBLEM CARD #2: Vinhomes Phân loại và Tuyến hóa Ý kiến Cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Tự động phân loại và route khiếu nại/ý kiến cư dân│
│ trên App Vinhomes Resident về đúng phòng ban xử lý.         │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Cư dân Vinhomes (chờ đợi phản hồi lâu),        │
│ Ban quản lý tòa nhà (mất thời gian đọc phân loại thủ công). │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh bằng văn bản/ảnh lên App           │
│   → 2. Điều phối viên BQL đọc phản ánh, phân loại thủ công  │
│   → 3. Chuyển tiếp phiếu yêu cầu đến bộ phận chuyên trách   │
│   → 4. Gửi phản hồi chuẩn hoặc cập nhật trạng thái cư dân.  │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 10 phút/phiếu yêu cầu)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và 3           │
│ (Tự động đọc hiểu -> Gán nhãn phân loại -> Định tuyến tự động)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian định tuyến từ 4 tiếng ──> dưới 5 phút,       │
│ đạt độ chính xác phân loại tự động > 92%.                   │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Phân loại & Gán nhãn)  │
└─────────────────────────────────────────────────────────────┘
```

### 3. QUICK PROBLEM CARD #3: Vinmec Tự động hóa Tóm tắt Hồ sơ Xuất viện

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Bác sĩ tốn quá nhiều thời gian viết tóm tắt hồ sơ │
│ xuất viện từ các ghi chú lâm sàng thô, đơn thuốc, xét nghiệm│
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ (quá tải sổ sách),                      │
│ Bệnh nhân (chờ đợi thủ tục xuất viện lâu).                  │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bác sĩ thu thập kết quả xét nghiệm, lịch sử lâm sàng   │
│   → 2. Bác sĩ viết tay tóm tắt diễn tiến bệnh, phương pháp  │
│   → 3. Bác sĩ soạn đơn thuốc dặn dò xuất viện               │
│   → 4. Ký duyệt hồ sơ và gửi bệnh nhân làm thủ tục xuất viện│
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 25 phút/bệnh nhân)          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và 3           │
│ (Đọc tổng hợp dữ liệu thô -> Tạo văn bản tóm tắt hồ sơ nháp)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian soạn hồ sơ của bác sĩ từ 25 phút ──> 5 phút  │
│ giúp tăng 15% năng lực tiếp nhận bệnh nhân.                 │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Tóm tắt y tế an toàn)  │
└─────────────────────────────────────────────────────────────┘
```
