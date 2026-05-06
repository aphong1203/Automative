
---
# State-Space Modeling in Simulink (Expert Guide)

## 1. Định nghĩa toán học
Khối **State-Space** mô tả hệ động lực tuyến tính (LTI) thông qua không gian trạng thái:
$$ \dot{x} = Ax + Bu $$
$$ y = Cx + Du $$

Trong đó:
- **$x$**: Vector trạng thái (State vector) - đại diện cho "bộ nhớ" của hệ thống.
- **$u$**: Vector đầu vào (Input vector).
- **$y$**: Vector đầu ra (Output vector).
- **$A, B, C, D$**: Các ma trận hệ thống.

## 2. Ý nghĩa ma trận trong thiết kế điều khiển
| Ma trận | Tên gọi | Ý nghĩa MBD |
| :--- | :--- | :--- |
| **A** | System Matrix | Đặc tính động học nội tại (quyết định tính ổn định). |
| **B** | Input Matrix | Cách tín hiệu điều khiển tác động vào trạng thái. |
| **C** | Output Matrix | Cách trạng thái được ánh xạ ra tín hiệu đo lường. |
| **D** | Feedthrough Matrix | Truyền thẳng từ Input sang Output (thường để bằng 0). |

## 3. Lưu ý quan trọng cho kỹ sư MBD
#### A. Algebraic Loops
Nếu ma trận **$D \neq 0$**, hệ thống có *Direct Feedthrough*. Điều này thường gây ra **Algebraic Loop** trong Simulink, làm giảm hiệu năng mô phỏng và gây lỗi khi sinh code C.
- **Best Practice:** Luôn thiết kế hệ thống sao cho $D = 0$ (Strictly Proper System).

#### B. Discretization 
ECU thực tế không chạy thời gian liên tục. Khi chuyển từ mô hình mô phỏng sang code C:
- Sử dụng khối **Discrete State-Space** thay vì khối Continuous.
- Sử dụng phương pháp rời rạc hóa (ví dụ: Tustin hoặc ZOH) để đảm bảo đáp ứng tần số của mô hình rời rạc khớp với mô hình liên tục.

#### C. Data Management
- **Không nhập ma trận trực tiếp vào khối:** Hãy định nghĩa ma trận trong `Model Workspace` hoặc `Data Dictionary (.sldd)` dưới dạng biến MATLAB.
- **Sử dụng `ss()` object:** Định nghĩa hệ thống bằng lệnh `sys = ss(A,B,C,D)` trong script khởi tạo (`InitFcn`) để dễ dàng thực hiện các phép toán phân tích (như `eig(sys)`, `step(sys)`).

## 4. Checklist kiểm tra trước khi Simulation
- [ ] **Stability:** `eig(A)` có phần thực âm (đối với hệ ổn định).
- [ ] **Controllability:** `rank(ctrb(A, B)) == n` (hệ thống có thể điều khiển được).
- [ ] **Observability:** `rank(obsv(A, C)) == n` (hệ thống có thể quan sát được).
- [ ] **Scaling:** Các giá trị trong ma trận không quá chênh lệch (tránh lỗi số học khi tính toán trên chip 16/32-bit).

## 5. Troubleshooting
- **Simulink báo lỗi kích thước:** Kiểm tra lại số hàng/cột của ma trận theo quy tắc:
    - $A: n \times n$
    - $B: n \times m$
    - $C: r \times n$
    - $D: r \times m$
    *(Với $n$=số trạng thái, $m$=số đầu vào, $r$=số đầu ra)*

---
**Xem thêm:**
- [[Hệ thống Rời rạc và Z-transform]]
- [[Multirate and Rate Transition]]
- [[MBD Block Knowledge Base]]

---
*Ghi chú: Đối với hệ tự trị (không có đầu vào), hãy để trống ma trận B và D (ví dụ: `B = []`, `D = []`).*