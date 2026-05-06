# Subsystem Semantics

Trong Simulink, việc chọn loại Subsystem quyết định **ngữ nghĩa thực thi (execution semantics)**, không chỉ là cách gom nhóm đồ họa.

## 1. Atomic Subsystem
- **Đặc điểm:** Chạy như một đơn vị duy nhất. Block order bên trong tách biệt với bên ngoài.
- **Khi nào dùng:** Khi cần modularize, tạo biên thực thi ổn định, hoặc chuẩn bị cho Model Reference.
- **Cài đặt:** Bật `Treat as atomic unit`.

## 2. Enabled Subsystem
- **Đặc điểm:** Chạy khi tín hiệu tại cổng Enable > 0.
- **Hành vi:** Giữ trạng thái (state) và output cuối cùng khi bị disable (có thể cấu hình reset).
- **Khi nào dùng:** Khi thuật toán cần "gating" theo điều kiện logic.

## 3. Triggered Subsystem
- **Đặc điểm:** Chạy tại thời điểm xảy ra sự kiện (rising/falling edge).
- **Hành vi:** Là một "conditionally executed atomic subsystem". Không được chứa khối liên tục (Integrator).
- **Khi nào dùng:** Event-driven logic, xử lý ngắt (interrupt).

## 4. Action Subsystem (If/Switch Case)
- **Đặc điểm:** Chạy khi khối `If` hoặc `Switch Case` tương ứng kích hoạt.
- **Hành vi:** Các nhánh loại trừ nhau (mutually exclusive). Thường kết hợp với khối `Merge`.
- **Khi nào dùng:** Logic rẽ nhánh if-else hoặc switch-case phức tạp.

## So sánh nhanh
| Kiểu | Kích hoạt | Output khi không chạy |
|---|---|---|
| **Enabled** | Mức logic (>0) | Giữ hoặc Reset |
| **Triggered** | Cạnh (Edge) | Giữ giá trị cuối |
| **Action** | Điều kiện logic | Thường qua Merge |

---
**Xem thêm:**
- [[MAB Modeling Guidelines]]
- [[Multirate and Rate Transition]]
