**1. Biến đổi Laplace (Laplace Transform)**

*   **Khái niệm:** Biến đổi Laplace là một công cụ toán học biến đổi một hàm của thời gian $f(t)$ (thường là các tín hiệu, phương trình vi phân) thành một hàm của biến phức $s$ trong "miền tần số phức" hay "miền Laplace" $F(s)$.
    *   Định nghĩa: $\mathcal{L}\{f(t)\} = F(s) = \int_{0}^{\infty} e^{-st} f(t) dt$
*   **Tại sao cần biết?**
    *   **Chuyển ODE thành đại số:** Đây là ưu điểm lớn nhất. Biến đổi Laplace biến các phương trình vi phân (rất khó giải trực tiếp trong miền thời gian) thành các phương trình đại số (rất dễ thao tác) trong miền $s$.
        *   Ví dụ: $\mathcal{L}\{\frac{df}{dt}\} = sF(s) - f(0)$ (với $f(0)$ là điều kiện ban đầu). Điều này biến đạo hàm thành phép nhân với $s$.
    *   **Hiểu Hàm truyền (Transfer Function):** Biến đổi Laplace là cơ sở để định nghĩa hàm truyền, cho phép chúng ta phân tích hành vi của hệ thống tuyến tính, bất biến theo thời gian (LTI) một cách dễ dàng.
    *   **Phân tích cực, zero (Poles and Zeros):** Các nghiệm của mẫu số (cực - poles) và tử số (zero - zeros) của hàm truyền trong miền $s$ cung cấp thông tin quan trọng về ổn định, đáp ứng tạm thời và đáp ứng tần số của hệ thống.
    *   **Thiết kế điều khiển dễ hơn:** Nhiều phương pháp thiết kế bộ điều khiển (như Lead-Lag compensator, thiết kế theo Root Locus hoặc Bode Plot) được thực hiện hiệu quả hơn trong miền $s$ thay vì miền thời gian.
*   **Ứng dụng trong MBD:**
    *   Khi sử dụng khối Transfer Fcn trong Simulink, bạn đang sử dụng trực tiếp các kết quả từ biến đổi Laplace.
    *   Hiểu Laplace giúp bạn cấu hình các hàm truyền, đọc và phân tích các biểu đồ tần số như Bode plot, và thiết kế các bộ điều khiển kỹ thuật số hiệu quả hơn.
**2. Transfer Function (Hàm truyền)**

*   **Khái niệm:** Hàm truyền $H(s)$ của một hệ thống LTI là tỉ số giữa biến đổi Laplace của tín hiệu đầu ra $Y(s)$ và biến đổi Laplace của tín hiệu đầu vào $U(s)$, giả sử tất cả các điều kiện ban đầu bằng 0.
    *   $H(s) = \frac{Y(s)}{U(s)}$
    *   Nó mô tả động học của hệ thống dưới dạng quan hệ đầu vào-đầu ra trong miền tần số phức.
*   **Cách hình thành từ ODE:**
    1.  Viết phương trình vi phân mô tả hệ thống.
    2.  Áp dụng biến đổi Laplace cho cả hai vế của phương trình (giả sử điều kiện ban đầu bằng 0).
    3.  Chuyển đổi các đạo hàm thành phép nhân với $s$ và tích phân thành phép chia cho $s$.
    4.  Đưa về tỉ số $\frac{Y(s)}{U(s)}$.
    *   **Ví dụ hệ bậc 1:** $\tau \frac{dy}{dt} + y = K u(t)$ $\xrightarrow{\mathcal{L}}$ $\tau s Y(s) + Y(s) = K U(s)$ $\xrightarrow{}$ $(\tau s + 1) Y(s) = K U(s)$ $\xrightarrow{}$ $H(s) = \frac{Y(s)}{U(s)} = \frac{K}{\tau s + 1}$
    *   **Ví dụ hệ bậc 2:** $\frac{d^2y}{dt^2} + 2\zeta\omega_n\frac{dy}{dt} + \omega_n^2 y = K\omega_n^2 u(t)$ $\xrightarrow{\mathcal{L}}$ $H(s) = \frac{Y(s)}{U(s)} = \frac{K\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$
*   **Phân tích cực (Poles) và zero (Zeros):**
    *   **Poles:** Là các nghiệm của mẫu số của $H(s)$. Các cực quyết định sự ổn định và đáp ứng tạm thời của hệ thống.
    *   **Zeros:** Là các nghiệm của tử số của $H(s)$. Các zero ảnh hưởng đến *hình dạng* của đáp ứng tạm thời và pha của đáp ứng tần số.
*   **Ứng dụng trong MBD:**
    *   **Simulink Transfer Fcn block:** Là khối cơ bản để xây dựng và mô phỏng các hệ thống tuyến tính dựa trên hàm truyền. Bạn nhập các hệ số của tử số và mẫu số.
    *   **Phân tích & Thiết kế:** Hàm truyền cho phép dễ dàng phân tích độ lợi (gain), pha (phase), biên độ ổn định (stability margins) qua các biểu đồ như Bode plot. Nhiều kỹ thuật thiết kế bộ điều khiển trực tiếp tác động vào các cực và zero của hàm truyền tổng thể để đạt được hiệu suất mong muốn.