
---

## Hệ thống Rời rạc và Z-transform

### 1. Liên hệ giữa $H(z)$ và phương trình sai phân

Đây là cầu nối cốt lõi giữa lý thuyết và ứng dụng thực tế. Biến đổi Z cho phép chúng ta chuyển đổi một phương trình sai phân (trong miền thời gian rời rạc) thành một phương trình đại số (trong miền $z$), từ đó dễ dàng tìm ra hàm truyền.

**Ví dụ minh họa:**

Xét phương trình sai phân sau:
$$y[k] = -a_1 y[k-1] + b_0 u[k] + b_1 u[k-1]$$

Để chuyển đổi sang hàm truyền $H(z)$, chúng ta thực hiện biến đổi Z cho cả hai vế của phương trình. Nhớ lại tính chất dịch chuyển thời gian của biến đổi Z:
*   $Z\{y[k]\} = Y(z)$
*   $Z\{y[k-1]\} = z^{-1}Y(z)$
*   $Z\{u[k]\} = U(z)$
*   $Z\{u[k-1]\} = z^{-1}U(z)$

Áp dụng biến đổi Z vào phương trình sai phân:
$$Y(z) = -a_1 z^{-1}Y(z) + b_0 U(z) + b_1 z^{-1}U(z)$$

Bây giờ, chúng ta nhóm các số hạng chứa $Y(z)$ và $U(z)$:
$$Y(z) + a_1 z^{-1}Y(z) = b_0 U(z) + b_1 z^{-1}U(z)$$
$$Y(z)(1 + a_1 z^{-1}) = U(z)(b_0 + b_1 z^{-1})$$

Cuối cùng, chúng ta tìm hàm truyền $H(z) = \frac{Y(z)}{U(z)}$:
$$H(z) = \frac{Y(z)}{U(z)} = \frac{b_0 + b_1 z^{-1}}{1 + a_1 z^{-1}}$$

**Tầm quan trọng của mối liên hệ này:**

*   **Toán học:** Cung cấp một phương pháp hệ thống để phân tích và thiết kế hệ thống rời rạc.
*   **Block Diagram (Simulink):** Hàm truyền $H(z)$ trực tiếp tương ứng với block "Discrete Transfer Fcn". Phương trình sai phân có thể được xây dựng bằng các block "Unit Delay", "Sum" và "Gain".
*   **Code C Generated:** Khi Simulink tạo mã C, nó sẽ chuyển đổi các block này (hoặc hàm truyền) trở lại thành các phương trình sai phân để thực thi tuần tự trong bộ điều khiển (ECU). Ví dụ, phương trình sai phân trên có thể được triển khai trong C như sau:
    ```c
    // Giả sử y_prev là y[k-1] và u_prev là u[k-1]
    float y_current = -a1 * y_prev + b0 * u_current + b1 * u_prev;
    // Cập nhật giá trị cho lần lặp tiếp theo
    y_prev = y_current;
    u_prev = u_current;
    ```

### 2. Ý nghĩa hình học của mặt phẳng z

Mặt phẳng $z$ là một mặt phẳng phức tạp, nơi các cực (poles) và zero của hàm truyền $H(z)$ được vẽ. Vòng tròn đơn vị (unit circle) là một đường tròn có bán kính 1 và tâm tại gốc tọa độ trên mặt phẳng $z$.

**Ý nghĩa trực giác của vị trí cực trên mặt phẳng $z$:**

*   **Càng gần gốc ($z=0$):** Đáp ứng của hệ thống tắt càng nhanh. Điều này có nghĩa là hệ thống sẽ đạt trạng thái ổn định nhanh chóng sau một nhiễu loạn hoặc thay đổi đầu vào. Các cực tại gốc thường liên quan đến trễ thuần túy.
*   **Gần biên vòng tròn đơn vị:** Đáp ứng của hệ thống tắt chậm hơn. Hệ thống có thể có dao động kéo dài hoặc mất nhiều thời gian để ổn định.
    *   **Bên trong vòng tròn đơn vị:** Hệ thống ổn định.
    *   **Trên vòng tròn đơn vị:** Hệ thống ổn định biên (marginally stable), có thể dao động liên tục (ví dụ: bộ dao động).
*   **Nằm ngoài vòng tròn đơn vị:** Hệ thống không ổn định. Đáp ứng của hệ thống sẽ tăng dần theo thời gian, dẫn đến sự mất kiểm soát.

**Trực giác khi nhìn pole-zero map:**
Khi bạn nhìn vào một bản đồ cực-zero (pole-zero map), hãy tưởng tượng mỗi cực là một "lực hút" và mỗi zero là một "lực đẩy". Vị trí của các cực so với vòng tròn đơn vị sẽ cho bạn biết ngay về tính ổn định và tốc độ đáp ứng của hệ thống.

### 3. Liên hệ giữa cực liên tục và cực rời rạc

Mối quan hệ $z = e^{sT_s}$ là chìa khóa để hiểu cách các cực của hệ thống liên tục (trong mặt phẳng $s$) được ánh xạ sang các cực của hệ thống rời rạc (trong mặt phẳng $z$).

*   **Cực bên trái mặt phẳng $s$ sẽ map vào trong vòng tròn đơn vị:**
    *   Mặt phẳng $s$ có trục thực $\sigma$ và trục ảo $j\omega$.
    *   $s = \sigma + j\omega$.
    *   $z = e^{(\sigma + j\omega)T_s} = e^{\sigma T_s} e^{j\omega T_s}$.
    *   $|z| = |e^{\sigma T_s} e^{j\omega T_s}| = |e^{\sigma T_s}| |e^{j\omega T_s}| = e^{\sigma T_s} \cdot 1 = e^{\sigma T_s}$.
    *   Để hệ thống rời rạc ổn định, $|z| < 1$. Điều này có nghĩa là $e^{\sigma T_s} < 1$, và chỉ xảy ra khi $\sigma < 0$.
    *   Do đó, tất cả các cực nằm ở nửa mặt phẳng trái của mặt phẳng $s$ (nơi $\sigma < 0$) sẽ được ánh xạ vào bên trong vòng tròn đơn vị trên mặt phẳng $z$. Đây là điều kiện ổn định cho cả hệ liên tục và rời rạc.

*   **Cực càng âm mạnh (càng xa trục ảo về bên trái) trong mặt phẳng $s$ thì $z$ càng gần gốc ($z=0$) trong mặt phẳng $z$:**
    *   Nếu $\sigma$ rất âm (ví dụ: $\sigma \to -\infty$), thì $e^{\sigma T_s} \to 0$.
    *   Điều này có nghĩa là các cực của hệ liên tục có đáp ứng tắt rất nhanh sẽ được ánh xạ thành các cực rời rạc gần gốc tọa độ, cho thấy đáp ứng tắt nhanh tương tự trong miền rời rạc.

*   **Sample time ($T_s$) ảnh hưởng vị trí cực rời rạc:**
    *   Giá trị $T_s$ trực tiếp ảnh hưởng đến $e^{\sigma T_s}$.
    *   Với cùng một cực $s$, nếu $T_s$ thay đổi, vị trí của cực $z$ cũng sẽ thay đổi.
    *   $T_s$ lớn hơn có thể đẩy các cực $z$ ra xa gốc hơn (gần biên vòng tròn đơn vị hơn), làm cho hệ thống rời rạc có vẻ "ít ổn định" hơn hoặc đáp ứng chậm hơn so với mô hình liên tục ban đầu.

**Hiểu sâu về Discretization:**
Mối liên hệ này giúp chúng ta hiểu rằng việc rời rạc hóa không phải lúc nào cũng giữ nguyên hoàn toàn các đặc tính của hệ thống liên tục. Việc lựa chọn $T_s$ là cực kỳ quan trọng để đảm bảo rằng mô hình rời rạc vẫn phản ánh chính xác hành vi của hệ thống liên tục và duy trì tính ổn định mong muốn.

### 4. Sample time ảnh hưởng gì đến hệ rời rạc

Thời gian lấy mẫu $T_s$ (sample time) là một tham số cực kỳ quan trọng trong thiết kế hệ thống điều khiển rời rạc và Model-Based Design (MBD).

*   **$T_s$ quá lớn $\rightarrow$ mô hình rời rạc xa hệ thật hơn:**
    *   Khi $T_s$ lớn, chúng ta lấy mẫu tín hiệu ít thường xuyên hơn. Điều này có nghĩa là chúng ta bỏ lỡ nhiều thông tin về hành vi của hệ thống giữa các lần lấy mẫu.
    *   Mô hình rời rạc sẽ trở thành một xấp xỉ kém của hệ thống liên tục ban đầu, có thể dẫn đến sai lệch lớn trong đáp ứng, thậm chí gây mất ổn định cho hệ thống thực tế.
    *   Hiện tượng aliasing (nhiễu chồng) có thể xảy ra nếu tần số lấy mẫu không đủ cao so với tần số cao nhất của tín hiệu.

*   **$T_s$ quá nhỏ $\rightarrow$ tăng tải ECU:**
    *   Khi $T_s$ quá nhỏ, bộ điều khiển (ECU) phải thực hiện các phép tính và cập nhật đầu ra rất thường xuyên.
    *   Điều này làm tăng đáng kể tải tính toán lên bộ vi điều khiển, có thể dẫn đến việc không kịp thực hiện các tác vụ khác, trễ thời gian (latency) hoặc thậm chí là lỗi hệ thống.
    *   Việc sử dụng $T_s$ quá nhỏ cũng có thể làm tăng nhiễu số (numerical noise) trong một số trường hợp.

*   **Cùng một hệ nhưng chọn $T_s$ khác nhau thì pole/response discrete khác nhau:**
    *   Như đã giải thích ở mục 4, $T_s$ là một yếu tố trong công thức ánh xạ $z = e^{sT_s}$.
    *   Do đó, việc thay đổi $T_s$ sẽ làm thay đổi vị trí của các cực rời rạc trên mặt phẳng $z$.
    *   Sự thay đổi vị trí cực này sẽ dẫn đến sự thay đổi trong đáp ứng thời gian của hệ thống rời rạc (ví dụ: thời gian xác lập, độ vọt lố, tần số dao động).
    *   Trong MBD, việc lựa chọn $T_s$ thường là một quá trình cân bằng giữa độ chính xác của mô hình, hiệu suất hệ thống và tài nguyên phần cứng của ECU.

### 5. Phân biệt Z-transform với DFT/FFT

Đây là một điểm nhầm lẫn phổ biến đối với người mới bắt đầu.

*   **[[Z-transform]]:**
    *   Là một công cụ toán học để phân tích các hệ thống và tín hiệu rời rạc trong miền tần số phức ($z$-plane).
    *   Chủ yếu được sử dụng để phân tích **hệ thống động học**, giải **phương trình sai phân**, thiết kế **bộ lọc số** và nghiên cứu **tính ổn định** của hệ thống.
    *   Biến đổi một chuỗi thời gian rời rạc vô hạn (hoặc hữu hạn) thành một hàm của biến phức $z$.
    *   Không nhất thiết phải liên quan đến việc thực hiện trên máy tính theo thời gian thực.

*   **Discrete Fourier Transform (DFT) / Fast Fourier Transform (FFT):**
    *   Là các thuật toán để phân tích **phổ tần số** của một chuỗi dữ liệu rời rạc **hữu hạn**.
    *   Chủ yếu được sử dụng để phân tích **nội dung tần số** của tín hiệu, tìm các thành phần tần số chiếm ưu thế, loại bỏ nhiễu, hoặc nén dữ liệu.
    *   Biến đổi một chuỗi thời gian rời rạc hữu hạn thành một chuỗi các giá trị tần số rời rạc.
    *   FFT là một thuật toán hiệu quả để tính toán DFT.
    *   Thường được sử dụng trong xử lý tín hiệu số, phân tích âm thanh, hình ảnh, v.v.

**Tóm tắt:**
*   **Z-transform:** Dùng để **phân tích hệ thống** và **phương trình sai phân**.
*   **DFT/FFT:** Dùng để **phân tích phổ tần số** của dữ liệu.

### 6. Causality và ROC

Vùng hội tụ (Region of Convergence - ROC) là tập hợp các giá trị của $z$ mà tại đó biến đổi Z hội tụ. Mặc dù trong MBD thực hành không cần đi sâu vào toán học phức tạp của ROC, nhưng việc hiểu ý nghĩa cơ bản của nó là quan trọng.

*   **Trong control thực tế thường xét hệ nhân quả (causal systems):**
    *   Một hệ thống nhân quả là hệ thống mà đầu ra tại thời điểm hiện tại chỉ phụ thuộc vào đầu vào hiện tại và các đầu vào/đầu ra trong quá khứ, không phụ thuộc vào các đầu vào/đầu ra trong tương lai.
    *   Tất cả các hệ thống vật lý và điều khiển thực tế đều là nhân quả.

*   **ROC liên quan đến tính nhân quả và tính ổn định:**
    *   **Tính nhân quả:** Đối với một hệ thống nhân quả, ROC luôn là một vùng bên ngoài một đường tròn (ví dụ: $|z| > R$).
    *   **Tính ổn định:** Đối với một hệ thống ổn định, ROC phải bao gồm vòng tròn đơn vị ($|z|=1$).
    *   Kết hợp cả hai: Đối với một hệ thống nhân quả và ổn định, ROC phải là một vùng bên ngoài một đường tròn và bao gồm vòng tròn đơn vị. Điều này có nghĩa là tất cả các cực của hệ thống phải nằm bên trong vòng tròn đơn vị.

**Định hướng đủ dùng trong MBD:**
Khi làm việc với các hệ thống điều khiển, chúng ta thường mặc định rằng hệ thống là nhân quả. Do đó, điều kiện ổn định chính là việc tất cả các cực của hàm truyền rời rạc phải nằm **bên trong vòng tròn đơn vị** trên mặt phẳng $z$. ROC của một hệ thống nhân quả ổn định sẽ bao gồm vòng tròn đơn vị.

### 7. Nối với các block discrete cơ bản trong Simulink

Để làm cho tài liệu thực sự hữu ích cho MBD, việc liên hệ với các block Simulink là rất quan trọng.

Ngoài "Unit Delay" và "Discrete Transfer Fcn", các block rời rạc sau đây cũng rất cơ bản và thường xuyên được sử dụng:

*   **Zero-Order Hold (ZOH):**
    *   **Chức năng:** Giữ giá trị đầu vào liên tục không đổi trong suốt một chu kỳ lấy mẫu. Nó chuyển đổi tín hiệu liên tục thành tín hiệu rời rạc (sample) và sau đó giữ giá trị mẫu đó cho đến lần lấy mẫu tiếp theo.
    *   **Ứng dụng:** Thường được sử dụng để mô hình hóa cách một bộ điều khiển số (digital controller) giữ giá trị đầu ra của nó không đổi giữa các lần cập nhật.

*   **Discrete-Time Integrator:**
    *   **Chức năng:** Thực hiện phép tích phân trong miền rời rạc. Có nhiều phương pháp tích phân rời rạc (ví dụ: Forward Euler, Backward Euler, Trapezoidal).
    *   **Ứng dụng:** Rất quan trọng trong các bộ điều khiển PID số (phần I của PID), mô hình hóa các quá trình tích lũy.

*   **Memory:**
    *   **Chức năng:** Lưu trữ giá trị đầu vào từ bước thời gian trước đó và xuất ra ở bước thời gian hiện tại. Về cơ bản, nó là một Unit Delay với khả năng khởi tạo giá trị ban đầu.
    *   **Ứng dụng:** Hữu ích khi cần truy cập giá trị cũ của một tín hiệu mà không muốn sử dụng Unit Delay trực tiếp (ví dụ: trong các vòng lặp hồi tiếp phức tạp hoặc khi cần giá trị khởi tạo cụ thể).

*   **Rate Transition:**
    *   **Chức năng:** Chuyển đổi tín hiệu giữa các tốc độ lấy mẫu khác nhau trong một mô hình đa tốc độ (multirate system).
    *   **Ứng dụng:** Cực kỳ quan trọng trong các hệ thống nhúng thực tế, nơi các tác vụ khác nhau chạy ở các tần số khác nhau (ví dụ: điều khiển động cơ ở 1kHz, giao diện người dùng ở 100Hz).

*   **Difference block hoặc Sum + Delay để tự dựng difference equation:**
    *   **Difference block:** Tính toán sự khác biệt giữa giá trị hiện tại và giá trị trước đó của tín hiệu.
    *   **Sum + Delay:** Bạn có thể tự xây dựng bất kỳ phương trình sai phân nào bằng cách kết hợp các block "Sum" (cộng/trừ), "Gain" (nhân hệ số) và "Unit Delay" (trễ $z^{-1}$).
        *   Ví dụ, để tạo $y[k] = -a_1 y[k-1] + b_0 u[k] + b_1 u[k-1]$:
            *   Sử dụng Unit Delay để tạo $y[k-1]$ và $u[k-1]$.
            *   Sử dụng Gain để nhân với $-a_1$, $b_0$, $b_1$.
            *   Sử dụng Sum để cộng các thành phần lại.

**Làm cho note hợp MBD hơn:**
Việc liệt kê và giải thích các block này sẽ giúp người đọc hình dung rõ hơn cách các khái niệm lý thuyết về hệ thống rời rạc được triển khai trực tiếp trong môi trường Simulink, từ đó dễ dàng áp dụng vào các dự án MBD thực tế.

---
