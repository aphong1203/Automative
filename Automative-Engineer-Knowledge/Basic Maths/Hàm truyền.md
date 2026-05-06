#### <mark style="background: #FFF3A3A6;">Hàm truyền rời rạc tổng quát</mark>

Hàm truyền rời rạc $H(z)$ là một công cụ toán học mạnh mẽ để mô tả mối quan hệ giữa tín hiệu đầu vào và đầu ra của một hệ thống rời rạc trong miền $z$. Dạng tổng quát của hàm truyền rời rạc thường được biểu diễn dưới dạng phân thức của các đa thức theo $z^{-1}$ (trễ đơn vị):

$$H(z) = \frac{Y(z)}{U(z)} = \frac{b_0 + b_1 z^{-1} + b_2 z^{-2} + \dots + b_m z^{-m}}{1 + a_1 z^{-1} + a_2 z^{-2} + \dots + a_n z^{-n}}$$

Trong đó:
*   $Y(z)$ là biến đổi Z của tín hiệu đầu ra $y[k]$.
*   $U(z)$ là biến đổi Z của tín hiệu đầu vào $u[k]$.
*   $b_i$ là các hệ số của tử số (liên quan đến đầu vào).
*   $a_i$ là các hệ số của mẫu số (liên quan đến đầu ra).
*   $m$ là bậc của đa thức tử số.
*   $n$ là bậc của đa thức mẫu số.

**Tại sao dạng này lại quan trọng?**

*   **Discrete Transfer Fcn:** Đây chính là dạng mà bạn sẽ nhập vào block "Discrete Transfer Fcn" trong Simulink. Các hệ số $b_i$ và $a_i$ được nhập trực tiếp vào các trường "Numerator coefficients" và "Denominator coefficients".
*   **Digital Filter:** Hầu hết các bộ lọc số (digital filter) như FIR (Finite Impulse Response) và IIR (Infinite Impulse Response) đều được biểu diễn dưới dạng hàm truyền này.
*   **Code Implementation:** Khi bạn tạo mã C từ Simulink (ví dụ: với Simulink Coder), các phương trình sai phân tương ứng với hàm truyền này sẽ được chuyển đổi thành các vòng lặp hoặc cấu trúc dữ liệu để tính toán đầu ra dựa trên đầu vào và các giá trị quá khứ.