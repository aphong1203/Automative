# Bus and Signal Management

Quản lý dữ liệu (Signal) và giao tiếp (Bus) là xương sống của một mô hình Simulink phức tạp.

## 1. Bus Management
- **Virtual Bus:** (Dùng Bus Creator) Chỉ là gom nhóm đồ họa, không ảnh hưởng code sinh ra.
- **Non-virtual Bus:** (Bật `Output as nonvirtual bus`) Ép kiểu dữ liệu thành một cấu trúc (struct) trong C code. Bắt buộc phải có **Bus Object**.
- **Bus Object (`Simulink.Bus`):** Định nghĩa schema cho bus (tên, kiểu dữ liệu, đơn vị của từng phần tử).
- **Workflow khuyến nghị:** Dùng **Out Bus Element** thay vì `Bus Creator + Outport` để giảm rối dây.

## 2. Signal Metadata
Mỗi tín hiệu cần được định nghĩa rõ ràng:
- **Data Type:** Kiểu dữ liệu (double, single, int8, boolean, enum...).
- **Min/Max:** Giới hạn vật lý của tín hiệu. Quan trọng cho Fixed-point và kiểm tra an toàn.
- **Unit:** Đơn vị (m/s, rad, V...). Giúp tránh lỗi mismatch khi tích hợp.
- **Signal Specification:** Khối dùng để ép hoặc kiểm tra thuộc tính tín hiệu tại một điểm.

## 3. Simulink.Signal Object
Dùng để lưu trữ thuộc tính tín hiệu trong Workspace hoặc Data Dictionary, giúp tách biệt dữ liệu và mô hình.

---
**Xem thêm:**
- [[Subsystem Semantics]]
- [[Multirate and Rate Transition]]
