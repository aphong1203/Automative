---
tags:
  - MBD
  - Simulink
  - Automotive
  - KnowledgeBase
  - Documentation
---

# 🧠 MBD Block Knowledge Base (Automotive Edition)

> [!ABSTRACT] Tổng quan
> Tài liệu này cung cấp cái nhìn chi tiết về 50 block Simulink phổ biến nhất được sử dụng trong ngành kỹ thuật ô tô. Mỗi block được phân tích qua 5 chiều: Chức năng, Toán học, Giao diện dữ liệu, Hành vi mô phỏng và Sinh mã (Code Generation).

---

## 📌 Phân tích chi tiết theo Nhóm

### Nhóm 1: Sources (Nguồn tín hiệu)

#### 1. Inport
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Sources`
> - **Ứng dụng:** ECU input: sensor voltage, CAN signal

- **Chức năng:** Tạo cổng đầu vào cho model hoặc subsystem. Đây là điểm nhận dữ liệu từ cấp cha hoặc từ môi trường thực.
- **Bản chất toán học:** Truyền thẳng không biến đổi: $y = u$.

> [!SETTINGS] Data & Interface
> - **IN:** N/A (đầu vào từ cấp cha)
> - **OUT:** Signal (bất kỳ kiểu dữ liệu)
> - **Sample time:** Kế thừa từ cha

- **Hành vi Runtime:** Tại mỗi time step, giá trị được truyền từ cấp gọi vào port tương ứng. Không có trạng thái nội bộ.

> [!CODE] Code Generation
> - **Pattern:** `void model_step(real_T u_SpeedIn, ...)` hoặc `ExtU_model.SpeedIn`
> - **AUTOSAR:** `Rte_IRead_<runnable>_<port>()`

---

#### 2. Constant
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Sources`
> - **Ứng dụng:** Threshold so sánh, hệ số gain calibration

- **Chức năng:** Phát ra giá trị hằng số không đổi. Dùng làm threshold, calibration parameter, offset.
- **Bản chất toán học:** $y(t) = K$.

> [!SETTINGS] Data & Interface
> - **OUT:** Signal kiểu $K$
> - **Sample time:** `inf` (constant) hoặc kế thừa

> [!CODE] Code Generation
> - **Pattern:** `#define MODEL_THRESHOLD (5.0)` hoặc `P.threshold = 5.0F;`
> - **AUTOSAR:** `Rte_Prm_<name>_Value()`

---

#### 3. From
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Signal Routing`
> - **Ứng dụng:** Chia sẻ tín hiệu chế độ vehicle state qua nhiều subsystem

- **Chức năng:** Nhận tín hiệu từ `Goto` block. Giúp model gọn hơn bằng cách thay thế dây nối dài.
- **Bản chất toán học:** $y = u_{goto}$.

- **Hành vi Runtime:** Simulink resolve tag tại compile time. Không overhead runtime.

> [!CODE] Code Generation
> - **Pattern:** `rtb_SignalName = source_val;` (Biến mất trong code, dùng alias trực tiếp)

---

#### 4. Data Store Read
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Signal Routing`
> - **Ứng dụng:** Đọc fault flag, operating mode từ global memory

- **Chức năng:** Đọc giá trị từ `Data Store Memory` (vùng nhớ dùng chung).
- **Bản chất toán học:** $y = DataStore[name]$.

- **Hành vi Runtime:** Đọc tại đầu mỗi time step. Cẩn thận race condition nếu có Write cùng step.

> [!CODE] Code Generation
> - **Pattern:** `val = DW.SharedVar;` (Truy cập `DWork` struct)

---

#### 5. Signal Builder
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Sources`
> - **Ứng dụng:** Test vector cho MIL test (ramp throttle, step brake)

- **Chức năng:** Tạo tín hiệu test dạng piecewise-linear phục vụ mô phỏng.
- **Hành vi Runtime:** Đọc dữ liệu từ bảng lưu trong block. Hỗ trợ nhiều scenarios.

> [!WARNING] Lưu ý Code Gen
> **Không sinh code production.** Thường được thay thế bằng `Inport` khi generate code cho ECU thực tế.

---

### Nhóm 2: Sinks (Điểm đích)

#### 6. Outport
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Sinks`
> - **Ứng dụng:** ECU output: duty cycle, torque request, fault code

- **Chức năng:** Tạo cổng đầu ra gửi tín hiệu ra cấp cha hoặc môi trường ngoài.
- **Bản chất toán học:** $y = u$.

> [!CODE] Code Generation
> - **Pattern:** `ExtY.TorqueCmd = rtb_torque;`
> - **AUTOSAR:** `Rte_IWrite_<runnable>_<port>()`

---

#### 7. Goto
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Signal Routing`
> - **Ứng dụng:** Phát vehicle speed đến nhiều controller

- **Chức năng:** Phát tín hiệu đến các `From` block cùng tag.
- **Bản chất toán học:** Truyền thẳng, không biến đổi.

> [!CODE] Code Generation
> - **Pattern:** Biến mất hoàn toàn trong code (chỉ là alias).

---

#### 8. Data Store Write
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Signal Routing`
> - **Ứng dụng:** Ghi fault flag, diagnostic result vào shared memory

- **Chức năng:** Ghi giá trị vào vùng nhớ dùng chung.
- **Bản chất toán học:** $DataStore[name] = u$.

> [!CODE] Code Generation
> - **Pattern:** `DW.SharedVar = val;`

---

#### 9. Terminator
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Sinks`
> - **Ứng dụng:** Kết thúc debug signal không dùng đến

- **Chức năng:** Đánh dấu port là 'intentionally unused' để tránh warning.
- **Code Gen:** Loại bỏ hoàn toàn.

---

### Nhóm 3: ➕ Math (Toán học)

#### 10. Add / Sum
> [!INFO] Thông tin cơ bản
> - **Thư viện:** `Simulink / Math Operations`
> - **Ứng dụng:** Tính error = setpoint - feedback trong PID

- **Chức năng:** Cộng hoặc trừ nhiều tín hiệu.
- **Toán học:** $y = \sum(\pm u_i)$.
- **Code Gen:** `y = u1 + u2;` hoặc macro `_add_sat()` cho fixed-point.

#### 11. Product
- **Toán học:** $y = \prod(u_i^{\pm 1})$.
- **Lưu ý:** Nguy cơ divide-by-zero nếu denominator = 0.

#### 12. Gain
- **Toán học:** $y = K \times u$.
- **Ứng dụng:** Unit conversion (rpm -> rad/s).
- **Code Gen:** `y = K * u;` (có thể optimize thành bit shift nếu $K = 2^n$).

#### 13. Abs
- **Toán học:** $y = |u|$.
- **Code Gen:** `fabs(u)` (double) hoặc `fabsf(u)` (float).

#### 14. Math Function
- **Các hàm:** `sqrt`, `exp`, `log`, `pow`, `sin`, `cos`...
- **Lưu ý:** Cần validate đầu vào (ví dụ $u < 0$ cho `sqrt`).

#### 15. MinMax
- **Toán học:** $y = \min(u_1, u_2, ...)$ hoặc $y = \max(u_1, u_2, ...)$.
- **Code Gen:** `(u1 < u2) ? u1 : u2`.

#### 16. Rounding Function
- **Các hàm:** `floor`, `ceil`, `round`, `fix`.

#### 17. Bias
- **Toán học:** $y = u + bias$. (Cộng hằng số offset).

---

### Nhóm 4:  Logic (Logic điều khiển)

#### 18. Logical Operator
- **Chức năng:** AND, OR, NOT, XOR, NAND, NOR, XNOR.
- **Code Gen:** `(u1 != 0) && (u2 != 0)` hoặc bitwise `u1 | u2`.

#### 19. Relational Operator
- **So sánh:** `>`, `<`, `>=`, `<=`, `==`, `!=`.
- **Lưu ý:** Với float, nên dùng $|a-b| < eps$ thay vì `==`.

#### 20. Compare to Constant
- **Ưu điểm:** Compiler có thể optimize vì so sánh với hằng số tĩnh.

#### 21. Compare to Zero
- **Ưu điểm:** Đặc biệt nhanh trong code thực thi.

#### 22. Switch
- **Cấu hình:** `y = (u2 OP threshold) ? u1 : u3`.
> [!WARNING] Cảnh báo
> **Không có short-circuit.** Cả hai nhánh $u1$ và $u3$ đều được tính toán trước khi chọn.

#### 23. Multiport Switch
- **Cấu hình:** Chọn 1 trong $N$ theo index.
- **Code Gen:** Sinh khối `switch-case`.

#### 24. If / If Action
- **Đặc điểm:** Chỉ execute nhánh đúng (khác với Switch). Phù hợp logic rẽ nhánh phức tạp.

---

### Nhóm 5: ⏱️ Timing (Thời gian & Trạng thái)

#### 25. Unit Delay ($z^{-1}$)
> [!ABSTRACT] Block quan trọng nhất
> Xương sống của mọi feedback loop và memory trong hệ rời rạc.

- **Toán học:** $y(k) = u(k-1)$.
- **Code Gen:**
  - `y = DW.UnitDelay_DSTATE;` (Output)
  - `DW.UnitDelay_DSTATE = u;` (Update)

#### 26. Delay
- **Chức năng:** Trễ $N$ bước. Dùng circular buffer nội bộ.

#### 27. Memory
- **Ứng dụng:** Tương tự Unit Delay nhưng dùng cho continuous-time (major steps).

#### 28. Detect Rise Positive
- **Logic:** `y(k) = (u(k) > 0) AND (u(k-1) <= 0)`. Xuất TRUE duy nhất 1 step.

#### 29. Detect Fall Negative
- **Logic:** `y(k) = (u(k) < 0) AND (u(k-1) >= 0)`.

#### 30. Counter Free-Running
- **Ứng dụng:** Timer, watchdog counter.

#### 31. Discrete-Time Integrator
> [!IMPORTANT] Ứng dụng Control
> Lõi của thành phần Integral (I) trong bộ điều khiển PID.

- **Các phương pháp:** Forward Euler, Backward Euler, Trapezoidal.
- **Tính năng:** Có sẵn anti-windup clamping.

---

### Nhóm 6:  Routing (Định tuyến tín hiệu)

#### 32. Mux / 33. Demux
- **Chức năng:** Ghép/Tách tín hiệu kiểu vector (array).
- **Code Gen:** Thường là zero-copy (reuse memory).

#### 34. Bus Creator / 35. Bus Selector
- **Chức năng:** Tạo/Trích xuất **Bus Object** (tương đương `struct` trong C).
- **Ứng dụng:** Nhóm hàng trăm sensor thành 1 tín hiệu Bus duy nhất để model gọn gàng.

#### 36. Selector
- **Chức năng:** Indexing array giống như `u[idx-1]`.

---

### Nhóm 7: ️ Conditioning (Xử lý tín hiệu)

#### 37. Saturation
- **Toán học:** $y = \text{clamp}(u, Lo, Hi)$.
- **Ứng dụng:** Giới hạn torque lệnh, duty cycle (0-100%).

#### 38. Dead Zone
- **Ứng dụng:** Lọc nhiễu joystick, vùng không nhạy của van.

#### 39. Rate Limiter
- **Toán học:** Giới hạn $\Delta y / \Delta t$. Giúp tín hiệu tăng/giảm mượt mà.

#### 40. 1-D / 41. 2-D Lookup Table
- **Ứng dụng:** Characteristic curve, Engine Maps ($RPM \times Load \to Torque$).
- **Code Gen:** Binary search trong ROM tables + nội suy (interpolation).

#### 42. Data Type Conversion
- **Ứng dụng:** Chuyển đổi `double` -> `float`, `float` -> `int16`. Cực kỳ quan trọng cho Fixed-point.

---

### Nhóm 8: 易 Control (Điều khiển cao cấp)

#### 43. Stateflow Chart
> [!NOTE] Ứng dụng
> Quản lý ECU Mode (Normal, Sport, Eco), Gear Shift Logic, Fault State Machine.

- **Code Gen:** Sinh `switch-case` lồng nhau và `Enum` cho states.

#### 44. MATLAB Function
- **Ứng dụng:** Thuật toán phức tạp không có sẵn block (Observer, Filter decode).

#### 45. PID Controller
- **Tính năng:** Đầy đủ P, I, D terms, Anti-windup, Filter.

#### 46. Enabled Subsystem
- **Đặc điểm:** Chỉ chạy logic bên trong khi Enable = TRUE. Giúp tiết kiệm CPU.

---

### Nhóm 9: 離 Testing (Kiểm thử)

#### 47. Assertion
- **MIL Test:** Kiểm tra signal bounds, dừng simulation nếu sai. Không sinh code.

#### 48. Scope / 49. To Workspace
- **Debug:** Visualization và lưu dữ liệu để phân tích bằng Python/Excel sau khi sim.

---

## 📌 Tra cứu nhanh (Quick Reference)

> [!TIP] Top 10 MUST-KNOW Blocks
> 1. **Unit Delay:** Feedback loop lõi.
> 2. **Switch:** Chọn chế độ đơn giản.
> 3. **Saturation:** Bảo vệ phần cứng.
> 4. **2D Lookup Table:** Bản đồ động cơ/pin.
> 5. **Stateflow Chart:** Máy trạng thái phức tạp.
> 6. **Logical Operator:** Tổng hợp lỗi.
> 7. **Relational Operator:** So sánh ngưỡng.
> 8. **Discrete Integrator:** Tích phân năng lượng/vị trí.
> 9. **Data Type Conv:** Chuẩn bị giao tiếp phần cứng.
> 10. **MATLAB Function:** Thuật toán tùy chỉnh.

---
**Xem thêm các tài liệu liên quan:**
- [[MAB Modeling Guidelines]] - Quy tắc trình bày mô hình chuẩn.
- [[Multirate and Rate Transition]] - Xử lý đa tốc độ.
