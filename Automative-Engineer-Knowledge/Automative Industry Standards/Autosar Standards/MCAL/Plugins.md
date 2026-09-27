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
