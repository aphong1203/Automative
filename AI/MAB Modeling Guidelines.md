# MAB Modeling Guidelines (Simulink)

## Tổng quan
Bộ quy tắc **MAB (MathWorks Automotive Advisory Board)** là hệ quy tắc chuẩn hóa cho việc mô hình hóa trong ngành ô tô, tập trung vào 3 mục tiêu chính:
1. **Readability (Khả đọc):** Giúp người khác dễ dàng hiểu mô hình.
2. **Simulation & Verification:** Đảm bảo mô hình chạy đúng và dễ kiểm thử.
3. **Code Generation:** Đảm bảo code sinh ra từ mô hình hiệu quả và an toàn.

## Các nhóm quy tắc trọng yếu
- **Naming Conventions:** Đặt tên block, signal, port nhất quán.
- **Simulink Layout:** Cách sắp xếp block, đường nối signal (tránh chồng chéo).
- **Stateflow Logic:** Quy tắc cho biểu đồ trạng thái.
- **MATLAB Function:** Cách viết code trong mô hình.

## Model Advisor
Model Advisor là công cụ chính để kiểm tra việc tuân thủ MAB.
- **Check IDs quan trọng:** 
    - `jc_0640`, `jc_0641`: Kiểm tra kết nối signal line.
    - `jc_0644`: Kiểm tra naming/labels.
    - `jc_0656`, `jc_0659`: Kiểm tra naming cho Inport/Outport.
    - `jc_0281`: Kiểm tra initial output của conditional subsystems.

## Quy trình thực hiện
1. Thiết kế mô hình.
2. Mở **Model Advisor** (phím tắt `Ctrl+Shift+A`).
3. Chọn thư mục **MAB Checks**.
4. Chạy và sửa lỗi dựa trên report.
5. Xuất report HTML để lưu trữ.

---
**Xem thêm:**
- [[Subsystem Semantics]]
- [[Index - Automotive Engineer Knowledge]]
