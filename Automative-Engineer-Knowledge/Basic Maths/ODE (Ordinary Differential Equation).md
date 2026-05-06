**1. [ODE (Ordinary Differential Equation)](app://obsidian.md/ODE%20\(Ordinary%20Differential%20Equation\)) - Phương trình vi phân thường**

- **Khái niệm:** Là một phương trình chứa một hoặc nhiều hàm số của một biến độc lập và đạo hàm của các hàm số đó.
    - Ví dụ: Định luật II của Newton  có thể viết lại thành  là một ODE bậc hai.
- **Tại sao quan trọng trong MBD:**
    - Hầu hết các hệ thống động học trong thế giới thực (cơ khí, điện, nhiệt, thủy lực) được mô tả bằng ODE. Chúng mô tả cách các biến của hệ thống thay đổi theo thời gian.
    - Simulink được thiết kế để giải quyết các hệ thống được mô tả bằng ODE. Khi bạn xây dựng một mô hình trong Simulink với các khối như tích phân, đạo hàm, hoặc các hệ thống con mô tả động học, thực chất bạn đang thiết lập một hệ thống ODE.
- **Simulink Solvers (ode4, ode23):**
    - **Vai trò:** ODE không thể giải quyết bằng công thức đóng (closed-form solution) cho tất cả các trường hợp phức tạp. Simulink sử dụng các "solver" (bộ giải) là các thuật toán số để ước tính nghiệm của ODE tại mỗi bước thời gian.
    - **Cách hoạt động:** Các solver này chia quá trình mô phỏng thành các bước thời gian nhỏ. Tại mỗi bước, chúng sử dụng các giá trị hiện tại của các biến và đạo hàm để ước tính giá trị của chúng ở bước tiếp theo.
    - **Ví dụ Solver:**
        - **ode4 (Runge-Kutta bậc 4):** Một thuật toán phổ biến, cân bằng giữa độ chính xác và tốc độ.
        - **ode23 (Bogacki-Shampine):** Thường là bộ giải thích nghi (adaptive solver) bậc thấp, điều chỉnh kích thước bước để đáp ứng độ chính xác yêu cầu, phù hợp cho các hệ thống có động học nhanh.
        - Simulink cung cấp nhiều solver khác nhau (ode15s, ode45, etc.), mỗi loại có ưu và nhược điểm riêng phù hợp với các loại hệ thống và yêu cầu độ chính xác khác nhau.
    - **Ứng dụng MBD:** Việc lựa chọn solver và cài đặt các tham số của nó (như bước thời gian cố định/thích nghi) là cực kỳ quan trọng trong MBD để đảm bảo kết quả mô phỏng chính xác và hiệu quả.