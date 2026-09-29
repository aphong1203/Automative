---
Title: Autosar
Concept: Plugin
tags:
  - BSW
---
### 1. Overview
Trong **EB tresos Studio**, _plugin_ là một module phần mềm được tích hợp vào công cụ để:

- Khai báo các tham số cấu hình ECU.
- Hiển thị giao diện cấu hình cho kỹ sư.
- Kiểm tra tính hợp lệ của cấu hình.
- Tính toán các giá trị dẫn xuất.
- Sinh mã nguồn cấu hình cho ECU.
- Sinh các file đầu ra như `.c`, `.h`, `.arxml`, tùy loại plugin.
### 2. The XMD format

![[XDMoverview | 1000]]

Trong một file XDM gồm các thành phần chính: **node** (schema node, data node), attribute và path. DataModel hỗ trợ các loại node khác nhau: 
- schema node: sẽ định các tham số cấu hình và cấu trúc của các tham số cấu hình. Xác định type, structure and attribute mặc định cho data-nodes.
- data node: Các nút dữ liệu đại diện cho các tham số cấu hình.

**Các loại Node**
-  **v:**  (variant) -- ***Schema*** định nghĩa cấu trúc module, kiểu dữ liệu, giá trị mặc định, ràng buộc. Do vendor (VD: NXP, Infineon, ST) cung cấp
- `d:` (_data_) — **Dữ liệu cấu hình thực tế**: giá trị người dùng nhập trong tool. Mỗi node `d:` tham chiếu về định nghĩa `v:` tương ứng qua thuộc tính `DEF`

###### <font color="#ff0000">Schema</font>

| Tag                                    | Fullname  | Discription                                                                                                    |
| -------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------- |
| <font color="#fac08f">**v:ctr**</font> | Container | Nhóm cố định các node con, mỗi node con có tên unique                                                          |
| **v:chc**                              | Choice    | Chỉ cho phép 1 trong các container con                                                                         |
| <font color="#fac08f">**v:lst**</font> | List      | Danh sách có thể lặp lại. Có 2 biến thể: • type rỗng → list thông thường • type="MAP" → map (tên entry unique) |
| v:var                                  | Variable  | Tham số có giá trị (INTEGER, FLOAT, STRING, BOOLEAN, ENUMERATION…)                                             |
| <font color="#ff0000">v:ref</font>     | Reference | Tham chiếu tới node khác                                                                                       |
>Các schema-node luôn có tên unique trong parent và không có value.

###### <font color="#e36c09">Data-nodes (prefix d:)</font>
| Tag                                    | Fullname  | Discription                                 |
| -------------------------------------- | --------- | ------------------------------------------- |
| **d:ctr**                              | Container | Nhóm các data-node con                      |
| **d:chc**                              | Choice    | Chứa đúng một container data-node           |
| **d:lst**                              | List      | Danh sách các entry (theo schema list)      |
| **d:var**                              | Variable  | Lưu giá trị thực của tham số                |
| <font color="#e36c09">**d:ref**</font> | Reference | Lưu đường dẫn tham chiếu tới data-node khác |
