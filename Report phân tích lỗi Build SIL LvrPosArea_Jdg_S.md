# Report phân tích nguyên nhân lỗi Build / SIL / B2B

**Dự án:** column_lever_sbw_model (FORD), `ctrl\ctrldesign`
**Model:** `LeverPos` (top model), `LvrPosArea_Jdg_S` (model tham chiếu)
**Môi trường:** MATLAB R2023a (giao diện tiếng Nhật), AUTOSAR Blockset, `autosar.tlc`, MinGW64
**Data dictionary:** `Common_DesignData.sldd`
**Ngày:** 30/09/2026

---

## 1. Tóm tắt

| # | Lỗi | Giai đoạn | Nguyên nhân gốc | Trạng thái |
|---|---|---|---|---|
| E1 | `autosar.build.checkTopModelBuild` | Build top model `LeverPos` | Target AUTOSAR không tự tạo executable, trong khi model đang được build ở chế độ tạo file thực thi | Đã xử lý (`GenCodeOnly = on`) |
| E2 | `_sharedutils` không khớp cấu hình | Build model tham chiếu | `LeverPos` và `LvrPosArea_Jdg_S` khác cấu hình nhưng dùng chung một thư mục `slprj\autosar\_sharedutils` | Đã xử lý (đồng bộ cấu hình, xóa `slprj`) |
| E3 | Compile lỗi `xil_interface.c` (21 lỗi) | Build SIL harness | Storage class của 15 parameter không khớp với header C của team (`app_Calib.h`, `AppCommon.h`, `common_define.h`) | Đã xác định. Chạy SIL trực tiếp thì qua |
| E4 | Đã sửa trong dictionary nhưng Test Manager vẫn báo lỗi E3 | Chạy B2B bằng Test Manager | Thay đổi chưa lưu trong dictionary không được Test Manager dùng khi build | Đã xác định. Giải pháp: môi trường test lưu hẳn |
| E5 | `式が無効です…区切り記号の不一致` | Dán script vào Command Window | Dòng nối `...` bị chèn thêm dòng trống khi dán | Đã xử lý (lưu thành file `.m`) |
| E6 | `プロパティ 'DataScope' が認識されません` | Chạy script sửa storage class | CSC `Const` của package Simulink trong R2023a không có thuộc tính `DataScope` | Đã xử lý (bỏ `DataScope` / `HeaderFile`) |
| W1 | Cảnh báo AUTOSAR 3.x platform types | Generate code | Model dùng tên kiểu AUTOSAR 3.x (`sint32`, …) | Chỉ là cảnh báo, không chặn build |
| R1 | Nguy cơ `app_Calib.h` bị ghi đè | Sau khi generate code | CSC `Const` tự sinh `app_Calib.h`, và `PostCodeGenCommand` có thể copy file ra ngoài thư mục build | Cần kiểm tra |

**Kết luận chính:** Code thuật toán và bước build đều không có vấn đề. Lỗi tập trung ở bước build SIL harness, và nguyên nhân là cách khai báo dữ liệu (storage class) trong `Common_DesignData.sldd` chưa khớp với header C có sẵn của team. Khi thay đổi được áp dụng thật sự lúc build, SIL build và chạy thành công.

---

## 2. Vị trí lỗi trong luồng B2B

```
[MIL: Normal mode] ──► [Generate code (TLC)] ──► [Compile code thuật toán] ──► [Build SIL harness] ──► [Chạy SIL] ──► [So sánh MIL/SIL]
       OK                     OK                          OK                   E3 / E4 LỖI TẠI ĐÂY      chưa tới         chưa tới
```

Bằng chứng từ log (Test Manager, run 2/2):

| Giai đoạn | Log | Kết quả |
|---|---|---|
| MIL | `シミュレーション実行 1/2 が完了` | OK |
| Generate code | `Writing source file LvrPosArea_Jdg_S.c`, `TLC code generation complete` | OK |
| Compile thuật toán | `-o "LvrPosArea_Jdg_S.obj"` → `Successfully generated all binary outputs` | OK |
| Coder assumptions | `Created: ./LvrPosArea_Jdg_S_ca.lib` | OK |
| SIL harness | `xil_interface.obj` → `recipe for target 'xil_interface.obj' failed` | LỖI |
| Kết quả | `シミュレーション実行 2/2 … 実行 2 にエラーがあります` | Thất bại |

---

## 3. Phân tích chi tiết từng lỗi

### E1: `autosar.build.checkTopModelBuild`

**Triệu chứng**
Khi chạy `slbuild('LeverPos')`, MATLAB báo không build được với cấu hình AUTOSAR target hiện tại. Thông báo gợi ý 4 cách: Generate code only, Create block SIL/PIL, SIL/PIL Manager, hoặc dùng template makefile riêng.

**Nguyên nhân**
- `autosar.tlc` chỉ sinh code của SWC (runnable và lời gọi RTE). Nó không sinh hàm `main()` và không có RTE thật, nên không thể link thành file `.exe` độc lập.
- Tùy chọn `GenCodeOnly = off` yêu cầu build ra executable, mâu thuẫn với bản chất của target AUTOSAR.

**Cách xử lý**
```matlab
set_param('LeverPos','GenCodeOnly','on');
```
Muốn chạy code trên host để kiểm thử thì dùng SIL (SimulationMode = software-in-the-loop). SIL tự sinh harness và RTE stub.

**Kết quả:** Build `LeverPos` thành công (`コード生成が正常に完了: LeverPos`).

---

### E2: `_sharedutils` không khớp cấu hình

**Triệu chứng**
Khi build model tham chiếu `LvrPosArea_Jdg_S`, MATLAB báo `LvrPosArea_Jdg_S.c は存在しません`. Thông báo cho biết cấu hình của model khác với cấu hình đã dùng để sinh thư mục `build\codegen\slprj\autosar\_sharedutils`.

**Nguyên nhân**
- Top model và các model tham chiếu dùng chung một thư mục shared utilities. Simulink yêu cầu các tham số ảnh hưởng tới code dùng chung (target, word size, kiểu dữ liệu, …) phải giống nhau giữa các model.
- Trước đó `_sharedutils` được sinh với một cấu hình. Sau đó `SystemTargetFile` và `GenCodeOnly` được đổi ở top model nhưng submodel chưa đổi theo, hoặc ngược lại. Kết quả là checksum cấu hình không khớp.

**Cách xử lý**
1. Đồng bộ cấu hình cho cả hai model: `SystemTargetFile = autosar.tlc`, `GenCodeOnly = on`.
2. Kiểm tra submodel đã được map AUTOSAR (`autosar.api.create(..., 'ReferencedFromComponentModel', true)`).
3. Xóa `build\codegen\slprj` rồi build lại.

**Bài học:** Cấu hình của top model và submodel nên nằm trong cùng một Configuration Reference, để không bị lệch nhau.

---

### E3: Compile lỗi `xil_interface.c` (lỗi chính)

#### 3.1 Triệu chứng

Có 21 lỗi compile trong `...\LvrPosArea_Jdg_S_autosar_rtw\sil\xil_interface.c`, chia thành 2 nhóm.

**Nhóm A: xung đột qualifier (6 parameter calibration)**
```
xil_interface.c:44: error: conflicting type qualifiers for 'cu2g_Calib_gsm_detnt_Home_d'
    uint16_T cu2g_Calib_gsm_detnt_Home_d;
app_Calib.h:320: note: previous declaration ... was here
    extern const u2 cu2g_Calib_gsm_detnt_Home_d;
```
Các biến bị ảnh hưởng: `cu2g_Calib_gsm_detnt_Home_d`, `_Home_r`, `_d_Home`, `_r_Home`, `_max_R`, `_min_D`

**Nhóm B: trùng tên với macro `#define` (9 parameter)**
```
AppCommon.h:467: #define mcU2_ANGLE_FAULTANG ((u2)(65535U))
xil_interface.c:62: uint16_T mcU2_ANGLE_FAULTANG;      → error: expected declaration specifiers ... before numeric constant
xil_interface.c:283: &(mcU2_ANGLE_FAULTANG)            → error: lvalue required as unary '&' operand
```
Các biến bị ảnh hưởng: `mcU1_VAL00`, `mcU1_VAL01`, `mcU1_VAL05`, `mcU1_LVR_POS_AREA_UNKNOWN/UP/CENTER/DOWN`, `mcU2_ANGLE_FAULTANG`, `mcU2_ANGLE_UNKNOWNANG`

#### 3.2 Cơ chế gây lỗi

SIL cần trao đổi dữ liệu giữa Simulink (host) và code C đang chạy. Với parameter imported, tức là code có dùng nhưng không tự định nghĩa, SIL làm hai việc trong `xil_interface.c`:
1. Tự khai báo biến: `uint16_T <tên>;`
2. Lấy địa chỉ của biến để ghi giá trị vào: `&(<tên>)`

Hai việc này chỉ thực hiện được nếu tên parameter là một biến bình thường, có thể ghi. Ở đây thì không:

| Nhóm | Header thực tế | SIL sinh ra | Vì sao lỗi |
|---|---|---|---|
| A | `extern const u2 x;` | `uint16_T x;` | Cùng một biến bị khai báo vừa có `const` vừa không có |
| B | `#define x ((u2)(65535U))` | `uint16_T x;`, `&(x)` | Preprocessor thay `x` bằng hằng số, nên dòng khai báo sai cú pháp và không lấy được địa chỉ của hằng số |

Code thuật toán không lỗi vì nó chỉ đọc giá trị (`= mcU1_VAL00;`, `<= cu2g_Calib_...`). Đọc giá trị thì macro và biến const đều dùng được. Chỉ có SIL harness mới phải khai báo biến và lấy địa chỉ.

#### 3.3 Phân tích "5 Whys"

| Cấp | Câu hỏi | Trả lời |
|---|---|---|
| 1 | Vì sao SIL không chạy? | Vì `xil_interface.c` compile lỗi |
| 2 | Vì sao `xil_interface.c` lỗi? | Vì SIL tự khai báo lại 15 tên đã có sẵn trong header của team, nhưng với kiểu khác (thiếu `const`, hoặc trùng với macro) |
| 3 | Vì sao SIL tự khai báo lại? | Vì theo storage class hiện tại, code generator coi 15 tên này là dữ liệu imported, có thể ghi và cần harness cung cấp |
| 4 | Vì sao storage class lại như vậy? | Nhóm A dùng `ImportFromFile`, CSC này không có thông tin `const`. Nhóm B dùng `Auto`, nên code generator tự quyết định và xử lý chúng như biến |
| 5 | Nguyên nhân gốc | Cách khai báo dữ liệu trong `Common_DesignData.sldd` không khớp với header C của team. Trước đây chỉ build code nên chưa lộ ra. Khi chạy SIL mới phát hiện |

#### 3.4 Bằng chứng xác minh

Script kiểm tra (`check_param_sc.m`) cho kết quả:

| Parameter | Nguồn | Storage class | CSC | Nhóm |
|---|---|---|---|---|
| `cu2g_Calib_gsm_detnt_*` (6) | `Common_DesignData.sldd` | Custom | `ImportFromFile` | A |
| `mcU1_*`, `mcU2_ANGLE_*` (9) | `Common_DesignData.sldd` | `Auto` | Default | B |

#### 3.5 Cách xử lý đã được xác nhận

| Nhóm | Storage class mới | Code được sinh ra | Tác dụng với SIL |
|---|---|---|---|
| A | Custom / `Const` | `const uint16_T x = <Value>;` do model tự định nghĩa | SIL không khai báo lại biến. Không còn xung đột `const` |
| B | Custom / `ImportedDefine`, `HeaderFile` = `common_define.h` hoặc `AppCommon.h` | Dùng thẳng macro có sẵn | SIL không coi macro là biến |

**Kết quả xác nhận:** Sau khi đổi storage class, chạy trực tiếp
```matlab
set_param('LvrPosArea_Jdg_S','SimulationMode','software-in-the-loop'); sim('LvrPosArea_Jdg_S');
```
thì build và link thành công (`Created: ./LvrPosArea_Jdg_S.exe`), SIL chạy xong (`Starting / Stopping SIL simulation for component`).

---

### E4: Test Manager vẫn lỗi dù dictionary đã được sửa

**Triệu chứng**
Sau khi sửa storage class trong bộ nhớ (chưa `saveChanges`), Test Manager vẫn báo đúng 21 lỗi như cũ. Code lần này có được generate lại (`Writing source file LvrPosArea_Jdg_S.c`).

**Bằng chứng**

| Kiểm tra | Kết quả |
|---|---|
| `HasUnsavedChanges` | `1`, thay đổi chỉ có trong bộ nhớ |
| Storage class trong bộ nhớ | Đã đổi (`ImportedDefine` / `Const`) |
| Biến trùng tên trong base workspace | Không có |
| `IgnoreCustomStorageClasses` | `off` |
| `.c` do Test Manager build ra | Không có dòng `const uint16_T cu2g_... = ...;`, tức là vẫn build theo `ImportFromFile` |
| `xil_interface.c` | Giống hệt lần trước khi sửa |
| Chạy `sim()` trực tiếp với cùng trạng thái bộ nhớ | SIL build và chạy thành công, có sinh thêm `app_Calib.h` |

**Nguyên nhân**
Với cùng một trạng thái trong bộ nhớ, chạy `sim()` thì qua, chạy Test Manager thì lỗi. Vậy khi build SIL, Test Manager không dùng các thay đổi chưa lưu trong data dictionary mà đọc theo bản đã lưu trên đĩa.
Đây là kết luận rút ra từ thực nghiệm. Hành vi này chưa được đối chiếu với tài liệu chính thức của MathWorks.

**Cách xử lý**
Phải lưu hẳn thay đổi, nhưng không lưu vào file production. Vì vậy dùng môi trường test riêng (`create_sil_test_env.m`):
- Copy model và dictionary sang `ctrldesign\test_sil\` với tên `LvrPosArea_Jdg_S_Test.slx` và `Common_DesignData_Test.sldd`.
- Sửa storage class và `saveChanges` trên bản copy.
- Dùng thư mục build và cache riêng (`Simulink.fileGenControl`), không dùng chung `slprj` với production.
- Tắt `PostCodeGenCommand` trên model test.
- Trong Test Manager, tạo test case mới với System Under Test là `LvrPosArea_Jdg_S_Test`.

---

### E5: Lỗi cú pháp khi dán script

**Triệu chứng:** `式が無効です。…区切り記号の不一致をチェックしてください。`
**Nguyên nhân:** Khi dán vào Command Window, mỗi dòng bị chèn thêm một dòng trống. Dòng kết thúc bằng `...` (nối dòng) vì thế bị nối với một dòng rỗng, làm lời gọi `fprintf(` bị cụt.
**Cách xử lý:** Lưu script thành file `.m`, hoặc viết mỗi lệnh trên một dòng, không dùng `...`.

---

### E6: CSC `Const` không có thuộc tính `DataScope`

**Triệu chứng:** `クラス 'SimulinkCSC.AttribClass_Simulink_Const' のプロパティ 'DataScope' が認識されません`
**Nguyên nhân:** Trong R2023a, CSC `Const` của package Simulink không có thuộc tính `DataScope` hay `HeaderFile` riêng cho từng object. Đây là giả định sai trong hướng dẫn trước đó.
**Cách xử lý:** Chỉ đặt `StorageClass = 'Custom'` và `CustomStorageClass = 'Const'`. Model sẽ tự định nghĩa biến const.

---

### W1: Cảnh báo AUTOSAR 3.x platform types

**Nội dung:** `非推奨の AUTOSAR 3.x プラットフォーム タイプが含まれています`
**Ảnh hưởng:** Không chặn build và không liên quan tới E3.
**Nếu muốn chuyển sang tên kiểu 4.x** (cần thống nhất với team, vì tên kiểu trong ARXML và code sẽ đổi):
```matlab
arProps = autosar.api.getAUTOSARProperties('LvrPosArea_Jdg_S');
set(arProps,'XmlOptions','PlatformTypeNames','AUTOSAR4.x');
```

---

### R1: Nguy cơ ghi đè `app_Calib.h` (cần kiểm tra)

**Quan sát:** Khi dùng CSC `Const`, log có dòng `### Writing header file app_Calib.h`. Simulink đã tự sinh một file `app_Calib.h` trong thư mục build.
**Rủi ro:** `PostCodeGenCommand = LeverPos_export(modelName);` chạy sau mỗi lần generate code. Nếu script này copy file sinh ra ra ngoài thư mục build, bản `app_Calib.h` dành cho test có thể đã ghi đè lên `common\include\app_Calib.h` thật.
**Cách kiểm tra:**
```matlab
dir('D:\work\tranphong\WORKSPACE\FORD\MAIN\column_lever_sbw_model\ctrl\ctrldesign\common\include\app_Calib.h')
```
Nếu ngày giờ sửa là khoảng 18:0x ngày 30/09/2026 thì khôi phục file từ SVN/Git, rồi đọc `LeverPos_export.m` để biết script này copy những gì.

---

## 4. Những điểm hướng dẫn trước đó chưa chính xác

| Hướng dẫn | Thực tế | Tác động |
|---|---|---|
| Dùng `DefaultParameterBehavior = 'Inlined'` để sửa E3 | Tùy chọn này không đổi được cách xử lý parameter nhóm A (có storage class riêng). Ngoài ra lần chạy đó code không được generate lại (`最新の状態です`) | Mất một vòng thử |
| Dùng CSC `Const` với `DataScope = 'Imported'` | R2023a không có thuộc tính này (E6) | Phải sửa lại script |
| Sửa dictionary trong bộ nhớ rồi chạy Test Manager | Test Manager không nhận thay đổi chưa lưu (E4) | Phải chuyển sang môi trường test lưu hẳn |

---

## 5. Trạng thái hiện tại và việc cần làm

| # | Việc | Mức độ |
|---|---|---|
| 1 | Khôi phục production: `set_param('LvrPosArea_Jdg_S','SimulationMode','normal')`, `discardChanges` trên `Common_DesignData.sldd`, không lưu model | Làm ngay |
| 2 | Kiểm tra `common\include\app_Calib.h` có bị ghi đè không (R1) | Làm ngay |
| 3 | Xóa `build\codegen\LvrPosArea_Jdg_S_autosar_rtw` vì đây là build test | Làm ngay |
| 4 | Chạy `create_sil_test_env.m`, tạo test case B2B mới cho `LvrPosArea_Jdg_S_Test` | Tiếp theo |
| 5 | Đối chiếu `Value` của `cu2g_Calib_*` trong dictionary với giá trị thật trong `app_Calib.c` | Trước khi đánh giá kết quả B2B |
| 6 | Thống nhất với team về việc áp dụng cố định storage class `ImportedDefine` / `Const` cho production | Dài hạn |

---

## 6. Phòng ngừa

1. **Storage class phải khớp với header C.**
   - Tên đã là `#define` trong header: dùng `ImportedDefine` và khai báo `HeaderFile`.
   - Tên đã là `extern const`: không dùng `ImportFromFile`. Dùng CSC có `const`, hoặc CSC riêng của team có hỗ trợ `const` + imported.
   - Không để `Auto` cho những parameter trùng tên với macro.
2. **Kiểm tra SIL sớm.** Build được code chưa chắc chạy được SIL. Nên chạy thử SIL ngay khi thêm dữ liệu mới vào dictionary.
3. **Tách môi trường test và production.** Test dùng bản copy lưu hẳn (model, dictionary, thư mục build riêng) và tắt `PostCodeGenCommand`.
4. **Dùng chung cấu hình.** Dùng Configuration Reference cho top model và submodel để tránh E2.
5. **Luôn buộc generate lại code sau khi đổi dữ liệu.** Xóa `<model>_autosar_rtw` và `.slxc`, rồi kiểm tra log có dòng `Writing source file`. Nếu log báo `最新の状態です` thì thay đổi chưa được áp dụng.
6. **Script nhiều dòng nên lưu thành file `.m`**, không dán thẳng vào Command Window.

---

## Phụ lục: các lệnh chính

```matlab
% Kiem tra storage class
p = Simulink.data.evalinGlobal('LvrPosArea_Jdg_S','cu2g_Calib_gsm_detnt_Home_d'); p.CoderInfo

% Buoc generate lai code
bd = 'D:\work\tranphong\WORKSPACE\FORD\MAIN\column_lever_sbw_model\ctrl\ctrldesign\build';
rmdir(fullfile(bd,'codegen','LvrPosArea_Jdg_S_autosar_rtw'),'s');
delete(fullfile(bd,'cache','LvrPosArea_Jdg_S.slxc'));

% Chay SIL truc tiep (khong qua Test Manager)
set_param('LvrPosArea_Jdg_S','SimulationMode','software-in-the-loop'); sim('LvrPosArea_Jdg_S');

% Khoi phuc
set_param('LvrPosArea_Jdg_S','SimulationMode','normal');
discardChanges(Simulink.data.dictionary.open('Common_DesignData.sldd'));
Simulink.fileGenControl('reset');
```
