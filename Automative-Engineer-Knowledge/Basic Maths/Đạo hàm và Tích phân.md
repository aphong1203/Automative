
**1. Giới hạn và tính liên tục (Foundation of Calculus)**

*   **Khái niệm giới hạn ($\lim_{x \to a} f(x)$):**
    *   Là giá trị mà hàm số $f(x)$ "tiến tới" khi biến độc lập $x$ "tiến tới" một điểm $a$ nào đó, mà không nhất thiết phải bằng giá trị của hàm tại điểm đó.
    *   **"Tiến tới" một điểm là gì?**: Có nghĩa là $x$ ngày càng gần $a$ nhưng không bao giờ (hoặc không bắt buộc) phải chạm tới $a$. Có thể tiến tới từ phía bên trái (nhỏ hơn $a$) hoặc phía bên phải (lớn hơn $a$).
    *   Ví dụ: $\lim_{x \to 0} \frac{\sin(x)}{x} = 1$, mặc dù tại $x=0$, hàm $\frac{\sin(x)}{x}$ không xác định.
*   **Hàm liên tục (Continuous function):**
    *   Một hàm $f(x)$ được gọi là liên tục tại điểm $a$ nếu:
        1.  $f(a)$ tồn tại (hàm được xác định tại $a$).
        2.  $\lim_{x \to a} f(x)$ tồn tại (giới hạn trái và giới hạn phải bằng nhau).
        3.  $\lim_{x \to a} f(x) = f(a)$ (giá trị giới hạn bằng giá trị của hàm tại điểm đó).
    *   Về mặt trực quan, đồ thị của hàm liên tục không có "lỗ", "nhảy vọt" hay "đứt quãng".
*   **Mối liên hệ với Đạo hàm:**
    *   Đạo hàm của một hàm tại một điểm được định nghĩa thông qua giới hạn:
        $\frac{df}{dx} = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}$
    *   Điều này có nghĩa là, để một hàm có đạo hàm tại một điểm, nó *phải* liên tục tại điểm đó. Tuy nhiên, hàm liên tục chưa chắc đã có đạo hàm (ví dụ: hàm giá trị tuyệt đối $y = |x|$ liên tục tại $x=0$ nhưng không có đạo hàm tại đó).
*   **Ứng dụng trong MBD:**
    *   Mặc dù ít khi trực tiếp mô hình hóa các "giới hạn", việc hiểu các khái niệm này đảm bảo rằng các công cụ Giải tích mà chúng ta sử dụng (như đạo hàm trong PID, hay việc giải ODE bằng numerical solver) có cơ sở toán học vững chắc. Nó giúp ta hiểu tại sao một số phép tính (ví dụ: chia cho 0) lại gây lỗi, hay khi nào một hàm số "khá tốt" để sử dụng trong mô hình. Nó cung cấp sự hiểu biết sâu sắc hơn về *bản chất* của các biến đổi tốc độ.

**2. Đạo hàm riêng và hàm nhiều biến (Multivariable Calculus)**

*   **Hàm nhiều biến (Functions of Multiple Variables):**
    *   Trong thực tế, các đại lượng thường phụ thuộc vào nhiều yếu tố. Ví dụ, hiệu suất động cơ phụ thuộc vào góc lái, tốc độ, tải trọng, nhiệt độ...
    *   Hàm $f(x_1, x_2, ..., x_n)$ là một hàm mà đầu ra của nó phụ thuộc vào nhiều biến độc lập $x_1, x_2, ..., x_n$.
*   **Đạo hàm riêng (Partial Derivative):**
    *   Khi có một hàm nhiều biến, đạo hàm riêng $\frac{\partial f}{\partial x_i}$ cho biết tốc độ thay đổi của hàm $f$ theo *duy nhất* một biến $x_i$, trong khi giữ các biến khác *cố định*.
    *   Ví dụ: Cho hàm nhiệt độ phòng $T(x,y,z)$, $\frac{\partial T}{\partial x}$ sẽ cho biết nhiệt độ thay đổi như thế nào khi bạn di chuyển dọc theo trục $x$ (giữ $y,z$ không đổi).
*   **Gradient ($\nabla f$):**
    *   Đối với hàm vô hướng nhiều biến $f(x_1, x_2, ..., x_n)$, gradient là một vector chứa tất cả các đạo hàm riêng:
        $\nabla f = \begin{pmatrix} \frac{\partial f}{\partial x_1} \\ \frac{\partial f}{\partial x_2} \\ \vdots \\ \frac{\partial f}{\partial x_n} \end{pmatrix}$
    *   Gradient chỉ ra hướng tăng nhanh nhất của hàm số tại một điểm, và độ lớn của nó cho biết tốc độ tăng nhanh nhất đó.
*   **Jacobian Matrix ($J$):**
    *   Đối với một hệ thống với nhiều hàm đầu ra, mỗi hàm lại phụ thuộc vào nhiều biến đầu vào (hàm vector của nhiều biến vector), Jacobian là một ma trận chứa tất cả các đạo hàm riêng của các hàm đầu ra đối với các biến đầu vào.
    *   Nếu có $m$ hàm đầu ra $y_i$ và $n$ biến đầu vào $x_j$, ma trận Jacobian $J$ sẽ có kích thước $m \times n$, với phần tử $J_{ij} = \frac{\partial y_i}{\partial x_j}$.
    *   $J = \begin{pmatrix} \frac{\partial y_1}{\partial x_1} & \frac{\partial y_1}{\partial x_2} & \cdots & \frac{\partial y_1}{\partial x_n} \\ \frac{\partial y_2}{\partial x_1} & \frac{\partial y_2}{\partial x_2} & \cdots & \frac{\partial y_2}{\partial x_n} \\ \vdots & \vdots & \ddots & \vdots \\ \frac{\partial y_m}{\partial x_1} & \frac{\partial y_m}{\partial x_2} & \cdots & \frac{\partial y_m}{\partial x_n} \end{pmatrix}$
*   **Ứng dụng trong MBD:**
    *   **State-space cho hệ phi tuyến:** Khi chuyển đổi một hệ phi tuyến về dạng không gian trạng thái tuyến tính ([[What is linearization]]), ma trận $A$, $B$, $C$, $D$ thường được tính bằng cách lấy Jacobian của các phương trình hệ thống tại một điểm làm việc.
    *   **Ước lượng (Estimation):** Các bộ lọc ước lượng trạng thái nâng cao như Extended Kalman Filter (EKF) hay Unscented Kalman Filter (UKF) sử dụng đạo hàm riêng và ma trận Jacobian để xấp xỉ hệ phi tuyến, cải thiện độ chính xác của việc ước lượng.
    *   **Tối ưu hóa (Optimization):** Các thuật toán tối ưu hóa thường sử dụng gradient để tìm hướng đi nhanh nhất tới điểm cực trị.