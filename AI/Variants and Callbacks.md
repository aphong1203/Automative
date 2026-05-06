# Variants and Callbacks

Kỹ thuật nâng cao để quản lý biến thể mô hình và vòng đời thực thi.

## 1. Variant Subsystem
Cho phép chọn một trong nhiều cấu trúc mô hình (Implementation) khác nhau.
- **Control Modes:**
    - `expression`: Dùng biến MATLAB (ví dụ `MODE == 1`).
    - `label`: Chọn qua UI.
- **Activation Time:**
    - `update diagram`: Quyết định khi nhấn Ctrl+D.
    - `code compile`: Quyết định khi sinh code.
    - `startup`: Quyết định khi chạy (linh hoạt nhất).

## 2. Callbacks
Đoạn code MATLAB tự động chạy tại các sự kiện của mô hình:
- **`PreLoadFcn`:** Chạy trước khi mở model. Dùng để load Bus Objects, Data Dictionaries.
- **`InitFcn`:** Chạy khi bắt đầu compile/simulation. Dùng để khởi tạo giá trị biến, Variant controls.
- **`ModelCloseFcn`:** Chạy khi đóng model. Dùng để dọn dẹp workspace hoặc đóng log.

## Thứ tự thực hiện (Khuyến nghị)
1. `PreLoadFcn`: Nạp schema và đối tượng.
2. `InitFcn`: Gán giá trị vận hành.
3. Chạy mô phỏng.
4. `ModelCloseFcn`: Dọn dẹp.

---
**Xem thêm:**
- [[Simulink Environment Setup (MATLAB)]]
- [[Index - Automotive Engineer Knowledge]]
