**1. Linearization (Tuyến tính hóa)**

*   **Hệ phi tuyến (Nonlinear Systems):**
    *   Là hệ thống mà không thể mô tả bằng phương trình tuyến tính (ví dụ: chứa các hàm như $\sin(x), x^2, \log(x)$, hoặc tích các biến trạng thái $x_1 x_2$).
    *   Đặc trưng: Không tuân theo nguyên lý chồng chất (superposition), hành vi phức tạp hơn nhiều so với hệ tuyến tính, có thể có nhiều điểm cân bằng, dao động giới hạn, hỗn loạn...
*   **Điểm cân bằng (Equilibrium Point):**
    *   Là một trạng thái mà tại đó hệ thống có thể tồn tại vĩnh viễn nếu không có tác động từ bên ngoài và các điều kiện đầu vào giữ nguyên. Tại điểm cân bằng, các đạo hàm của trạng thái đều bằng 0 ($\dot{x} = 0$).
*   **Tuyến tính hóa quanh điểm làm việc (Linearization around an Operating Point):**
    *   **Mục tiêu:** Xấp xỉ một hệ phi tuyến bằng một hệ tuyến tính đơn giản hơn xung quanh một "điểm làm việc" (operating point) hoặc điểm cân bằng cụ thể.
    *   **Cách làm:** Sử dụng khai triển Taylor bậc nhất (hay nói cách khác là Jacobian matrix) tại điểm đó. Ý tưởng là, trong một khu vực nhỏ quanh điểm đó, hành vi của hệ phi tuyến có thể được xấp xỉ khá tốt bằng một đường thẳng (hoặc mặt phẳng) tiếp tuyến.
    *   Ví dụ: Hàm $\sin(x)$ có thể được xấp xỉ bởi $x$ khi $x$ gần 0 (vì đạo hàm của $\sin(x)$ tại $x=0$ là $\cos(0)=1$).
*   **Tại sao cần tuyến tính hóa?**
    *   Các công cụ phân tích và thiết kế bộ điều khiển tuyến tính rất phát triển và dễ sử dụng.
    *   Giúp dự đoán hành vi địa phương của hệ phi tuyến, kiểm tra ổn định địa phương.
*   **Ứng dụng trong MBD:**
    *   Rất nhiều hệ thống vật lý là phi tuyến (robot, xe hơi, máy bay...). Để áp dụng các kỹ thuật điều khiển tuyến tính truyền thống (PID, LQR,...) hoặc sử dụng các khối Transfer Fcn/State-Space tuyến tính trong Simulink, người ta thường phải tuyến tính hóa hệ thống.
    *   Simulink có công cụ tự động tuyến tính hóa một mô hình xung quanh một điểm làm việc cụ thể.