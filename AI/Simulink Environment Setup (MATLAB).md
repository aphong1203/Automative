# Simulink Environment Setup (MATLAB)

Dựa trên mẫu `script.m`, đây là quy trình chuẩn để thiết lập môi trường làm việc cho một dự án MBD (Model-Based Design).

## 1. Khởi tạo (Initialization)
Sử dụng script để:
- Clear workspace (`clear all`).
- Định nghĩa tên mô hình chính và các thành phần phụ thuộc.
- Thiết lập các đường dẫn (Path management).

```matlab
% Ví dụ thiết lập path
bpwd = pwd;
cpwd = [bpwd, '\common'];
ppwd = [bpwd, '\parts'];
addpath(genpath(cpwd));
addpath(ppwd);
```

## 2. Quản lý Data Dictionary (DD)
Tải dữ liệu định nghĩa từ Excel hoặc M-file.
- Dùng `trst_ddload` (hoặc lệnh tương đương) để nạp các file `.xls` chứa định nghĩa tín hiệu và tham số.

## 3. Cấu hình Model (Configuration)
Chạy các script cấu hình để đảm bảo Solver, Sample Time, và Code Generation settings đúng chuẩn.
```matlab
run(fullfile(cpwd, [modelCommon, '_config']));
```

## 4. Quản lý Biến thể (Variants)
Thiết lập các điều kiện Variant trước khi mở mô hình.
```matlab
Variant1 = Simulink.Variant;
Variant1.Condition = 'JVDETENTTYP == DL4POS_TYP';
```

## 5. Mở mô hình
Sử dụng `open_system` để mở mô hình tự động sau khi đã nạp đủ dữ liệu.

---
**Xem thêm:**
- [[Variants and Callbacks]]
- [[Index - Automotive Engineer Knowledge]]
