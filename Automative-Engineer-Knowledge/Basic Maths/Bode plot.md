
---
### Bode Plot: Phân tích đáp ứng tần số của hệ thống

**Bode Plot** (biểu đồ Bode) là một công cụ phân tích đồ họa mạnh mẽ trong lý thuyết điều khiển và xử lý tín hiệu. Nó biểu diễn **đáp ứng tần số** của một hệ thống tuyến tính bất biến theo thời gian (LTI system) dưới dạng hai đồ thị riêng biệt:

1.  **Đồ thị biên độ (Magnitude Plot):** Biểu diễn độ lợi (gain) của hệ thống theo tần số.
2.  **Đồ thị pha (Phase Plot):** Biểu diễn độ dịch pha (phase shift) của hệ thống theo tần số.

Cả hai đồ thị này đều có trục hoành là tần số được chia theo thang logarit, cho phép dễ dàng khảo sát hành vi của hệ thống trên một dải tần số rộng.

#### 1. Đồ thị Biên độ (Magnitude Plot)

*   **Trục tung (Y-axis):** Độ lợi được đo bằng decibel (dB).
    *   Công thức: $\text{Gain (dB)} = 20 \log_{10} |H(j\omega)|$
    *   $|H(j\omega)|$ là biên độ của hàm truyền tần số, có được bằng cách thay $s = j\omega$ vào hàm truyền Laplace $H(s)$ hoặc $z = e^{j\omega T_s}$ vào hàm truyền Z $H(z)$.
    *   **Ý nghĩa:** Biểu đồ này cho bạn biết mức độ khuếch đại (hoặc suy hao) của tín hiệu đầu ra so với tín hiệu đầu vào ở mỗi tần số. Ví dụ, 0dB có nghĩa là độ lợi bằng 1 (biên độ đầu ra bằng biên độ đầu vào), 20dB có nghĩa là độ lợi bằng 10, -20dB có nghĩa là độ lợi bằng 0.1.
*   **Trục hoành (X-axis):** Tần số $(\omega)$ theo thang logarit, thường là rad/s hoặc Hz (1 decade = 10 lần tần số).

#### 2. Đồ thị Pha (Phase Plot)

*   **Trục tung (Y-axis):** Độ dịch pha được đo bằng độ (degrees) hoặc radian (rad).
    *   Công thức: $\text{Phase} = \angle H(j\omega)$
    *   **Ý nghĩa:** Biểu đồ này cho bạn biết tín hiệu đầu ra bị lệch pha bao nhiêu so với tín hiệu đầu vào ở mỗi tần số. Độ dịch pha dương có nghĩa là tín hiệu đầu ra dẫn pha (lead), độ dịch pha âm có nghĩa là tín hiệu đầu ra trễ pha (lag). Một sự dịch pha 180 độ thường rất quan trọng trong phân tích ổn định.
*   **Trục hoành (X-axis):** Tần số $(\omega)$ theo thang logarit.

### ◆ Bode Plot trong MBD và Control Theory

Bode Plot là một công cụ vô giá vì nó cung cấp cái nhìn trực quan về:

*   **Đáp ứng của hệ thống với các tần số khác nhau:** Bạn có thể dễ dàng thấy hệ thống ưu tiên truyền tải tín hiệu ở tần số nào và loại bỏ tần hiệu ở tần số nào.
*   **Tính ổn định (Stability) của hệ thống vòng kín:** Đây là một trong những ứng dụng quan trọng nhất. Từ Bode Plot, ta có thể xác định:
    *   **Gain Margin (GM):** Lượng độ lợi mà hệ thống có thể chịu thêm trước khi trở nên không ổn định (tại tần số mà pha đạt -180 độ).
    *   **Phase Margin (PM):** Lượng độ trễ pha mà hệ thống có thể chịu thêm trước khi trở nên không ổn định (tại tần số mà độ lợi đạt 0 dB).
    *   GM và PM càng lớn (và dương) thì hệ thống càng ổn định và bền vững trước nhiễu loạn.
*   **Băng thông (Bandwidth):** Tần số mà tại đó độ lợi giảm xuống 3 dB so với giá trị cực đại trong passband. Băng thông liên quan đến tốc độ đáp ứng của hệ thống.
*   **Ảnh hưởng của việc thêm cực (poles) và zero vào hệ thống:** Các cực sẽ làm giảm độ lợi và gây ra trễ pha, trong khi các zero sẽ tăng độ lợi và gây ra sớm pha. Bode Plot giúp hình dung điều này.

### Bode Plot và Thiết kế Bộ lọc (Filter Design)

Đây là nơi Bode Plot thể hiện sức mạnh vượt trội.

1.  **Bộ lọc thông thấp (Low-Pass Filter - LPF):**
    *   **Mục tiêu:** Cho phép các tần số thấp đi qua và làm suy giảm (attenuate) các tần số cao.
    *   **Trên Bode Plot:**
        *   **Đồ thị biên độ:** Bắt đầu với độ lợi cao (thường là 0 dB) ở tần số thấp, sau đó độ lợi giảm dần (roll-off) sau một tần số cắt (cut-off frequency, $\omega_c$ hoặc $f_c$). Tần số cắt thường được định nghĩa là điểm mà độ lợi giảm 3dB.
        *   **Đồ thị pha:** Pha bắt đầu ở 0 độ và dịch dần về -90 độ (cho LPF bậc 1) hoặc -180 độ (cho LPF bậc 2), v.v.
    *   **Ứng dụng:** Làm mịn tín hiệu, loại bỏ nhiễu cao tần (noise), như nhiễu cảm biến, nhiễu điện từ.

2.  **Bộ lọc chặn dải/khía (Notch Filter):**
    *   **Mục tiêu:** Làm suy giảm mạnh một dải tần số rất hẹp và cho phép các tần số khác đi qua mà không bị ảnh hưởng nhiều.
    *   **Trên Bode Plot:**
        *   **Đồ thị biên độ:** Có một "khía" (notch) rất sâu tại tần số trung tâm mà nó muốn chặn. Độ lợi giảm xuống rất thấp tại tần số đó, trong khi ở các tần số khác, độ lợi vẫn cao.
        *   **Đồ thị pha:** Có sự thay đổi pha nhanh chóng xung quanh tần số bị chặn.
    *   **Ứng dụng:** Loại bỏ các thành phần nhiễu có tần số cố định cụ thể, ví dụ như nhiễu tần số nguồn điện 50/60 Hz, hoặc loại bỏ một tần số cộng hưởng gây ra dao động không mong muốn trong một hệ thống cơ khí.

### [[Knowledge of Mathematics & Control]]

*   **Block Transfer Function:** Khi bạn định nghĩa một hệ thống bằng block "Transfer Fcn" (liên tục) hoặc "Discrete Transfer Fcn" (rời rạc) trong Simulink, bạn có thể dễ dàng sử dụng các hàm của MATLAB như `bode(sys)` để tạo biểu đồ Bode cho hệ thống đó.
*   **Phân tích & Thiết kế Bộ điều khiển:** Trong MBD, các kỹ sư sử dụng Bode Plot để:
    *   **Kiểm tra tính ổn định** của hệ thống vòng kín sau khi thiết kế bộ điều khiển.
    *   **Điều chỉnh các tham số bộ điều khiển** (ví dụ: các hệ số PID) để đạt được biên độ và pha mong muốn, từ đó đảm bảo hiệu suất và độ bền vững của hệ thống.
    *   **Xác định ảnh hưởng của các thành phần phụ trợ** như bộ lọc, bộ khuếch đại lên đáp ứng tần số tổng thể.
*   **Đánh giá sự phù hợp của thiết kế với yêu cầu:** Liệu bộ lọc có loại bỏ đủ nhiễu không? Liệu bộ điều khiển có đáp ứng đủ nhanh và ổn định không? Bode Plot cung cấp một cách trực quan để trả lời các câu hỏi này.

---

Bằng cách sử dụng Bode Plot, các kỹ sư có thể hình dung được hành vi của hệ thống ở mọi dải tần số, giúp đưa ra quyết định thiết kế sáng suốt hơn, đảm bảo hệ thống vừa ổn định vừa đáp ứng được các yêu cầu hiệu suất.