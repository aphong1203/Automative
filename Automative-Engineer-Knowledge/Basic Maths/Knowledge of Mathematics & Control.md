Target : MBD - Model Basic Development ở mức cơ bản dùng mô hình simulink thiết kế để mô phỏng hệ thống điều khiển. Để củng cố nến tảng toán học và kiến thức lập trình embedded C/C++, hiểu rõ quy trình phát triền phần mềm.

---

**Tổng hợp Kiến thức nền Toán học & Điều khiển cho MBD**

**Tại sao cần biết?**
*   MBD thực chất là "lập trình bằng toán học".
*   Simulink biểu diễn hệ thống bằng các khối tính toán.
*   Hiểu nền tảng toán học giúp bạn biết tại sao model hoạt động và cách debug khi có lỗi.

**1. [[Đại số tuyến tính]]**
*   **Ma trận & vector:** Cần nắm vững các phép toán cơ bản (nhân, chuyển vị, nghịch đảo) để sử dụng trong mô hình không gian trạng thái (state-space model).
*   **Eigenvalue / eigenvector:** Dùng để phân tích sự ổn định của hệ thống điều khiển.
*   **Không gian trạng thái ([[State Space]]):** Biểu diễn hệ thống động học dưới dạng $x' = Ax + Bu$, $y = Cx + Du$.

**2. [[Giải tích & Phương trình vi phân]]**
*   **[[Đạo hàm, tích phân]]:** Là cơ sở của bộ điều khiển PID (P = tỉ lệ, I = tích phân, D = đạo hàm).
*   **[[ODE (Ordinary Differential Equation)]]:** Simulink solvers (như ode4, ode23) giải các phương trình vi phân thường tại mỗi bước thời gian.
*   **Phương trình trạng thái bậc 1 và 2:** Giúp hiểu các đặc tính của hệ thống như đáp ứng bước (step response), dao động (oscillation), và thời gian ổn định (settling time).

**3. Biến đổi Laplace & Z-Transform**
*   **[[Biến đổi Laplace]]:** Chuyển đổi ODE sang miền tần số để có [[Hàm truyền]] (Transfer Function) $H(s)$.
*   **[[Biến đổi Z]]:** Tương đương với Laplace nhưng dành cho hệ thống rời rạc (discrete-time), thường dùng trong embedded systems với fixed-step solver.
*   **[[Bode plot]]:** Phân tích độ lợi (gain) và pha (phase) của hệ thống, quan trọng trong thiết kế bộ lọc (low-pass, notch filter).
*   **Ứng dụng:** Simulink sử dụng khối Transfer Function, đòi hỏi người dùng phải hiểu để cấu hình chính xác.
* [[Hệ thống Rời rạc và Z-transform]]
    ・Hệ thống rời rạc là: Hệ thống sử lý tín hiệu theo từng bước thời gian rời rạc, không chạy liên tục theo thời gian thực.
    　 Trong MBD, Control in Automative sử lý theo kiểu: 
    　    1.  Đọc input theo chu kỳ
    　    2. Tính toán theo chu kỳ 
    　    3. Xuất Output theo chu kỳ
    　    ➞Discrete-time System
    ・[Z-Transform](obsidian://open?vault=Obsidian%20Vault&file=Automative%2FFoundation%20MBD%2FZ-transform) là sử dụng công cụ để biến miền thời gian rời rạc sang miền đại số để dễ phân tích hơn.

**4. Lý thuyết điều khiển cơ bản**
*   **[[Bộ điều khiển PID]]:** Hiểu cách điều chỉnh các tham số Kp, Ki, Kd để điều khiển tốc độ, vị trí, nhiệt độ.
*   **[[Vòng lặp phản hồi (Feedback loop)]]:** Phân biệt closed-loop và open-loop, hiểu tín hiệu sai số (error signal) và điểm đặt (setpoint).
*   **Ổn định hệ thống:** Nắm các tiêu chí cơ bản như Routh-Hurwitz criterion và root locus.
*   **Anti-windup cho PID:** Cần thiết khi bộ chấp hành (actuator) bị bão hòa (saturation).

**5. [[Konwledge điều khiển cho MBD]]**
   Điều khiển cho MBD là một hệ vật lý được mô tả bằng toán, mô phỏng trong Simulink, chuyển sang rời rạc để chạy trên ECU, và luôn phải kiểm tra stability của hệ
   ###### **Gồm những phần:**
    - Continuous vs Discrete
    - Sample Time / Discretization
    - Solver
    - Stability / Pole
---

Bode Plot trong Mô hình Simulink và MBD