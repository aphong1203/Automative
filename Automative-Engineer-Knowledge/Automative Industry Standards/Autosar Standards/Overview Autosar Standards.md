---
aliases: Overview
tags:
  - BSW
cssclasses:
  - Researcho note
---

<font color="#ff0000">AUTOSAR</font> (viết tắt của **AUTomotive Open System ARchitecture**) là kiến trúc hệ thống mở tiêu chuẩn toàn cầu dành cho ngành công nghiệp ô tô. Tiêu chuẩn này được phát triển thông qua sự hợp tác của các hãng xe và tập đoàn công nghệ hàng đầu thế giới.

### <font color="#ff0000">**1. Lý do ra đời của AUTOSAR**</font>

-**Thách thức ban đầu**: Trước khi có AUTOSAR, phần mềm và phần cứng (ECU) trên xe ô tô liên kết rất chặt chẽ với nhau. Mỗi khi thay đổi phần cứng, nhà phát triển buộc phải điều chỉnh lại toàn bộ phần mềm và tiến hành kiểm tra lại độ an toàn, gây tốn kém nhiều chi phí và thời gian.

> - **Giải pháp**: AUTOSAR ra đời nhằm **tách biệt phần mềm ứng dụng khỏi phần cứng**. Nhờ đó, một phần mềm có thể tái sử dụng trên nhiều dòng phần cứng khác nhau mà không cần thay đổi quá nhiều.

### 2. Sứ mệnh và Tầm nhìn

- **Sứ mệnh (Mission)**: Hợp tác toàn cầu giữa các công ty trong ngành để phát triển và thiết lập phần mềm tiêu chuẩn hóa cho hệ thống điện/điện tử (E/E).
- **Tầm nhìn (Vision)**: Trở thành tiêu chuẩn toàn cầu cho phần mềm ô tô, hỗ trợ các hệ thống di động thông minh với độ tin cậy cao, **an toàn (safety) và bảo mật (security)**.
- **Mạng lưới đối tác**: AUTOSAR quy tụ hơn 350 đối tác, bao gồm 9 thành viên sáng lập (Core Partners) cùng các nhóm Premium Partners và Development Partners.

### 3. Hai nền tảng chính (Platforms)

1. **AUTOSAR Classic Platform**: Tập trung vào các ứng dụng nhúng thời gian thực (real-time), có độ tin cậy cao và đáp ứng các tiêu chuẩn an toàn nghiêm ngặt (từ QM đến ASIL D).
2. **Adaptive Platform**: Phục vụ các hệ thống di động thông minh phức tạp hơn, hỗ trợ kết nối xe với xe (V2X), tính linh hoạt cao và khả năng cập nhật phần mềm qua mạng (OTA).

### 4. Kiến trúc phân lớp tổng quan (Classic Platform)

Kiến trúc Classic Platform được chia thành 3 tầng chính từ trên xuống dưới:

- **Application Layer (Tầng ứng dụng)**: Tầng cao nhất chứa các **Software Component (SWC)** thực thi các chức năng ô tô như phanh ABS, túi khí, trợ lái, giải trí....
- **RTE (Runtime Environment)**: Môi trường thời gian thực làm cầu nối giúp các SWC giao tiếp với nhau và giao tiếp với các tầng phần mềm cơ sở bên dưới.
- **BSW (Basic Software - Phần mềm cơ sở)**: Cung cấp dịch vụ nền tảng, được chia làm 3 lớp nhỏ:
    - _Services Layer_: Cung cấp dịch vụ hệ thống, quản lý bộ nhớ (NVRAM), chẩn đoán lỗi và truyền thông.
    - _ECU Abstraction Layer_: Trừu tượng hóa ECU, giúp che giấu vị trí phần cứng.
    - _MCAL (Microcontroller Abstraction Layer)_: Tầng thấp nhất truy cập trực tiếp vào các thanh ghi của vi điều khiển.
- **CDD (Complex Device Driver)**: Lớp đặc biệt cho phép đâm thẳng từ RTE xuống phần cứng nhằm đáp ứng các yêu cầu khắt khe về mặt thời gian thực.

---

💡 Bạn có muốn tìm hiểu sâu hơn về các lớp trong phần mềm cơ sở **BSW (Services, ECU Abstraction, MCAL)** hay cách các **SWC** giao tiếp với nhau qua **Port và Interface** không?