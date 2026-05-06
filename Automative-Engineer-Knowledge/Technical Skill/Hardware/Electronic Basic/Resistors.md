### 1. Khái niệm điện trở (R)
**Điện trở là:** sự cản trở dòng điện của một vật dẫn điện, nếu một vật dẫn điện càng tốt thì điện trở càng nhỏ, vật dẫn điện kém thì điện trở càng lớn.

 Kí hiệu : `Ohm (Ω)`
![[Pasted image 20260420143352.png | 300]]

  **◆App:** Điện trở thường được dùng với mục đích như là giảm tốc độ nạp của tụ, cung cấp dòng điện điều khiển thích hợp cho transistor BJT, bảo vệ LED hoặc các chân bán dẫn khác từ việc quá dòng, điều chỉnh hoặc hạn chế đáp ứng tần số trong mạch âm thanh (kết hợp với các thành phần khác), kéo lên hoặc kéo xuống điện áp tại các chân đầu vào của chip, hoặc điều khiển một điện áp tại một điểm trong mạch, hoặc tạo cầu phân áp.

### 2. Nguyên lý hoạt động của điện trở 
Trong quá trình cản trở dòng điện và giảm điện áp, một điện trở sẽ tiêu tán năng
lượng dưới dạng nhiệt.

Nếu R là đại lượng tính bằng ohms, I là dòng điện chạy qua điện trở tính bằng Ampe,
và V là điện áp rơi trên điện trở đó thì theo định luật ohms:

                   $V=I * R$

Đây là cách khác để nói rằng điện trở 1Ω cho phép dòng điện 1A chạy qua nó thì
điện áp rơi giữa hai đầu điện trở là 1V.

Nếu W là công suất mà điện trở tiêu tán trong mạch điện một chiều sẽ được tính bằng
công thức:

                $W=V * I=I² * R=V²/R$

### 3. Phân loại điện trở 
##### 3.1. Phân loại điện trở dựa vào tính chất của điện trở
 - **Điện trở tuyến tính:** Điện trở có trở kháng không đổi khi gia tăng sự chênh lệch điện áp.
 - **Điện trở phi tuyến tính:** Điện trở thay đổi khi có dòng điện đi qua, cụ thể tỉ lệ thuận với tỉ lệ thuận với sự chênh lệch điện áp trên đó. 
##### 3.2. Phân loại theo giá trị điện trở 
 - **Điện trở cố định :** là điện trở đã được cố định giá trị điện trở trong khi sản xuất và không thay đổi trong khi quá trình sử dụng.
 - **Biến trở hoặc chiết áp** : là điện trở có giá trị điện trở suất có thể thay đổi trong quá trình sử dụng.
##### 3.3. Phân loại theo chức năng của điện trở 
 - **Điện trở chính xác** : là điện trở có sai số thấp nhất và nó gần chính xác (gần với giá trị định danh của nó)
 - **Điện trở nóng chảy** : là một điện trở dây quấn được thiết kế dễ nóng chảy khi điện trở vượt mức cho phép. Lúc này, điện trở nóng chảy vừa là điện trở vừa là cầu chì. Khi công suất không bị vượt quá nó hoạt động như một điện trở. Và khi công suất vượt quá mức cho phép nó có chức năng như một cầu chì, nó nóng chảy và làm hở mạch để bảo vệ các thành phần khác trong mạch không bị dòng điện quá mức chạy qua.
 - **Điện trở nhiệt** : là linh kiện có giá trị điện trở thay đổi theo nhiệt độ. Do hiệu ứng tự làm nóng của dòng điện trong một điện trở nhiệt, các thiết bị tự thay đổi trở kháng với những thay đổi của dòng điện. 
    Có hai loại điện trở nhiệt: Nhiệt trở có hệ số nhiệt âm và Nhiệt trở có hệ số nhiệt dương.
 - **Điện trở quang** : là linh kiện nhạy cảm với bức xạ điện từ quanh phổ ánh sáng  nhìn thấy. Quang trở có giá trị điện trở thay đổi phụ thuộc vào cường độ ánh sáng chiều vào nó. Cường độ ánh sáng càng mạnh thì điện trở càm giảm và ngược lại.
### 4. Các cách mắc điện trở
##### 4.1. Cách mắc nối tiếp

(+) ──[R1]──[R2]──[R3]── ... ──[Rn]── (-)

- Điện trở tương đương: $R = R_1 + R_2 + \cdots + R_n$
- Cường độ dòng điện có giá trị như nhau tại mọi điểm: $I = I_1 = I_2 = \cdots = I_n$
- Hiệu điện thế giữa hai đầu mạch bằng tổng các hiệu điện thế trên mỗi trở:$U=U1+U2+⋯+UnU\cdots$
##### 4.2. Mắc song song
![[Pasted image 20260420163954.png | 300]]
* Điện trở tương đương:  $\frac{1}{R} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n}$
* Cường độ dòng điện: chạy qua mạch chính bằng tổng cường độ dòng điện chạy qua các mạch rẽ: $I = I_1 + I_2 + \cdots + I_n$.
* Hiệu điện thế giữa hai đầu đoạn mạch song song bằng hiệu điện thế giữa hai đầu mỗi đoạn mạch rẽ: $U = U_1 = U_2 = \cdots = U_n$.
### 5. Ứng dụng của điện trở
##### 5.1. Hạn chế dòng điện đi qua LED
   Để bảo vệ LED khỏi việc quá dòng thì ta cần mắc thêm một điện trở nối tiếp để dòng điện không vượt quá dòng điện cho phép của nhà sản xuất. Trong trường hợp là LED cắm (thường dùng để báo trạng thái) thì dòng điện được giới hạn trong khoảng 20mA = 0.02A.
  
![[Pasted image 20260420173504.png | 500]]
   Giả sử nguồn cấp của chúng ta là 5VDC, LED hoạt động ở điện áp 2V (tùy thuộc vào màu sắc của LED mà điện áp hoạt động khác nhau) thì ta tính giá trị điện trở theo công thức:

$$
R = \frac{(V_{cc} - V_{led})}{I} = \frac{(5 - 2)}{0.02} = 150\ \Omega
$$
   Vậy ta sẽ chọn điện trở mắc nối tiếp với LED có giá trị là 150 $\Omega$.
##### 5.2. Điện trở kéo
  Khi một công tắc cơ học hoặc một nút nhấn được gắn vào đầu vào của vi điều khiển,
  một điện trở kéo lên (*pull up*) hay điện trở kéo xuống (*pull down*) được dùng để kéo lên
  dương nguồn hoặc kéo xuống đất, và ngăn trạng thái không xác định (*floating*).
 
  ![[Pasted image 20260420174342.png | 500]]

  Ở hình bên trái biểu thị là điện trở kéo xuống đất. Khi chưa nhấn nút nhấn thì chân đầu vào của chip nhận giá trị mức thấp (0), còn khi nhấn nút thì đầu vào của chip nhận giá trị mức cao (1).

  Ngược lại ở hình bên phải là điện trở kéo lên dương nguồn. Khi chưa nhấn nút nhấn thì chân đầu vào của chip nhận giá trị mức cao (1), còn khi nhấn nút thì đầu vào của chip nhận giá trị mức thấp (0). Giá trị điện trở thường được sử dụng là 10kΩ.
##### 5.3. Mạch lọc thụ động thông thấp RC
Sự kết hợp giữa điện trở và tụ điện góp phần hạn chế tần số cao trong điều khiển âm thanh. Như hình bên dưới thì một tín hiệu đi từ A tới B, điện trở được mắc nối tiếp với một tụ điện thì sẽ cho các tín hiệu tần số thấp đi qua, trong khi chặn các tín hiệu tần số cao. Tín hiệu tần số cao sẽ đi qua tụ điện, vì tụ điện cung cấp cho chúng đường dẫn có trở kháng rất thấp. Đây được gọi là bộ lọc RC thông thấp.



* Hàm truyền đạt của mạch được tính như sau:

$$
K(f)=\frac{U_{ra}}{U_v}=\frac{\dfrac{1}{j2\pi fC}}{R+\dfrac{1}{j2\pi fC}}=\frac{1}{1+j2\pi fRC}
$$