
---

**Giải tích & Phương trình vi phân: Nền tảng của các hệ thống động học và điều khiển**

Phần này là xương sống để hiểu cách các hệ thống vật lý hoạt động và phản ứng với các tín hiệu đầu vào theo thời gian. Trong MBD, nó giúp chúng ta xây dựng, mô phỏng và phân tích các mô hình một cách chính xác.

**1. [[Đạo hàm và Tích phân]]: Cơ sở của Bộ điều khiển PID**

*   **Đạo hàm (Derivative):**
    *   **Khái niệm:** Đo lường tốc độ thay đổi tức thời của một đại lượng theo một đại lượng khác (thường là thời gian). Về mặt hình học, đạo hàm của một hàm tại một điểm là độ dốc của tiếp tuyến với đồ thị hàm số tại điểm đó.
    *   **Ký hiệu:** $\frac{dx}{dt}$ (đạo hàm của $x$ theo thời gian $t$), hoặc $\dot{x}$.
    *   **Ý nghĩa vật lý:**
        *   Nếu $x$ là vị trí, $\dot{x}$ là vận tốc.
        *   Nếu $\dot{x}$ là vận tốc, $\ddot{x}$ (đạo hàm bậc hai) là gia tốc.
    *   **Trong điều khiển PID (D - Derivative term):** Thành phần "Đạo hàm" trong PID nhìn vào *tốc độ thay đổi của sai số*. Nó dự đoán sai số sẽ diễn biến như thế nào trong tương lai gần và cung cấp một phản ứng "phanh" hoặc "tăng tốc" để chống lại sự thay đổi đó. Điều này giúp giảm độ vọt lố (overshoot) và tăng độ ổn định, nhưng có thể nhạy cảm với nhiễu.
*   **Tích phân (Integral):**
    *   **Khái niệm:** Tìm tổng tích lũy của một đại lượng trong một khoảng thời gian nhất định (hoặc khoảng xác định khác). Về mặt hình học, tích phân xác định của một hàm trong một khoảng là diện tích dưới đồ thị hàm số trong khoảng đó.
    *   **Ký hiệu:** $\int x(t) dt$.
    *   **Ý nghĩa vật lý:**
        *   Nếu vận tốc là $v(t)$, thì $\int v(t) dt$ là tổng quãng đường di chuyển.
        *   Tích lũy nước trong bồn chứa từ dòng chảy vào.
    *   **Trong điều khiển PID (I - Integral term):** Thành phần "Tích phân" trong PID nhìn vào *sai số tích lũy theo thời gian*. Nhiệm vụ của nó là loại bỏ sai số xác lập (steady-state error) – tức là sai số nhỏ còn lại sau khi thành phần P và D đã phản ứng. Nó "nhớ" sai số trong quá khứ và tiếp tục điều chỉnh đầu ra cho đến khi sai số tích lũy về 0.
*   **Kết nối PID:**
    *   **P (Proportional - Tỷ lệ):** Phản ứng theo *sai số hiện tại*. ($e(t)$)
    *   **I (Integral - Tích phân):** Phản ứng theo *sai số tích lũy trong quá khứ*. ($\int e(t) dt$)
    *   **D (Derivative - Đạo hàm):** Phản ứng theo *tốc độ thay đổi của sai số trong tương lai*. ($\frac{de(t)}{dt}$)
    *   Bộ điều khiển PID là tổng của ba thành phần này, với mỗi thành phần được nhân với một hằng số khuếch đại (Kp, Ki, Kd) để tinh chỉnh hiệu suất.

**2. [[ODE (Ordinary Differential Equation)]] - Phương trình vi phân thường**

*   **Khái niệm:** Là một phương trình chứa một hoặc nhiều hàm số của một biến độc lập và đạo hàm của các hàm số đó.
    *   Ví dụ: Định luật II của Newton $F = ma$ có thể viết lại thành $F = m \frac{d^2x}{dt^2}$ là một ODE bậc hai.
*   **Tại sao quan trọng trong MBD:**
    *   Hầu hết các hệ thống động học trong thế giới thực (cơ khí, điện, nhiệt, thủy lực) được mô tả bằng ODE. Chúng mô tả cách các biến của hệ thống thay đổi theo thời gian.
    *   Simulink được thiết kế để giải quyết các hệ thống được mô tả bằng ODE. Khi bạn xây dựng một mô hình trong Simulink với các khối như tích phân, đạo hàm, hoặc các hệ thống con mô tả động học, thực chất bạn đang thiết lập một hệ thống ODE.
*   **Simulink Solvers (ode4, ode23):**
    *   **Vai trò:** ODE không thể giải quyết bằng công thức đóng (closed-form solution) cho tất cả các trường hợp phức tạp. Simulink sử dụng các "solver" (bộ giải) là các thuật toán số để ước tính nghiệm của ODE tại mỗi bước thời gian.
    *   **Cách hoạt động:** Các solver này chia quá trình mô phỏng thành các bước thời gian nhỏ. Tại mỗi bước, chúng sử dụng các giá trị hiện tại của các biến và đạo hàm để ước tính giá trị của chúng ở bước tiếp theo.
    *   **Ví dụ Solver:**
        *   **ode4 (Runge-Kutta bậc 4):** Một thuật toán phổ biến, cân bằng giữa độ chính xác và tốc độ.
        *   **ode23 (Bogacki-Shampine):** Thường là bộ giải thích nghi (adaptive solver) bậc thấp, điều chỉnh kích thước bước để đáp ứng độ chính xác yêu cầu, phù hợp cho các hệ thống có động học nhanh.
        *   Simulink cung cấp nhiều solver khác nhau (ode15s, ode45, etc.), mỗi loại có ưu và nhược điểm riêng phù hợp với các loại hệ thống và yêu cầu độ chính xác khác nhau.
    *   **Ứng dụng MBD:** Việc lựa chọn solver và cài đặt các tham số của nó (như bước thời gian cố định/thích nghi) là cực kỳ quan trọng trong MBD để đảm bảo kết quả mô phỏng chính xác và hiệu quả.

**3. Phương trình trạng thái bậc 1 và 2: Hiểu đặc tính hệ thống**

*   Đây là hai loại ODE cơ bản thường được dùng để phân tích và mô tả hành vi của nhiều hệ thống động học. Hiểu rõ chúng giúp bạn dự đoán cách hệ thống sẽ phản ứng và đặt ra yêu cầu hiệu suất.
*   **Phương trình trạng thái bậc 1 (First-Order System):**
    *   **Dạng tổng quát:** $\tau \frac{dy}{dt} + y = K u(t)$
        *   Trong đó: $\tau$ (tau) là hằng số thời gian (time constant), $K$ là độ lợi tĩnh (DC gain), $u(t)$ là đầu vào, $y(t)$ là đầu ra.
    *   **Đặc tính:**
        *   **Đáp ứng bước (Step Response):** Khi đầu vào $u(t)$ là một hàm bước, đầu ra $y(t)$ sẽ tăng (hoặc giảm) một cách mũ (exponentially) và dần ổn định tại một giá trị cuối cùng.
        *   **Hằng số thời gian ($\tau$):** Thời gian cần để hệ thống đạt được khoảng 63.2% giá trị cuối cùng. Một $\tau$ nhỏ hơn có nghĩa là hệ thống phản ứng nhanh hơn.
        *   **Thời gian ổn định (Settling time):** Thời gian để đầu ra đạt và duy trì trong một biên độ (thường là 2% hoặc 5%) so với giá trị cuối cùng. Đối với hệ bậc 1, thường là $3\tau$ (đến 5% biên độ) hoặc $4\tau$ (đến 2% biên độ).
        *   **Không có dao động (No Oscillation):** Đặc trưng của hệ bậc 1 là không bao giờ có dao động hoặc độ vọt lố.
    *   **Ví dụ trong MBD:** Mô hình hóa nhiệt độ của một vật thể đơn giản đang được gia nhiệt, sạc/xả của tụ điện trong mạch RC đơn giản.

*   **Phương trình trạng thái bậc 2 (Second-Order System):**
    *   **Dạng tổng quát:** $\frac{d^2y}{dt^2} + 2\zeta\omega_n\frac{dy}{dt} + \omega_n^2 y = K\omega_n^2 u(t)$
        *   Trong đó: $\omega_n$ là tần số tự nhiên không tắt dần (undamped natural frequency), $\zeta$ (zeta) là hệ số tắt dần (damping ratio), $K$ là độ lợi tĩnh.
    *   **Đặc tính quan trọng:**
        *   **Hệ số tắt dần ($\zeta$):** Xác định dạng của đáp ứng hệ thống.
            *   **$\zeta > 1$ (Overdamped - Quá tắt dần):** Phản ứng chậm, không có dao động, không có vọt lố. Giống hệ bậc 1 nhưng chậm hơn.
            *   **$\zeta = 1$ (Critically Damped - Tắt dần tới hạn):** Phản ứng nhanh nhất mà không có dao động.
            *   **$0 < \zeta < 1$ (Underdamped - Dưới tắt dần):** Phản ứng có **dao động** và **độ vọt lố** (overshoot) trước khi ổn định. Đây là trường hợp phổ biến trong nhiều hệ thống vật lý như hệ thống cơ học có quán tính và đàn hồi.
            *   **$\zeta = 0$ (Undamped - Không tắt dần):** Hệ thống sẽ dao động liên tục với tần số tự nhiên.
        *   **Đáp ứng bước (Step Response):** Phân tích đáp ứng bước của hệ bậc 2 (đặc biệt là dưới tắt dần) rất quan trọng:
            *   **Độ vọt lố (Overshoot):** Giá trị cực đại mà đầu ra vượt quá giá trị cuối cùng, tính bằng phần trăm.
            *   **Thời gian lên (Rise Time):** Thời gian để đầu ra tăng từ 10% đến 90% (hoặc 0% đến 100%) của giá trị cuối cùng.
            *   **Thời gian đạt đỉnh (Peak Time):** Thời gian để đạt được độ vọt lố đầu tiên.
            *   **Thời gian ổn định (Settling Time):** Thời gian để đầu ra duy trì trong một biên độ xác định (ví dụ 2% hoặc 5%) so với giá trị cuối cùng.
    *   **Ví dụ trong MBD:** Mô hình hóa hệ thống lò xo-giảm chấn-khối lượng (mass-spring-damper), hệ thống điều khiển vị trí của động cơ điện DC, hoặc dao động của cánh máy bay.

**Ứng dụng trong MBD:**

*   Khi phát triển mô hình, việc nhận diện hệ thống bạn đang mô hình hóa là bậc 1 hay bậc 2 (hoặc cao hơn) là bước đầu tiên để hiểu hành vi của nó.
*   Bạn sẽ sử dụng các khái niệm về hằng số thời gian, hệ số tắt dần, tần số tự nhiên để định lượng các yêu cầu hiệu suất (ví dụ: cần thời gian ổn định nhanh 0.5s, độ vọt lố không quá 5%).
*   Simulink sẽ giải các ODE này để cho bạn thấy đáp ứng thời gian của hệ thống, giúp bạn điều chỉnh các tham số thiết kế hoặc bộ điều khiển để đáp ứng các tiêu chí đó.

Tóm lại, giải tích và các phương trình vi phân là công cụ toán học để mô tả cách mọi thứ thay đổi và tương tác. Trong MBD, chúng cho phép bạn biến các hiện tượng vật lý thành các mô hình có thể mô phỏng và phân tích được, từ đó đưa ra quyết định thiết kế và điều khiển hiệu quả.