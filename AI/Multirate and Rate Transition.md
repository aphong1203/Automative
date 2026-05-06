# Multirate and Rate Transition

Mô hình ô tô thường chạy đa tốc độ (multirate), ví dụ: Control loop chạy 10ms, Sensor đọc 100ms.

## 1. Vấn đề chuyển đổi tốc độ (Rate Transition)
Khi dữ liệu truyền giữa hai task có Sample Time khác nhau:
- **Fast-to-Slow:** Task nhanh ghi, task chậm đọc. Nguy cơ: Mất dữ liệu nếu không có buffer.
- **Slow-to-Fast:** Task chậm ghi, task nhanh đọc. Nguy cơ: Đọc trùng dữ liệu cũ.

## 2. Khối Rate Transition
Dùng để bảo vệ truyền dữ liệu:
- **Data Integrity:** Đảm bảo dữ liệu không bị hỏng khi task này đang ghi mà task kia đọc.
- **Deterministic:** Đảm bảo độ trễ (latency) cố định.
- **Chế độ phổ biến:**
    - `ZOH` (Zero Order Hold): Chốt giá trị.
    - `Buf`: Dùng buffer.
    - `Db_buf`: Double buffer.

## 3. Cấu hình tự động
Simulink có thể tự động chèn các khối ẩn: `Automatically handle rate transition for data transfer`.
**Lưu ý:** Trong thiết kế chuyên nghiệp, nên chèn thủ công ở các biên quan trọng để kiểm soát logic.

---
**Xem thêm:**
- [[Subsystem Semantics]]
- [[Bus and Signal Management]]
