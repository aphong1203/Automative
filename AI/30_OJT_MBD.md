# Bộ 30 Bài OJT Model-Based Design (MBD) cho Dự án Ô tô

Tài liệu này cung cấp 30 bài On-the-Job Training (OJT) thực tế tập trung vào Battery Management System (BMS), Powertrain và System Integration. Các bài được sắp xếp theo trình tự từ cơ bản đến chuyên gia, tuân thủ các tiêu chuẩn ngành như ISO 26262, ASPICE và AUTOSAR.

## 1. Bảng Tóm Tắt 30 Bài OJT

| ID | Tiêu đề (Short Title) | Domain | Level | Thời lượng | Vai trò mục tiêu |
|:---|:---|:---|:---|:---|:---|
| OJT-01 | BMS Cell Voltage Monitoring and Basic Filtering | BMS | Beginner | 8h | Model-Based Designer |
| OJT-02 | Basic SOC Estimation using Coulomb Counting | BMS | Beginner | 12h | Model-Based Designer |
| OJT-03 | BMS Contactors State Machine Design | BMS | Beginner | 16h | System Engineer |
| OJT-04 | Equivalent Circuit Battery Cell Modeling | BMS | Intermediate | 24h | MBSE Engineer |
| OJT-05 | BMS Overvoltage/Undervoltage Fault Detection Logic | BMS | Intermediate | 16h | Model-Based Designer |
| OJT-06 | SOC Estimation using Extended Kalman Filter | BMS | Intermediate | 32h | Integration Engineer |
| OJT-07 | Multi-cell Battery Pack Thermal Modeling | BMS | Advanced | 32h | Model-Based Designer |
| OJT-08 | BMS Passive Cell Balancing Control Logic | BMS | Advanced | 24h | Integration Engineer |
| OJT-09 | Advanced BMS Derating Strategy based on Tradeoffs | BMS | Expert | 40h | System Engineer |
| OJT-10 | BMS System-Level Safety Architecture for Overcharge | BMS | Expert | 40h | MBSE Engineer |
| OJT-11 | Simple Powertrain Inverter PWM Generation | Powertrain | Beginner | 8h | Model-Based Designer |
| OJT-12 | Powertrain Speed Controller (PID) Design | Powertrain | Beginner | 8h | Model-Based Designer |
| OJT-13 | Basic EV Powertrain Longitudinal Dynamics Model | Powertrain | Beginner | 16h | System Engineer |
| OJT-14 | Motor Torque Control Strategy for EV | Powertrain | Intermediate | 24h | Model-Based Designer |
| OJT-15 | EV Regenerative Braking Control Logic | Powertrain | Intermediate | 16h | Model-Based Designer |
| OJT-16 | Powertrain Driveline Modeling with Gearbox | Powertrain | Intermediate | 24h | MBSE Engineer |
| OJT-17 | PMSM Field Oriented Control (FOC) Implementation | Powertrain | Advanced | 40h | Model-Based Designer |
| OJT-18 | Powertrain Inverter Thermal Loss Modeling | Powertrain | Advanced | 32h | MBSE Engineer |
| OJT-19 | Torque Vectoring Logic for Dual Motor EV | Powertrain | Advanced | 40h | System Engineer |
| OJT-20 | Powertrain Fault Tolerant Control (Inverter short-circuit) | Powertrain | Expert | 40h | System Engineer |
| OJT-21 | Signal Interface Mapping between BMS and Powertrain | Integration | Beginner | 12h | MBSE Engineer |
| OJT-22 | Basic VCU Torque Coordination | Integration | Beginner | 16h | System Engineer |
| OJT-23 | BMS and Powertrain CAN Interface Communication | Integration | Intermediate | 24h | Integration Engineer |
| OJT-24 | EV Pre-charge Sequence Control Integrating BMS and Inverter | Integration | Intermediate | 24h | System Engineer |
| OJT-25 | Vehicle Cruise Control Integrating Powertrain | Integration | Advanced | 32h | Model-Based Designer |
| OJT-26 | Dynamic Torque Limiting based on BMS Discharge Limits | Integration | Advanced | 32h | Integration Engineer |
| OJT-27 | Eco-Routing Energy Consumption System-Level Simulation | Integration | Advanced | 40h | System Engineer |
| OJT-28 | Automated HIL Test Harness Generation for VCU Logic | Integration | Advanced | 32h | Integration Engineer |
| OJT-29 | Integrated Thermal Management System (BMS and Powertrain) | Integration | Expert | 48h | System Engineer |
| OJT-30 | System-level V&V for EV Sudden Acceleration Hazard | Integration | Expert | 48h | MBSE Engineer |

---

## 2. Các Ví Dụ Chi Tiết

### Ví dụ 1: Beginner BMS - Thiết kế Logic Lọc và Giám sát Điện áp Cell BMS
- **Mức độ:** Beginner
- **Vai trò:** Model-Based Designer
- **Thời lượng:** 8h
- **Tiền đề:** Simulink cơ bản, lý thuyết bộ lọc số.
- **Công cụ:** MATLAB/Simulink, Requirements Toolbox.
- **Dữ liệu:** File CSV 96 cell thực tế kèm nhiễu trắng và lỗi open-wire.
- **Nhiệm vụ:** Phân tích SRS, nạp dữ liệu, thiết kế bộ lọc Low-pass bậc 1, tạo logic Plausibility check (hở mạch), map traceability bằng Requirements Toolbox, chạy Simulation Data Inspector.
- **Sản phẩm:** Model .slx, Script .m, RTM PDF.
- **Tiêu chí:** Khử nhiễu >80%, trễ <50ms, RTM 100%.

### Ví dụ 2: Expert System Integration - System-level V&V for EV Sudden Acceleration Hazard
- **Mức độ:** Expert
- **Vai trò:** MBSE Engineer / Integration Engineer
- **Thời lượng:** 48h
- **Tiền đề:** ISO 26262 (ASIL D), SysML, E-Gas Monitoring.
- **Công cụ:** Cameo Systems Modeler, MATLAB/Simulink, Simulink Test.
- **Nhiệm vụ:** Thiết kế Safety Concept 3 lớp (E-Gas) trên SysML, tích hợp mô hình đa ECU, tiêm lỗi VCU treo, triển khai logic Plausibility lớp 2 tại Inverter, verify Safe State ngắt Torque, chạy Automated Testing bằng Simulink Test.
- **Sản phẩm:** SysML Model, Fault Injection Model, Test Suite, V&V Report.
- **Tiêu chí:** Thời gian phản hồi <200ms, kiến trúc phân tách rõ ràng, Traceability 1:1.

---

## 3. Dữ liệu Chi tiết 30 Bài (JSON)

```json
[
  {
    "id": "OJT-01",
    "title": "BMS Cell Voltage Monitoring and Basic Filtering",
    "domain": "BMS",
    "level": "Beginner",
    "target_role": "Model-Based Designer",
    "duration": "8h",
    "effort_person_days": 1.0,
    "prerequisites": ["Kiến thức Simulink cơ bản", "Lý thuyết tín hiệu số"],
    "tools": "MATLAB/Simulink R2024a; Requirements Toolbox",
    "data": "File CSV chứa dữ liệu điện áp 96 cell thực tế, có nhiễu đo lường tần số cao và 1 cell bị nhiễu open-wire.",
    "tasks": [
      "1. Phân tích tài liệu yêu cầu (System Requirements) về dải đo và sai số cho phép.",
      "2. Nạp file dữ liệu CSV vào MATLAB Workspace và tạo cấu trúc dữ liệu mô phỏng.",
      "3. Xây dựng Simulink model nhận tín hiệu đầu vào từ Inport.",
      "4. Thiết kế bộ lọc Low-pass (First-order) cho 96 kênh tín hiệu.",
      "5. Thêm logic 'Plausibility Check' để phát hiện giá trị ngoài dải (0V - 5V).",
      "6. Map các yêu cầu từ Requirements Toolbox vào các khối Simulink tương ứng.",
      "7. Chạy mô phỏng, dùng Simulation Data Inspector so sánh tín hiệu trước/sau lọc.",
      "8. Xuất báo cáo mô phỏng."
    ],
    "artifacts": "Requirements Traceability Matrix (RTM) map giữa System Requirements và Software Components.",
    "deliverables": ["Simulink model (.slx)", "Init script (.m)", "Simulation report"],
    "criteria": ["Model pass unit tests: nhiễu giảm >80%", "Traceability: 1:1 coverage", "Model compile không có warning"],
    "eval_skills": "Pass: Khử nhiễu thành công, trễ pha < 50ms. Fail: Algebraic loop hoặc sai số > 10mV.",
    "tips": ["Thử nghiệm thuật toán Moving Average để so sánh tài nguyên tính toán.", "Cấu hình Fixed-point data types."],
    "risks": ["Rủi ro: Trễ tín hiệu ảnh hưởng thời gian đáp ứng bảo vệ. Biện pháp: Phân tích tần số cắt (Cut-off frequency) tối ưu."]
  },
  {
    "id": "OJT-02",
    "title": "Basic SOC Estimation using Coulomb Counting",
    "domain": "BMS",
    "level": "Beginner",
    "target_role": "Model-Based Designer",
    "duration": "12h",
    "effort_person_days": 1.5,
    "prerequisites": ["Simulink cơ bản", "Nguyên lý pin Lithium-ion"],
    "tools": "MATLAB/Simulink R2024a",
    "data": "Dữ liệu dòng điện (Current profile) theo chu trình WLTP dài 30 phút.",
    "tasks": [
      "1. Thiết lập model với đầu vào: Dòng điện, Nhiệt độ, Trạng thái Contactors.",
      "2. Xây dựng bộ tích phân rời rạc (Discrete-Time Integrator) cho Coulomb Counting.",
      "3. Tích hợp bảng tra (Look-up Table - LUT) OCV-SOC để khởi tạo giá trị SOC ban đầu.",
      "4. Xây dựng logic reset bộ tích phân khi pin sạc đầy (dựa trên điện áp cell lớn nhất).",
      "5. Thêm giới hạn Saturation (0% - 100%) cho giá trị SOC đầu ra.",
      "6. Khởi tạo Test Harness đơn giản để cấp profile dòng điện giả lập.",
      "7. Chạy mô phỏng WLTP và vẽ biểu đồ suy giảm SOC.",
      "8. Đánh giá sai số tích lũy do nhiễu cảm biến dòng."
    ],
    "artifacts": "Software Unit Design Specification cho module SOC.",
    "deliverables": ["Model (.slx)", "Data Dictionary (.sldd)", "Unit Test Report"],
    "criteria": ["SOC khởi tạo đúng từ OCV", "SOC không vượt quá dải 0-100%", "Độ chính xác tích phân khớp với tính toán thủ công"],
    "eval_skills": "Pass: Implement đúng Coulomb Counting with reset logic. Fail: Sai lệch SOC do sai chu kỳ lấy mẫu (Sample Time).",
    "tips": ["Thêm logic bù dung lượng (Capacity compensation) theo nhiệt độ.", "Kiểm tra với các step size khác nhau."],
    "risks": ["Rủi ro: Lỗi tràn bộ nhớ (Overflow) khi tích phân thời gian dài. Biện pháp: Cấu hình Data Type phù hợp (Single/Double)."]
  },
  {
    "id": "OJT-03",
    "title": "BMS Contactors State Machine Design",
    "domain": "BMS",
    "level": "Beginner",
    "target_role": "System Engineer",
    "duration": "16h",
    "effort_person_days": 2.0,
    "prerequisites": ["Stateflow", "Kiến thức về rơ-le/contactor"],
    "tools": "MATLAB/Simulink/Stateflow R2024a",
    "data": "Các kịch bản đánh lửa (Key On), Tắt máy (Key Off), Lỗi nghiêm trọng (Crash/HV Interlock).",
    "tasks": [
      "1. Phân tích sơ đồ trạng thái hoạt động của xe (Off, Standby, Pre-charge, Main On, Error).",
      "2. Tạo Stateflow chart quản lý trạng thái của Main Positive, Main Negative, và Pre-charge contactor.",
      "3. Lập trình trình tự Pre-charge: Đóng Main Neg -> Đóng Pre-charge -> Chờ điện áp Inverter đạt 95% -> Đóng Main Pos -> Mở Pre-charge.",
      "4. Thêm logic ngắt khẩn cấp (Emergency Open) khi có tín hiệu HVIL hoặc Crash.",
      "5. Cấu hình Stateflow animation để debug trực quan.",
      "6. Tạo Signal Builder chứa các test case: Normal Start, Normal Stop, Pre-charge Timeout.",
      "7. Mô phỏng và kiểm tra State transition coverage."
    ],
    "artifacts": "State Transition Matrix, Hazard Analysis (FMEA) cho trường hợp dính Contactor.",
    "deliverables": ["Stateflow Model (.slx)", "Coverage Report", "Test scenarios"],
    "criteria": ["100% State/Transition coverage", "Thời gian Pre-charge timeout hoạt động chính xác", "Logic ngắt khẩn cấp có độ ưu tiên cao nhất"],
    "eval_skills": "Pass: Xử lý đúng trình tự đóng cắt. Fail: Xảy ra ngắn mạch do đóng sai trình tự.",
    "tips": ["Tích hợp Temporal Logic (ví dụ: `after(10, sec)`) để xử lý timeout.", "Sử dụng Simulink Test để tự động hóa."],
    "risks": ["Rủi ro: Contactor hàn dính (Welding). Biện pháp: Thêm trạng thái chẩn đoán Welding Detection."]
  },
  {
    "id": "OJT-04",
    "title": "Equivalent Circuit Battery Cell Modeling",
    "domain": "BMS",
    "level": "Intermediate",
    "target_role": "MBSE Engineer",
    "duration": "24h",
    "effort_person_days": 3.0,
    "prerequisites": ["Simscape Electrical", "SysML cơ bản", "Parameter Estimation"],
    "tools": "MATLAB/Simulink R2024a; Simscape; Cameo Systems Modeler",
    "data": "Dữ liệu HPPC (Hybrid Pulse Power Characterization) test của cell NCA 21700.",
    "tasks": [
      "1. Dùng Cameo tạo SysML Block Definition Diagram (BDD) định nghĩa cấu trúc của 1 Battery Cell (RC branches).",
      "2. Khởi tạo mô hình 2-RC Equivalent Circuit Model (ECM) bằng Simscape Electrical.",
      "3. Import dữ liệu HPPC thực tế vào Parameter Estimation Tool.",
      "4. Thiết lập bài toán tối ưu hóa để tìm các tham số R0, R1, C1, R2, C2 phụ thuộc vào SOC và T.",
      "5. Tạo Look-up tables 2D (SOC, Temp) cho các tham số đã nhận diện.",
      "6. Tích hợp LUTs trở lại mô hình Simscape.",
      "7. Xác thực mô hình bằng cách chạy mô phỏng chu trình NEDC và so sánh với dữ liệu đo.",
      "8. Cập nhật SysML Parametric Diagram với độ trễ và sai số mong muốn."
    ],
    "artifacts": "SysML BDD, SysML Parametric Diagram, Model Validation Report.",
    "deliverables": ["Cameo project (.mdzip)", "Simscape model (.slx)", "Workspace data (.mat)"],
    "criteria": ["Sai số mô phỏng RMSE điện áp < 20mV", "SysML model map 1:1 with Simscape architecture"],
    "eval_skills": "Pass: Nhận dạng tham số ECM thành công và liên kết SysML. Fail: Thuật toán tối ưu không hội tụ.",
    "tips": ["Sử dụng Design Optimization để tune PID tự động.", "Kiểm tra tính phi tuyến của R0 ở SOC thấp."],
    "risks": ["Rủi ro: Overfitting dữ liệu HPPC. Biện pháp: Validate bằng một chu trình lái xe (Drive cycle) độc lập."]
  },
  {
    "id": "OJT-05",
    "title": "BMS Overvoltage/Undervoltage Fault Detection Logic",
    "domain": "BMS",
    "level": "Intermediate",
    "target_role": "Model-Based Designer",
    "duration": "16h",
    "effort_person_days": 2.0,
    "prerequisites": ["AUTOSAR khái niệm", "Embedded Coder"],
    "tools": "MATLAB/Simulink R2024a; Embedded Coder",
    "data": "Thông số giới hạn (Thresholds) cho OV/UV kèm thời gian debounce (ví dụ: UV < 2.8V trong 2s).",
    "tasks": [
      "1. Thiết kế logic chẩn đoán OV/UV sử dụng các khối logic và Timer (Debounce).",
      "2. Triển khai phân cấp mức độ lỗi: Warning (Derating) và Critical (Mở contactor).",
      "3. Xây dựng Test Harness sẵn sàng cho HIL (tách biệt Model và Plant).",
      "4. Cấu hình Embedded Coder: System target file (ert.tlc), Hardware implementation (ARM Cortex-M).",
      "5. Áp dụng chuẩn MISRA C:2012 trong cấu hình Code Generation.",
      "6. Sinh mã C tự động từ mô hình.",
      "7. Kiểm tra báo cáo sinh mã (Code Generation Report) và Static Code Analysis (nếu có Polyspace).",
      "8. Viết Software in the Loop (SIL) test để verify mã C so với Model."
    ],
    "artifacts": "Software Requirements Specification (SRS), HIL Test Harness Architecture.",
    "deliverables": ["Mô hình chuẩn bị cho HIL (.slx)", "Mã C được sinh ra (C/H files)", "SIL Test Report"],
    "criteria": ["100% MISRA C compliant (zero violations)", "Thời gian debounce chính xác mức mili-giây", "Pass SIL testing"],
    "eval_skills": "Pass: Code C sạch, cấu trúc tốt, chạy SIL pass. Fail: Logic debounce bị reset sai cách.",
    "tips": ["Sử dụng Data Dictionary để định nghĩa Calibration Parameters (để có thể tune trên HIL)."],
    "risks": ["Rủi ro: Mã sinh ra quá cồng kềnh (RAM/ROM footprint lớn). Biện pháp: Tối ưu Code Generation Settings."]
  },
  {
    "id": "OJT-06",
    "title": "SOC Estimation using Extended Kalman Filter",
    "domain": "BMS",
    "level": "Intermediate",
    "target_role": "Integration Engineer",
    "duration": "32h",
    "effort_person_days": 4.0,
    "prerequisites": ["Đại số tuyến tính", "Lý thuyết điều khiển (Kalman Filter)"],
    "tools": "MATLAB/Simulink R2024a; Control System Toolbox",
    "data": "Dữ liệu lab tĩnh và động của pin; file cấu hình ma trận Q, R.",
    "tasks": [
      "1. Rút trích phương trình không gian trạng thái (State-space) phi tuyến từ mô hình ECM.",
      "2. Tính toán ma trận Jacobian cho thuật toán EKF.",
      "3. Xây dựng Simulink model cho EKF (sử dụng MATLAB Function block hoặc khối EKF có sẵn).",
      "4. Chèn logic kết hợp: Chạy EKF khi có dòng điện, dùng OCV khi xe đỗ (rest).",
      "5. Tuning ma trận hiệp phương sai nhiễu quá trình (Q) và nhiễu đo lường (R).",
      "6. Tạo kịch bản mô phỏng sai số khởi tạo (SOC ban đầu sai 20%).",
      "7. Đánh giá tốc độ hội tụ và độ bám sát của EKF so với giá trị thực.",
      "8. Phân tích tài nguyên tính toán (Execution time estimation)."
    ],
    "artifacts": "Algorithm Design Document cho EKF.",
    "deliverables": ["EKF Simulink Model", "Tuning script (.m)", "Algorithm Evaluation Report"],
    "criteria": ["Thuật toán hội tụ trong < 10 phút mô phỏng", "Sai số SOC ổn định < 3%", "Model có thể code gen (Fixed/Floating point)"],
    "eval_skills": "Pass: Jacobian đúng, bộ lọc hội tụ. Fail: Bộ lọc phân kỳ do ma trận P âm.",
    "tips": ["Thử nghiệm với Unscented Kalman Filter (UKF) nếu độ phi tuyến OCV quá lớn.", "Đảm bảo mã trong MATLAB Function support Code Generation."],
    "risks": ["Rủi ro: EKF phân kỳ. Biện pháp: Đảm quả tính quan sát được (Observability) của hệ thống."]
  },
  {
    "id": "OJT-07",
    "title": "Multi-cell Battery Pack Thermal Modeling",
    "domain": "BMS",
    "level": "Advanced",
    "target_role": "Model-Based Designer",
    "duration": "32h",
    "effort_person_days": 4.0,
    "prerequisites": ["Truyền nhiệt (Heat Transfer)", "Simscape Fluids/Thermal"],
    "tools": "MATLAB/Simulink/Simscape R2024a",
    "data": "Bản vẽ 3D đơn giản hóa của pack pin (12s1p) và thông số vật liệu tản nhiệt.",
    "tasks": [
      "1. Xây dựng mạng lưới nhiệt độ Simscape cho 12 cells nối tiếp.",
      "2. Mô hình hóa sinh nhiệt lượng (Heat Generation) = I^2*R + Heat_entropic.",
      "3. Thêm các khối truyền nhiệt dẫn (Conduction) giữa cell-cell và cell-cooling plate.",
      "4. Xây dựng mô hình tản nhiệt chất lỏng (Liquid Cooling) dùng Simscape Fluids cơ bản (bơm, ống dẫn).",
      "5. Viết logic điều khiển Bơm làm mát (Bang-bang hoặc PID dựa trên T_max của cell).",
      "6. Chạy mô phỏng Fast Charge (150A) và quan sát chênh lệch nhiệt độ (Delta T) giữa cell nóng nhất và lạnh nhất.",
      "7. Cân chỉnh lưu lượng bơm để giữ Delta T < 5°C.",
      "8. Phân tích tiêu thụ năng lượng của hệ thống làm mát."
    ],
    "artifacts": "Thermal Architecture Diagram, Interface Control Document (ICD) điện - nhiệt.",
    "deliverables": ["Simscape Thermal Model", "Controller Model", "Thermal Analysis Report"],
    "criteria": ["Mô phỏng phản ánh đúng quán tính nhiệt", "Delta T < 5°C trong suốt quá trình charge", "T_max < 45°C"],
    "eval_skills": "Pass: Mô hình ghép nối Điện-Nhiệt chạy ổn định. Fail: Lỗi mô phỏng do Zero-crossing ở khối chất lỏng.",
    "tips": ["Sử dụng khối 'Battery (Table-Based)' tích hợp sẵn của MathWorks để tăng tốc độ model."],
    "risks": ["Rủi ro: Thời gian chạy mô phỏng vật lý quá lâu. Biện pháp: Đơn giản hóa node nhiệt (Lumped capacitance model)."]
  },
  {
    "id": "OJT-08",
    "title": "BMS Passive Cell Balancing Control Logic",
    "domain": "BMS",
    "level": "Advanced",
    "target_role": "Integration Engineer",
    "duration": "24h",
    "effort_person_days": 3.0,
    "prerequisites": ["Thiết kế logic điều khiển", "Code Generation", "HIL Testing"],
    "tools": "MATLAB/Simulink R2024a; Stateflow; Embedded Coder",
    "data": "Cấu hình phần cứng: Điện trở xả 30 Ohm, số cell 96, ngưỡng chênh lệch 15mV.",
    "tasks": [
      "1. Thiết kế thuật toán Balancing: Xác định Cell có điện áp lớn nhất và trung bình Pack.",
      "2. Dùng Stateflow tạo máy trạng thái: Chờ (Rest) -> Kiểm tra điều kiện (SOC > 30%, xe không chạy) -> Kích hoạt ngõ ra PWM điều khiển MOSFET xả.",
      "3. Áp dụng logic quản lý nhiệt độ: Tạm dừng balance nếu nhiệt độ board mạch (NTC) quá cao.",
      "4. Bao bọc (Wrap) model thành HIL-ready architecture (Inputs/Outputs chuẩn hóa).",
      "5. Sinh mã C (Embedded Coder) tuân thủ tiêu chuẩn AUTOSAR Software Component (SWC) cơ bản.",
      "6. Khởi tạo một Test Harness cấp điện áp giả lập cho 96 cells (có độ lệch).",
      "7. Mô phỏng vòng lặp kín (Closed-loop) để thấy các điện áp hội tụ.",
      "8. Cấu hình pipeline CI/CD đơn giản (bằng script) để tự động sinh mã và báo cáo."
    ],
    "artifacts": "Software Component Description (ARXML - khái niệm), Test Harness Architecture.",
    "deliverables": ["Model HIL-ready", "C Code", "Closed-loop Simulation Report"],
    "criteria": ["Thuật toán đưa Delta V < 15mV", "Mã C sinh ra rõ ràng, có giao diện Step/Init function", "HIL test harness hoạt động"],
    "eval_skills": "Pass: Logic an toàn ngắt xả khi quá nhiệt hoạt động đúng. Fail: Thuật toán gây dao động (chuyển mạch liên tục).",
    "tips": ["Bảo đảm logic có Hysteresis để tránh bật/tắt liên tục (chattering)."],
    "risks": ["Rủi ro: Tính toán Max/Min cho 96 cell tốn nhiều CPU. Biện pháp: Tối ưu mảng (Array processing) trong Simulink."]
  },
  {
    "id": "OJT-09",
    "title": "Advanced BMS Derating Strategy based on Tradeoffs",
    "domain": "BMS",
    "level": "Expert",
    "target_role": "System Engineer",
    "duration": "40h",
    "effort_person_days": 5.0,
    "prerequisites": ["System Engineering", "Đa miền vật lý (Multi-physics)", "Tối ưu hóa đa mục tiêu"],
    "tools": "MATLAB/Simulink/Simscape R2024a; Model-Based Calibration Toolbox",
    "data": "Mô hình pin đầy đủ (Điện - Nhiệt), Giới hạn thiết kế cơ khí và hóa học của Cell.",
    "tasks": [
      "1. Xác định bài toán Trade-off: Nhiệt độ pin (Thermal) vs Dòng xả tối đa (Electrical) vs Khả năng tăng tốc xe (Control) vs Tuổi thọ pin (Safety/Degradation).",
      "2. Xây dựng bản đồ Derating 3D (Lookup Table): Dòng xả tối đa = f(SOC, T_max, T_min).",
      "3. Tích hợp mô hình suy giảm dung lượng (Aging model) cơ bản (SEI layer growth).",
      "4. Xây dựng logic VCU giả lập: Yêu cầu mô-men xoắn cao trên cao tốc.",
      "5. Chạy mô phỏng tích hợp: Xe chạy chu trình US06 liên tục.",
      "6. Khi pin đạt 45°C, logic Derating kích hoạt, giảm công suất khả dụng.",
      "7. Phân tích ảnh hưởng ở cấp độ hệ thống (System-level): Đánh giá xe bị giảm tốc độ (Driveability) so với việc giữ được an toàn nhiệt.",
      "8. Tối ưu hóa tham số Derating để cân bằng giữa cảm giác lái và an toàn."
    ],
    "artifacts": "System-level Tradeoff Study Report, Derating Interface Control Document.",
    "deliverables": ["Integrated System Model (.slx)", "Calibration Maps", "Tradeoff Analysis Report"],
    "criteria": ["Mô phỏng thể hiện rõ tương tác đa miền", "Nhiệt độ không bao giờ vượt 50°C", "Xe vẫn duy trì tốc độ tối thiểu an toàn (Limp-home)"],
    "eval_skills": "Pass: Chứng minh được System-level verification qua Drive cycle. Fail: Logic derating hoạt động giật cục làm xe rung lắc.",
    "tips": ["Dùng Rate Limiter ở đầu ra Derating để torque giảm mượt mà."],
    "risks": ["Rủi ro: Xung đột giữa Powertrain request và BMS limit. Biện pháp: Ưu tiên an toàn pin nhưng có dải buffer cho VCU xử lý."]
  },
  {
    "id": "OJT-10",
    "title": "BMS System-Level Safety Architecture for Overcharge",
    "domain": "BMS",
    "level": "Expert",
    "target_role": "MBSE Engineer",
    "duration": "40h",
    "effort_person_days": 5.0,
    "prerequisites": ["ISO 26262 (ASIL)", "SysML (Cameo/Enterprise Architect)"],
    "tools": "Cameo Systems Modeler; MATLAB/Simulink R2024a",
    "data": "Hazard Analysis and Risk Assessment (HARA) xác định Overcharge là ASIL D.",
    "tasks": [
      "1. Trên Cameo, thiết kế Safety Concept bằng SysML (Block Definition, Internal Block Diagram).",
      "2. Phân bổ Yêu cầu An toàn (Functional Safety Requirements) cho BMS Main Microcontroller và Redundant SBC (System Basis Chip).",
      "3. Xây dựng mô hình Simulink mô phỏng 2 đường dẫn cắt contactor độc lập (Primary path qua Relay driver, Secondary path qua SBC hardware pin).",
      "4. Đưa lỗi (Fault Injection): Giả lập Software bị treo (MCU Watchdog reset).",
      "5. Mô phỏng hệ thống chứng minh phần cứng Redundant vẫn cắt được Contactor khi Voltage > 4.3V.",
      "6. Liên kết (Trace) các khối Simulink ngược lại với SysML Safety Blocks.",
      "7. Viết kịch bản kiểm thử System-level Verification xác nhận vi phạm Safety Goal (Overcharge).",
      "8. Trích xuất báo cáo V-Model coverage."
    ],
    "artifacts": "SysML Safety Architecture Diagrams, Fault Tree Analysis (FTA) cơ bản, Requirements Traceability (SysML <-> Simulink).",
    "deliverables": ["SysML Model", "Simulink Fault Injection Model", "Safety Verification Report"],
    "criteria": ["Thể hiện rõ kiến trúc ASIL D (Redundancy)", "Hệ thống an toàn cắt điện trong < 50ms khi fault inject", "Traceability 100%"],
    "eval_skills": "Pass: System level verification chứng minh Safety Goal được đáp ứng. Fail: Lỗi Common Cause Failure không được xử lý.",
    "tips": ["Tạo các Variant Subsystem trong Simulink để dễ dàng bật/tắt Fault Injection."],
    "risks": ["Rủi ro: Mô hình trở nên quá phức tạp. Biện pháp: Phân tách rõ Logic layer và Hardware Abstraction layer."]
  },
  {
    "id": "OJT-11",
    "title": "Simple Powertrain Inverter PWM Generation",
    "domain": "Powertrain",
    "level": "Beginner",
    "target_role": "Model-Based Designer",
    "duration": "8h",
    "effort_person_days": 1.0,
    "prerequisites": ["Điện tử công suất cơ bản", "Simulink"],
    "tools": "MATLAB/Simulink R2024a",
    "data": "Thông số tần số chuyển mạch (Switching Frequency) = 10kHz, điện áp DC link = 400V.",
    "tasks": [
      "1. Xây dựng máy phát sóng mang tam giác (Triangle Carrier Generator) ở 10kHz.",
      "2. Tạo tín hiệu điều chế 3 pha (Sine wave 3-phase, 50Hz).",
      "3. So sánh tín hiệu Sine và sóng mang để tạo xung SPWM (Sinusoidal PWM).",
      "4. Thêm logic Dead-time (trễ thời gian) để ngăn ngắn mạch cùng nhánh biến tần.",
      "5. Sử dụng khối Simscape lý tưởng để đóng cắt Inverter 3 pha.",
      "6. Cấp nguồn tải R-L giả lập động cơ.",
      "7. Mô phỏng và dùng Scope phân tích phổ tần số (FFT) của dòng điện ngõ ra.",
      "8. Đánh giá Total Harmonic Distortion (THD)."
    ],
    "artifacts": "Thiết kế Unit: Khối PWM Generator.",
    "deliverables": ["Simulink/Simscape model", "FFT Analysis plot"],
    "criteria": ["Xung PWM tạo ra đúng tần số", "Logic Dead-time hoạt động ngăn ngừa short-circuit", "Dòng tải có dạng hình sin"],
    "eval_skills": "Pass: Hiểu và mô phỏng được Inverter cơ bản. Fail: Dead-time sai gây dòng shoot-through cực lớn.",
    "tips": ["Chạy ở chế độ mô phỏng Continuous/Discrete solver có step size nhỏ (vd: 1us) để bắt được xung PWM."],
    "risks": ["Rủi ro: Simulation cực chậm do tần số cao. Biện pháp: Tối ưu solver hoặc dùng Average Value Inverter model nếu không cần phân tích sóng."]
  },
  {
    "id": "OJT-12",
    "title": "Powertrain Speed Controller (PID) Design",
    "domain": "Powertrain",
    "level": "Beginner",
    "target_role": "Model-Based Designer",
    "duration": "8h",
    "effort_person_days": 1.0,
    "prerequisites": ["Lý thuyết điều khiển PID"],
    "tools": "MATLAB/Simulink R2024a",
    "data": "Hàm truyền động cơ DC đơn giản hoặc mô hình PT1 (First-order plus dead time).",
    "tasks": [
      "1. Thiết lập mô hình động cơ (Plant model) với đầu vào là điện áp, đầu ra là tốc độ (RPM).",
      "2. Xây dựng mạch vòng điều khiển kín (Closed-loop) với khối PID Controller.",
      "3. Thêm nhiễu tải (Load torque disturbance) dạng Step.",
      "4. Sử dụng tính năng PID Tuner trong Simulink để tự động chỉnh định Kp, Ki, Kd.",
      "5. Thêm cấu hình Anti-windup cho khâu I để ngăn quá điều chỉnh khi bão hòa điện áp.",
      "6. Khảo sát đáp ứng thời gian (Overshoot, Settling time).",
      "7. Cập nhật model để chuẩn bị sinh mã (chuyển sang Discrete time).",
      "8. Verify lại đáp ứng sau khi rời rạc hóa (Discretization)."
    ],
    "artifacts": "Báo cáo chỉnh định (Tuning Report).",
    "deliverables": ["Simulink model (.slx)", "PID tuning parameters", "Step response plots"],
    "criteria": ["Overshoot < 5%", "Thời gian xác lập < 2s", "Anti-windup hoạt động khi bão hòa"],
    "eval_skills": "Pass: Hiểu và triển khai được PID số. Fail: Rời rạc hóa sai khiến hệ thống mất ổn định.",
    "tips": ["Chuyển đổi PID từ dạng Ideal sang Parallel hoặc Dạng phương trình vi phân để dễ viết code C sau này."],
    "risks": ["Rủi ro: Chênh lệch giữa Continuous và Discrete model. Biện pháp: So sánh trực tiếp trên cùng một Scope."]
  },
  {
    "id": "OJT-13",
    "title": "Basic EV Powertrain Longitudinal Dynamics Model",
    "domain": "Powertrain",
    "level": "Beginner",
    "target_role": "System Engineer",
    "duration": "16h",
    "effort_person_days": 2.0,
    "prerequisites": ["Cơ học ô tô cơ bản", "Simulink"],
    "tools": "MATLAB/Simulink R2024a",
    "data": "Thông số xe: Khối lượng 1500kg, Hệ số cản gió (Cd) 0.3, Diện tích cản 2m2, Bán kính bánh xe 0.3m.",
    "tasks": [
      "1. Xây dựng phương trình lực cản di chuyển: F_aero (Cản không khí), F_roll (Lăn), F_grade (Độ dốc).",
      "2. Tạo subsystem 'Vehicle Body' áp dụng định luật II Newton (F = m*a) để tính gia tốc và tốc độ xe.",
      "3. Xây dựng subsystem 'Drivetrain' mô phỏng tỷ số truyền cố định (Gear ratio = 9.0) và hiệu suất cơ khí.",
      "4. Nhập đầu vào là Motor Torque (Nm), tính toán Traction Force tại bánh xe.",
      "5. Chạy mô phỏng kiểm tra tốc độ tối đa của xe (Top speed test).",
      "6. Tạo profile gia tốc từ 0-100 km/h với Max Torque cố định.",
      "7. Xác nhận thời gian tăng tốc 0-100 km/h phù hợp với thông số lý thuyết.",
      "8. Đóng gói mô hình thành Reference Model để dùng cho các dự án sau."
    ],
    "artifacts": "Vehicle Dynamics Mathematical Model Specification.",
    "deliverables": ["Vehicle Dynamics Simulink Model", "Simulation Report (0-100km/h)"],
    "criteria": ["Các lực cản vật lý tính toán chính xác", "Khớp với kết quả tính toán tay (sai số < 5%)"],
    "eval_skills": "Pass: Nắm vững hệ phương trình động lực học dọc. Fail: Quên chia/nhân tỷ số truyền ngược/xuôi.",
    "tips": ["Có thể so sánh kết quả tự build với khối 'Vehicle Body' có sẵn trong Powertrain Blockset."],
    "risks": ["Rủi ro: Lỗi dấu (+/-) giữa lực kéo và lực cản. Biện pháp: Dùng Free Body Diagram kiểm tra trước."]
  },
  {
    "id": "OJT-14",
    "title": "Motor Torque Control Strategy for EV",
    "domain": "Powertrain",
    "level": "Intermediate",
    "target_role": "Model-Based Designer",
    "duration": "24h",
    "effort_person_days": 3.0,
    "prerequisites": ["Đặc tính Torque-Speed của động cơ điện", "Embedded Coder"],
    "tools": "MATLAB/Simulink R2024a; Embedded Coder",
    "data": "Bản đồ Torque tối đa theo tốc độ (Torque Envelope LUT).",
    "tasks": [
      "1. Thiết kế logic chuyển đổi từ Pedal ga (0-100%) sang Yêu cầu Torque (Target Torque).",
      "2. Áp dụng giới hạn Torque dựa trên tốc độ động cơ hiện tại (Lookup table 1D).",
      "3. Thêm bộ lọc Rate Limiter để tránh giật xe khi đạp ga đột ngột (Driveability tuning).",
      "4. Thiết kế logic Active Damping (Chống rung động truyền lực) đơn giản dùng PID phản hồi tốc độ.",
      "5. Bao bọc hệ thống thành HIL-ready architecture (Software Component).",
      "6. Cấu hình Embedded Coder để sinh mã C (Embedded target, MISRA compliant).",
      "7. Thiết lập Test Harness với Plant Model ảo (đã làm ở OJT-13).",
      "8. Chạy SIL/MIL Back-to-Back testing để đảm bảo code sinh ra chạy giống hệt model."
    ],
    "artifacts": "Software Unit Design (Torque Coordinator), HIL Test Harness.",
    "deliverables": ["Control Model (.slx)", "Generated C/H Files", "SIL Back-to-Back Test Report"],
    "criteria": ["Torque không bao giờ vượt quá Torque Envelope", "Pass Back-to-back test", "Code generation không lỗi"],
    "eval_skills": "Pass: Chiến lược điều khiển mượt mà, code chuẩn. Fail: Rate limiter hoạt động sai khi xe lùi (Reverse).",
    "tips": ["Tạo các calibration variables cho Rate Limiter để người dùng cấu hình 'Eco' hoặc 'Sport' mode."],
    "risks": ["Rủi ro: Active Damping gây mất ổn định nếu phase lag lớn. Biện pháp: Bắt đầu với bộ lọc thông thấp (LPF) thay vì đạo hàm PID."]
  },
  {
    "id": "OJT-15",
    "title": "EV Regenerative Braking Control Logic",
    "domain": "Powertrain",
    "level": "Intermediate",
    "target_role": "Model-Based Designer",
    "duration": "16h",
    "effort_person_days": 2.0,
    "prerequisites": ["Logic điều khiển", "Phanh tái sinh"],
    "tools": "MATLAB/Simulink/Stateflow R2024a",
    "data": "Đường cong phanh lý tưởng (Ideal Braking Force Distribution ECE R13).",
    "tasks": [
      "1. Xây dựng logic phân bổ lực phanh giữa Phanh ma sát (Hydraulic) và Phanh tái sinh (Regen Motor).",
      "2. Sử dụng Stateflow để quản lý trạng thái: Acceleration, Coasting (Nhả chân ga), Braking (Đạp phanh).",
      "3. Lập trình Coasting Regen: Tạo một lượng Torque âm nhẹ (-10Nm) khi không đạp ga/phanh để giả lập phanh động cơ truyền thống.",
      "4. Lập trình Blended Braking: Ưu tiên Regen Torque trước, nếu không đủ lực hoặc tốc độ quá thấp (<10km/h), kích hoạt phanh thủy lực.",
      "5. Nhận tín hiệu (giả lập) từ BMS: Nếu SOC > 95%, tắt Regen để bảo vệ pin.",
      "6. Tích hợp mô hình với mô hình động lực học xe (OJT-13).",
      "7. Chạy chu trình phanh từ 100km/h -> 0 và quan sát sự phối hợp hai hệ thống phanh.",
      "8. Tính toán năng lượng thu hồi được (Energy Recovery) bằng tích phân."
    ],
    "artifacts": "Braking Strategy Logic Flowchart, Test Cases.",
    "deliverables": ["Regen Logic Model", "Stateflow Diagram", "Energy Recovery Analysis"],
    "criteria": ["Chuyển đổi mượt mà giữa Regen và Phanh cơ", "Tuân thủ giới hạn an toàn của BMS", "Xe dừng hẳn không bị trôi (Creep logic)"],
    "eval_skills": "Pass: Blended braking hoạt động đúng và an toàn. Fail: Torque phanh giật cục khi nhả bàn đạp.",
    "tips": ["Giảm Regen Torque từ từ về 0 khi tốc độ xe về gần 0 để tránh xe bị giật lúc dừng hẳn."],
    "risks": ["Rủi ro: Lực phanh không đủ khi Regen bị cắt đột ngột (do lỗi pin). Biện pháp: Logic dự phòng bù ngay bằng phanh thủy lực."]
  },
  {
    "id": "OJT-16",
    "title": "Powertrain Driveline Modeling with Gearbox",
    "domain": "Powertrain",
    "level": "Intermediate",
    "target_role": "MBSE Engineer",
    "duration": "24h",
    "effort_person_days": 3.0,
    "prerequisites": ["SysML (Cameo)", "Simscape Driveline"],
    "tools": "Cameo Systems Modeler; MATLAB/Simulink/Simscape R2024a",
    "data": "Thông số hệ thống truyền lực: Quán tính rotor, độ cứng trục, thông số vi sai (Differential).",
    "tasks": [
      "1. Sử dụng SysML BDD và IBD để vẽ kiến trúc Driveline (Motor -> Gearbox -> Differential -> Half-shafts -> Wheels).",
      "2. Chuyển kiến trúc SysML thành mô hình vật lý trên Simscape Driveline.",
      "3. Khai báo các thuộc tính vật lý: Quán tính (Inertia), Độ cứng xoắn (Torsional Stiffness), Hệ số cản (Damping).",
      "4. Khai báo mô hình bánh xe với mô hình trượt (Pacejka Magic Formula).",
      "5. Tạo kịch bản mô phỏng xe tăng tốc đột ngột trên đường trơn trượt (m_mu thay đổi).",
      "6. Quan sát hiện tượng dao động xoắn (Torsional vibration) trên trục bán trục (Half-shaft).",
      "7. Thiết lập yêu cầu Traceability từ SysML Requirements sang Simscape Blocks.",
      "8. Xuất báo cáo chứng minh hệ thống truyền lực chịu được Torque tối đa mà không bị gãy trục."
    ],
    "artifacts": "SysML IBD Diagram, RTM Document, Mechanical Stress Analysis Report.",
    "deliverables": ["SysML Project", "Simscape Driveline Model", "Simulation Traceability Report"],
    "criteria": ["Mô hình Simscape khớp 100% với kiến trúc SysML", "Có khả năng mô phỏng hiện tượng trượt bánh"],
    "eval_skills": "Pass: Mô hình vật lý chi tiết, MBSE trace đầy đủ. Fail: Gán sai đơn vị (rad/s vs RPM) gây sai kết quả.",
    "tips": ["Tạo thư viện tùy chỉnh (Custom Library) trong SysML/Simulink để tái sử dụng component."],
    "risks": ["Rủi ro: Mô hình quá cứng (Stiff system) làm solver bị nghẽn. Biện pháp: Dùng biến đổi độ cứng (Stiffness relaxation) cho Simscape."]
  },
  {
    "id": "OJT-17",
    "title": "PMSM Field Oriented Control (FOC) Implementation",
    "domain": "Powertrain",
    "level": "Advanced",
    "target_role": "Model-Based Designer",
    "duration": "40h",
    "effort_person_days": 5.0,
    "prerequisites": ["FOC Theory (Clarke/Park Transform)", "Embedded Coder", "HIL"],
    "tools": "MATLAB/Simulink R2024a; Motor Control Blockset; Embedded Coder",
    "data": "Thông số động cơ PMSM: Rs, Ld, Lq, Flux linkage, số cặp cực.",
    "tasks": [
      "1. Xây dựng khối biến đổi Clarke và Park (Alpha/Beta sang d/q).",
      "2. Xây dựng 2 mạch vòng điều khiển dòng PI cho Id (dòng từ thông) và Iq (dòng mô-men).",
      "3. Tích hợp thuật toán điều chế Space Vector PWM (SVPWM).",
      "4. Thêm logic Field Weakening (Suy giảm từ thông) để vận hành ở tốc độ cao (trên tốc độ cơ sở).",
      "5. Ghép nối Plant Model (PMSM từ Simscape hoặc khối có sẵn) để chạy vòng kín (Closed-loop MIL).",
      "6. Chỉnh định (Tune) các bộ điều khiển PI dòng điện để đạt băng thông 1kHz.",
      "7. Bao bọc Software Component và cấu hình sinh mã C Embedded Coder tối ưu cho MCU (như TI C2000 hoặc Infineon AURIX).",
      "8. Xuất mô hình thành cấu trúc sẵn sàng cho HIL (Tách riêng Controller và Plant)."
    ],
    "artifacts": "Software Architecture, FOC Calibration Report, C Code Generation Report.",
    "deliverables": ["FOC Controller Simulink Model", "HIL Test Harness", "Generated C/H Files"],
    "criteria": ["Theo dõi Torque chính xác với độ gợn < 2%", "Field weakening hoạt động đúng", "Code sinh ra có thể chạy Real-time"],
    "eval_skills": "Pass: FOC vòng kín hoạt động tốt với HIL-ready architecture. Fail: Sai góc Rotor gây động cơ bị kẹt/rung.",
    "tips": ["Sử dụng thư viện Motor Control Blockset để tham khảo các khối chuẩn đã được tối ưu."],
    "risks": ["Rủi ro: Code thực thi quá chậm trên MCU thực tế. Biện pháp: Sử dụng Fixed-point data types (IQmath) thay vì Floating-point."]
  },
  {
    "id": "OJT-18",
    "title": "Powertrain Inverter Thermal Loss Modeling",
    "domain": "Powertrain",
    "level": "Advanced",
    "target_role": "MBSE Engineer",
    "duration": "32h",
    "effort_person_days": 4.0,
    "prerequisites": ["SysML", "Điện tử công suất", "Mô hình nhiệt"],
    "tools": "Cameo Systems Modeler; MATLAB/Simulink/Simscape R2024a",
    "data": "Datasheet của IGBT/SiC MOSFET (Đường cong Switching/Conduction loss).",
    "tasks": [
      "1. Định nghĩa yêu cầu hệ thống tản nhiệt Inverter bằng SysML Requirements Diagram.",
      "2. Chuyển datasheet đặc tính tổn hao (Turn-on, Turn-off, Conduction) thành LUTs 3D (dựa trên Điện áp, Dòng điện, Nhiệt độ junction).",
      "3. Xây dựng khối tính toán tổn hao trung bình (Averaged Loss Model) cho 6 switch Inverter.",
      "4. Xây dựng mô hình nhiệt mạng RC tương đương (Foster/Cauer network) từ Junction tới Case, Case tới Heatsink.",
      "5. Liên kết năng lượng tổn hao làm nguồn nhiệt (Heat flow source) cho mô hình RC.",
      "6. Khởi tạo một chu trình lái WLTP chạy trong 30 phút.",
      "7. Đánh giá nhiệt độ Junction T_j có vượt quá giới hạn an toàn (150°C) hay không.",
      "8. Trace các chỉ số (T_max, Loss) ngược lại với SysML để verify System Requirements."
    ],
    "artifacts": "SysML Requirements Diagram, RTM, Inverter Thermal Analysis Report.",
    "deliverables": ["SysML Project", "Simscape Thermal Model (.slx)", "Traceability Matrix"],
    "criteria": ["Mô hình tính toán tổn hao có độ chính xác hợp lý", "Traceability đầy đủ giữa SysML và Simulink"],
    "eval_skills": "Pass: Hiểu cách convert datasheet linh kiện bán dẫn sang mô hình vật lý. Fail: Mô phỏng quá chậm do chuyển mạch chi tiết thay vì Averaged model.",
    "tips": ["Sử dụng phương pháp Averaged model thay vì Switching model để có thể mô phỏng chu trình dài 30 phút."],
    "risks": ["Rủi ro: Datasheet không đủ dữ liệu. Biện pháp: Sử dụng phép nội suy/ngoại suy tuyến tính (Extrapolation)."]
  },
  {
    "id": "OJT-19",
    "title": "Torque Vectoring Logic for Dual Motor EV",
    "domain": "Powertrain",
    "level": "Advanced",
    "target_role": "System Engineer",
    "duration": "40h",
    "effort_person_days": 5.0,
    "prerequisites": ["Động lực học xe cơ bản (Vehicle Dynamics)", "Lý thuyết điều khiển tối ưu"],
    "tools": "MATLAB/Simulink R2024a; Vehicle Dynamics Blockset",
    "data": "Tham số xe: Chiều dài cơ sở, vết bánh xe, dữ liệu tham chiếu Yaw Rate.",
    "tasks": [
      "1. Xây dựng mô hình Bicycle Model làm tham chiếu (Reference Model) tính toán Yaw Rate mong muốn dựa trên góc đánh lái (Steering Angle) và tốc độ xe.",
      "2. Nhận tín hiệu Yaw Rate thực tế từ cảm biến (giả lập).",
      "3. Thiết kế bộ điều khiển (PI hoặc LQR) để tính toán mô-men xoắn hiệu chỉnh (Correction Yaw Moment).",
      "4. Xây dựng logic phân bổ Torque: Dựa trên Total Torque (người lái) và Correction Yaw Moment, tính toán Torque phân bổ cho Motor Trái và Phải độc lập.",
      "5. Thêm giới hạn trượt (Slip control) đơn giản: Rút Torque nếu bánh xe quay trơn.",
      "6. Tích hợp Plant model xe 4 bánh (14 DOF) bằng Vehicle Dynamics Blockset.",
      "7. Chạy kịch bản đánh lái gấp (Double Lane Change / ISO 3888).",
      "8. So sánh độ ổn định thân xe giữa có và không có Torque Vectoring."
    ],
    "artifacts": "Torque Vectoring Algorithm Design Specification.",
    "deliverables": ["Control Logic Model", "Vehicle Plant Integration", "ISO 3888 Test Report"],
    "criteria": ["Xe bám theo Yaw rate tham chiếu", "Giảm đáng kể góc trượt ngang (Side slip angle)", "Phân bổ Torque tối ưu, không vượt giới hạn Motor"],
    "eval_skills": "Pass: Cải thiện rõ rệt Handling của xe. Fail: Bộ điều khiển bị phân kỳ gây xoay xe (Spin-out).",
    "tips": ["Bắt đầu với Feedforward control dựa trên góc lái trước khi tuning Feedback control."],
    "risks": ["Rủi ro: Sai lệch giữa Reference model và xe thực tế. Biện pháp: Robust control hoặc gain scheduling theo tốc độ."]
  },
  {
    "id": "OJT-20",
    "title": "Powertrain Fault Tolerant Control (Inverter short-circuit)",
    "domain": "Powertrain",
    "level": "Expert",
    "target_role": "System Engineer",
    "duration": "40h",
    "effort_person_days": 5.0,
    "prerequisites": ["ISO 26262", "Diagnostic & Fault Handling", "Motor Control"],
    "tools": "MATLAB/Simulink/Stateflow R2024a",
    "data": "Kịch bản HARA: Ngắn mạch Inverter gây mô-men hãm ngoài ý muốn (Unintended braking torque).",
    "tasks": [
      "1. Xây dựng mô phỏng lỗi: Cài đặt công tắc (Fault injection block) làm chập mạch ngẫu nhiên 3 pha (Active Short Circuit - ASC) trên Inverter.",
      "2. Phân tích hiện tượng: Khi chập 3 pha ở tốc độ cao, dòng điện sinh ra mô-men hãm cực lớn.",
      "3. Thiết kế chẩn đoán lỗi (Diagnostic logic): Giám sát dòng điện pha lớn bất thường hoặc lỗi phần cứng từ Gate Driver.",
      "4. Xây dựng Safe State (Trạng thái an toàn) trong Stateflow: Nếu phát hiện lỗi, chuyển sang trạng thái Freewheeling (Mở tất cả công tắc) thay vì ASC nếu an toàn hơn, hoặc kích hoạt ASC nếu điện áp back-EMF quá cao.",
      "5. Triển khai phân tích Trade-off: Giữa nguy cơ quá áp DC link (khi Freewheeling) và nguy cơ phanh gấp (khi ASC).",
      "6. Đưa logic Torque phản ứng: Báo VCU giới hạn công suất.",
      "7. Chạy System-level mô phỏng: Xe đang chạy 100km/h, tiêm lỗi, xác minh hệ thống phản ứng trong <10ms.",
      "8. Xuất báo cáo chứng minh tuân thủ ASIL (Safety Validation)."
    ],
    "artifacts": "System Safety Architecture, Fault Injection Test Plan & Report, Cross-discipline Tradeoff analysis (Safety vs Control).",
    "deliverables": ["Fault Injection Model", "Diagnostic Stateflow", "Safety Validation Report"],
    "criteria": ["Thời gian chẩn đoán và chuyển trạng thái < 10ms", "Lựa chọn Safe State đúng dựa trên dải tốc độ", "Mô phỏng System-level V&V rõ ràng"],
    "eval_skills": "Pass: Hiểu sâu sắc hệ quả cơ điện của Inverter short-circuit và xử lý đúng. Fail: Quyết định sai Safe State gây hỏng linh kiện.",
    "tips": ["Thêm biểu đồ Safe Operating Area (SOA) của Inverter vào logic ra quyết định."],
    "risks": ["Rủi ro: Logic xử lý lỗi quá nhạy gây ngắt nhầm (False positive). Biện pháp: Calibration thời gian Debounce."]
  },
  {
    "id": "OJT-21",
    "title": "Signal Interface Mapping between BMS and Powertrain",
    "domain": "Integration",
    "level": "Beginner",
    "target_role": "MBSE Engineer",
    "duration": "12h",
    "effort_person_days": 1.5,
    "prerequisites": ["SysML cơ bản", "Simulink Data Dictionary"],
    "tools": "Cameo Systems Modeler; MATLAB/Simulink R2024a",
    "data": "Tài liệu danh sách tín hiệu (Signal Matrix) định dạng Excel.",
    "tasks": [
      "1. Sử dụng SysML tạo Internal Block Diagram (IBD) hiển thị các Port kết nối giữa BMS và Inverter/VCU.",
      "2. Định nghĩa các luồng Item Flows: Tín hiệu năng lượng (Max Discharge Limit, HV Voltage) và Tín hiệu điều khiển.",
      "3. Chuyển đổi SysML Ports thành kiến trúc khung (Skeleton model) trong Simulink sử dụng System Composer hoặc Simulink Bus.",
      "4. Tạo Simulink Data Dictionary (.sldd) để định nghĩa kiểu dữ liệu (uint8, float32, boolean) và độ phân giải của các tín hiệu.",
      "5. Thiết lập Bus Creator và Bus Selector tại các ranh giới Subsystem.",
      "6. Xây dựng logic stubbing: Khối phát tín hiệu giả lập (Signal generator) từ phía BMS.",
      "7. Khối nhận tín hiệu phía Powertrain có logic kiểm tra tính hợp lệ của tín hiệu (Data type, Range check).",
      "8. Generate báo cáo Interface Control Document (ICD)."
    ],
    "artifacts": "SysML IBD, Interface Control Document (ICD), Data Dictionary.",
    "deliverables": ["SysML Model", "Simulink Architecture Model", "Data Dictionary file", "ICD Report"],
    "criteria": ["Khớp 100% giữa SysML Port và Simulink Bus", "ICD được gen tự động", "Data type chuẩn (MISRA)"],
    "eval_skills": "Pass: Nắm vững quy trình quản lý giao diện phức tạp bằng Model. Fail: Dùng sai Data Type gây tràn số.",
    "tips": ["Dùng System Composer profile để gán trực tiếp thuộc tính tín hiệu lên SysML Ports."],
    "risks": ["Rủi ro: Xung đột Data Dictionary. Biện pháp: Dùng Referenced Data Dictionaries."]
  },
  {
    "id": "OJT-22",
    "title": "Basic VCU Torque Coordination",
    "domain": "Integration",
    "level": "Beginner",
    "target_role": "System Engineer",
    "duration": "16h",
    "effort_person_days": 2.0,
    "prerequisites": ["Simulink", "Kiến trúc xe EV"],
    "tools": "MATLAB/Simulink R2024a",
    "data": "Các profile đầu vào: Pedal vị trí (0-100%), Giới hạn Torque của Inverter, Giới hạn dòng xả của BMS.",
    "tasks": [
      "1. Thiết lập Subsystem điều phối Torque (Torque Coordinator).",
      "2. Quy đổi Driver Pedal Demand thành Torque Yêu Cầu thô.",
      "3. Lấy tín hiệu BMS Max Discharge Current, dùng điện áp HV để tính Công suất tối đa, từ đó tính Torque giới hạn do Pin.",
      "4. Lấy tín hiệu Motor Max Torque giới hạn do nhiệt độ Inverter.",
      "5. Áp dụng logic Minimum (Min) để chọn Torque mục tiêu cuối cùng an toàn nhất.",
      "6. Thêm logic Creep Torque (xe bò chậm ở tốc độ < 5km/h khi nhả phanh).",
      "7. Đưa các kịch bản test vào Signal Builder: Tăng tốc khi pin yếu, Tăng tốc khi motor quá nhiệt.",
      "8. Mô phỏng và kiểm tra tính ưu tiên của các giới hạn."
    ],
    "artifacts": "Logic Design Specification.",
    "deliverables": ["Simulink Model", "Test Scenarios", "Simulation Report"],
    "criteria": ["Torque không bao giờ vượt qua giới hạn khắt khe nhất", "Logic Creep hoạt động đúng tốc độ", "Các chức năng phân rã rõ ràng"],
    "eval_skills": "Pass: Hiểu vai trò trung tâm của VCU. Fail: Sai vòng tính toán (vd: tính sai Công suất thành Torque).",
    "tips": ["Luôn dùng Rate Transition nếu các tín hiệu đến từ các mạng có sample time khác nhau."],
    "risks": ["Rủi ro: Chia cho số 0 khi tính Torque từ Công suất ở tốc độ = 0. Biện pháp: Thêm epsilon ở mẫu số hoặc chặn tốc độ tối thiểu."]
  },
  {
    "id": "OJT-23",
    "title": "BMS and Powertrain CAN Interface Communication",
    "domain": "Integration",
    "level": "Intermediate",
    "target_role": "Integration Engineer",
    "duration": "24h",
    "effort_person_days": 3.0,
    "prerequisites": ["Giao thức CAN", "Vehicle Network Toolbox", "SysML"],
    "tools": "MATLAB/Simulink R2024a; Vehicle Network Toolbox; Cameo Systems Modeler",
    "data": "File mô tả mạng CAN (.dbc file) định nghĩa message BMS_Status và Motor_Cmd.",
    "tasks": [
      "1. Sử dụng SysML tạo sơ đồ Physical Architecture với 2 ECU nối với nhau qua CAN bus.",
      "2. Dùng MATLAB Vehicle Network Toolbox nạp file .dbc vào Workspace.",
      "3. Xây dựng model bên phát (BMS): Đóng gói tín hiệu SOC, Voltage, Fault_Level vào CAN Pack block theo định nghĩa DBC.",
      "4. Xây dựng model bên nhận (VCU/Powertrain): Sử dụng CAN Unpack block để giải mã tín hiệu.",
      "5. Thêm logic giả lập lỗi đường truyền: Rớt mạng (Message timeout) hoặc Dữ liệu rác (CRC error - mô phỏng).",
      "6. Phía VCU thiết kế logic chẩn đoán mất liên lạc (Loss of Communication - Node Missing) bằng bộ đếm thời gian (Counter).",
      "7. Cấu hình Requirements trace từ SysML Communication requirement tới CAN Pack/Unpack logic.",
      "8. Chạy mô phỏng, ngắt CAN ở giây thứ 5, xác minh VCU phản hồi bằng cách báo lỗi và đưa Torque về 0 an toàn."
    ],
    "artifacts": "SysML Network Architecture, CAN Communication RTM, Timeout Handling Logic Document.",
    "deliverables": ["CAN Simulation Model (.slx)", "DBC file update (nếu có)", "Network Simulation Report"],
    "criteria": ["Thông điệp được mã hóa và giải mã chuẩn xác theo DBC", "Logic Timeout xử lý chính xác", "SysML/RTM đầy đủ"],
    "eval_skills": "Pass: Mô phỏng đúng cơ chế CAN và xử lý mất gói tin. Fail: Tín hiệu bị sai Data type (vd: Float vs Integer scale/offset).",
    "tips": ["Sử dụng công cụ CAN FD nếu muốn băng thông lớn hơn."],
    "risks": ["Rủi ro: Khác biệt Sample time giữa các mạng. Biện pháp: Sử dụng cơ chế Asynchronous subsystem cho bộ nhận."]
  },
  {
    "id": "OJT-24",
    "title": "EV Pre-charge Sequence Control Integrating BMS and Inverter",
    "domain": "Integration",
    "level": "Intermediate",
    "target_role": "System Engineer",
    "duration": "24h",
    "effort_person_days": 3.0,
    "prerequisites": ["Stateflow", "Hệ thống điện cao áp (HV)", "Code Generation", "HIL"],
    "tools": "MATLAB/Simulink/Stateflow R2024a; Embedded Coder",
    "data": "Thông số R_precharge, C_DC_link của Inverter, điện áp Pin 400V.",
    "tasks": [
      "1. Xây dựng mô hình vật lý đơn giản: Pin -> Contactors (Main Neg, Pre-charge, Main Pos) -> Tụ DC Link Inverter.",
      "2. Thiết kế VCU State Machine (Stateflow): Gửi lệnh đóng contactor tới BMS, chờ BMS phản hồi, kiểm tra điện áp DC Link thông qua Inverter CAN.",
      "3. Tích hợp Handshake logic: VCU gửi 'HV_Request', BMS đóng relay báo 'Precharging', Inverter báo 'DC_Link_Vol_Reach_95%', VCU lệnh đóng Main Pos.",
      "4. Xử lý kịch bản lỗi: Pre-charge resistor bị đứt (Timeout) -> Hủy quá trình.",
      "5. Xử lý kịch bản lỗi: Ngắn mạch tại DC Link -> Dòng pre-charge quá cao, BMS chủ động ngắt -> VCU ghi nhận lỗi.",
      "6. Chuẩn bị mô hình Controller cho HIL (tách Plant và Control logic).",
      "7. Cấu hình Embedded Coder sinh mã C cho Stateflow logic (MISRA/AUTOSAR).",
      "8. Chạy vòng lặp SIL Back-to-Back verify thời gian delay Handshake."
    ],
    "artifacts": "Sequence Diagram, State Machine Design, C-Code Generation Report.",
    "deliverables": ["HIL-Ready Model", "C/H Source code", "SIL Report", "Sequence test report"],
    "criteria": ["Precharge thành công mượt mà", "Xử lý Timeout tốt", "Mã C được tạo ra không có warning"],
    "eval_skills": "Pass: Hiểu quy trình an toàn cấp nguồn HV và hand-shake nhiều ECU. Fail: Gây chập điện (Main đóng khi tụ chưa sạc).",
    "tips": ["Tạo kịch bản đo đạc thời gian nạp tụ thực tế (Tau = R*C) để set Timeout hợp lý."],
    "risks": ["Rủi ro: Độ trễ đường truyền CAN làm lệch Handshake. Biện pháp: Thiết lập dung sai thời gian (Tolerance) phù hợp."]
  },
  {
    "id": "OJT-25",
    "title": "Vehicle Cruise Control Integrating Powertrain",
    "domain": "Integration",
    "level": "Advanced",
    "target_role": "Model-Based Designer",
    "duration": "32h",
    "effort_person_days": 4.0,
    "prerequisites": ["Thiết kế điều khiển PID", "Stateflow"],
    "tools": "MATLAB/Simulink R2024a",
    "data": "Mô hình dọc xe tải (Vehicle Longitudinal Dynamics), Dải tốc độ mục tiêu (30 - 120 km/h).",
    "tasks": [
      "1. Lập trình giao diện HMI ảo trong Stateflow: Off, Standby, Active, Override (Khi đạp ga/phanh).",
      "2. Xây dựng PID Controller điều chỉnh vận tốc mục tiêu -> Torque Command.",
      "3. Tích hợp bộ điều khiển với mô hình động lực học Powertrain (từ OJT-13/14).",
      "4. Chỉnh định PID: Tăng tốc mượt mà, không quá ngưỡng gia tốc hành khách (comfort limit < 2m/s^2).",
      "5. Tích hợp logic giới hạn Torque từ BMS: Nếu hệ thống pin báo giới hạn dòng thấp, giới hạn sự gia tăng Torque của Cruise Control.",
      "6. Kiểm tra xử lý Override: Khi người lái đạp phanh, Cruise Control phải về Standby ngay lập tức; khi nhả phanh, dùng Resume để đưa về tốc độ cũ.",
      "7. Thêm kịch bản đường dốc: Lên dốc (bù Torque), Xuống dốc (dùng phanh tái sinh Regen).",
      "8. Phân tích kết quả mô phỏng trên các địa hình khác nhau."
    ],
    "artifacts": "Cruise Control Logic Spec, PID Tuning report.",
    "deliverables": ["Integrated Cruise Control Model", "Stateflow logic", "Terrain Simulation Report"],
    "criteria": ["Chuyển đổi trạng thái HMI đúng logic", "Gia tốc không vượt ngưỡng thoải mái", "Phối hợp tốt với Torque limit của BMS"],
    "eval_skills": "Pass: Hệ thống linh hoạt, êm ái trên các địa hình. Fail: PID bị windup khi xe leo dốc cao vượt quá khả năng.",
    "tips": ["Sử dụng Gain Scheduling cho PID tùy theo tốc độ (Tốc độ cao cần phản ứng khác tốc độ thấp)."],
    "risks": ["Rủi ro: Dao động (Hunting) ở tốc độ cài đặt khi đổ dốc nhẹ. Biện pháp: Tạo vùng chết (Deadband) nhỏ quanh setpoint."]
  },
  {
    "id": "OJT-26",
    "title": "Dynamic Torque Limiting based on BMS Discharge Limits",
    "domain": "Integration",
    "level": "Advanced",
    "target_role": "Integration Engineer",
    "duration": "32h",
    "effort_person_days": 4.0,
    "prerequisites": ["Động lực học xe", "Hệ thống năng lượng"],
    "tools": "MATLAB/Simulink R2024a",
    "data": "Bản đồ giới hạn công suất cực đại của Pin theo SOC, Nhiệt độ và SOH.",
    "tasks": [
      "1. Tái sử dụng model VCU Torque Coordination (OJT-22).",
      "2. Nhúng một mô hình BMS rút gọn phát tín hiệu Dynamic Discharge Power Limit liên tục.",
      "3. Thiết kế bộ lọc (LPF hoặc Rate Limiter) để làm mịn Power Limit, tránh hiện tượng rung xe khi ranh giới giới hạn thay đổi liên tục.",
      "4. Chuyển đổi Power Limit thành Torque Limit tại từng tốc độ motor.",
      "5. Thử nghiệm tình huống khó: Xe tăng tốc ở trạng thái SOC 10%, nhiệt độ pin -10°C (Power limit rất gắt).",
      "6. Thiết kế thuật toán Anti-jerk (Chống giật cục) ở Powertrain Controller để làm mượt đường cong Torque khi bị 'cắt' bởi giới hạn pin.",
      "7. Đánh giá Driveability bằng cách đo độ giật dọc xe (Longitudinal Jerk).",
      "8. Tối ưu thông số để Jerk < 10m/s^3."
    ],
    "artifacts": "Driveability Analysis Report, Anti-jerk Algorithm specification.",
    "deliverables": ["Integration Model", "Anti-jerk Controller", "Driveability Report"],
    "criteria": ["Ngăn chặn triệt để vượt quá giới hạn Pin", "Jerk duy trì ở ngưỡng an toàn", "Tốc độ xe đáp ứng mượt mà"],
    "eval_skills": "Pass: Xử lý mâu thuẫn giữa An toàn điện và Cảm giác lái tốt. Fail: Bộ lọc gây trễ pha làm hệ thống điều khiển mất ổn định.",
    "tips": ["Việc giới hạn nên có yếu tố dự báo (Predictive) dựa trên gia tốc hiện tại."],
    "risks": ["Rủi ro: Sự phụ thuộc vòng (Algebraic loop) giữa Công suất và Torque. Biện pháp: Dùng Unit delay để ngắt vòng lặp."]
  },
  {
    "id": "OJT-27",
    "title": "Eco-Routing Energy Consumption System-Level Simulation",
    "domain": "Integration",
    "level": "Advanced",
    "target_role": "System Engineer",
    "duration": "40h",
    "effort_person_days": 5.0,
    "prerequisites": ["Mô phỏng hệ thống", "Xử lý dữ liệu không gian"],
    "tools": "MATLAB/Simulink R2024a; Powertrain Blockset",
    "data": "Dữ liệu địa hình tuyến đường (Elevation profile), Thông số khí động học xe, Hiệu suất Motor/Battery.",
    "tasks": [
      "1. Nhập dữ liệu độ dốc (Grade/Elevation) theo quãng đường.",
      "2. Xây dựng Driver Model vòng ngoài điều khiển chân ga theo tốc độ giới hạn của biển báo trên đường.",
      "3. Ghép nối Mô hình Động lực dọc (Powertrain) và Mô hình Tiêu hao năng lượng (BMS).",
      "4. Tích hợp bản đồ hiệu suất (Efficiency maps) của Motor, Inverter, Gearbox.",
      "5. Chạy kịch bản: Mô phỏng chuyến đi 100km qua địa hình đồi núi.",
      "6. Tính toán điện năng tiêu thụ dự kiến (kWh/100km).",
      "7. Thay đổi chế độ lái sang 'Eco-mode' (Giới hạn max Torque, tăng Regen) và chạy lại mô phỏng.",
      "8. So sánh năng lượng tiết kiệm được và độ trễ thời gian đến đích."
    ],
    "artifacts": "Energy Consumption Analysis Report.",
    "deliverables": ["System Simulation Model", "Efficiency Maps data", "Comparison Report (Normal vs Eco)"],
    "criteria": ["Tính toán tiêu thụ năng lượng khớp với số liệu mẫu", "Eco-mode thể hiện rõ sự khác biệt"],
    "eval_skills": "Pass: Kết hợp đa hệ thống thành một mô hình đánh giá năng lượng chuẩn xác. Fail: Tính sai dấu công suất lúc thả dốc/Regen.",
    "tips": ["Sử dụng công cụ Optimization Toolbox để tìm Profile vận tốc tiết kiệm nhất."],
    "risks": ["Rủi ro: Mô hình chạy chậm do dữ liệu lớn. Biện pháp: Tăng kích thước bước (Step size) hoặc nội suy dữ liệu bản đồ với độ phân giải thấp hơn."]
  },
  {
    "id": "OJT-28",
    "title": "Automated HIL Test Harness Generation for VCU Logic",
    "domain": "Integration",
    "level": "Advanced",
    "target_role": "Integration Engineer",
    "duration": "32h",
    "effort_person_days": 4.0,
    "prerequisites": ["Simulink Test", "Code Generation", "HIL (dSpace/Speedgoat)"],
    "tools": "MATLAB/Simulink/Simulink Test R2024a; Embedded Coder",
    "data": "Mô hình VCU Controller (Ví dụ: Từ OJT-22, 24).",
    "tasks": [
      "1. Sử dụng tính năng Create Test Harness của Simulink để tách riêng Controller (Device Under Test - DUT) khỏi Plant model.",
      "2. Cấu hình Inports/Outports của DUT theo chuẩn tín hiệu I/O của phần cứng HIL (Tỷ lệ scale, offset, data type).",
      "3. Tạo Test Sequence block dùng để sinh các kịch bản kích thích đầu vào tự động (Step input, Sine wave, Fault pulses).",
      "4. Xây dựng Test Assessment logic để tự động đánh giá Pass/Fail ngay trong lúc chạy.",
      "5. Cấu hình Embedded Coder sinh mã cho toàn bộ DUT.",
      "6. Build ứng dụng Real-Time (nếu có target Speedgoat/dSPACE, hoặc build dạng Standalone Executable mô phỏng).",
      "7. Cấu hình Test Manager tự động chạy một lô 50 test cases.",
      "8. Xuất báo cáo HTML/PDF tự động về độ bao phủ (Coverage) và kết quả."
    ],
    "artifacts": "Test Plan, Automated Test Script, HIL Architecture.",
    "deliverables": ["Test Harness Model (.slx)", "Test Sequence definitions", "Automated HTML Test Report", "Generated C/H Code"],
    "criteria": ["Harness độc lập hoàn toàn với mô hình nguồn", "Test Assessment hoạt động tự động", "Báo cáo phủ 100% kịch bản"],
    "eval_skills": "Pass: Làm chủ quy trình test tự động và Code Gen. Fail: Khai báo sai giao diện I/O khiến HIL không build được.",
    "tips": ["Sử dụng Data Dictionary cho Harness để dễ dàng điều chỉnh cấu hình khi chuyển sang các dàn HIL khác nhau."],
    "risks": ["Rủi ro: Thiết kế test assessment quá chặt (Fragile tests). Biện pháp: Đưa vào sai số cho phép (Tolerance band)."]
  },
  {
    "id": "OJT-29",
    "title": "Integrated Thermal Management System (BMS and Powertrain)",
    "domain": "Integration",
    "level": "Expert",
    "target_role": "System Engineer",
    "duration": "48h",
    "effort_person_days": 6.0,
    "prerequisites": ["SysML", "Đa miền vật lý", "Hệ thống nhiệt"],
    "tools": "Cameo Systems Modeler; MATLAB/Simulink/Simscape R2024a",
    "data": "Hệ thống có chung 1 vòng làm mát bằng chất lỏng (Coolant loop) đi qua cả Battery, Inverter và Motor.",
    "tasks": [
      "1. Xây dựng SysML Architecture: Tương tác Hệ thống (Điện, Nhiệt, Cơ) cho toàn bộ hệ truyền động và hệ thống lưu chất (Bơm, Radiator, Chiller).",
      "2. Xây dựng mô hình vật lý Simscape Fluid/Thermal gồm vòng lặp nước làm mát đi qua Inverter -> Motor -> Battery -> Radiator.",
      "3. Mô phỏng bài toán Trade-off: Chạy chế độ 'Track Mode' (tải rất cao). Bơm chạy max công suất tiêu tốn năng lượng điện, giảm SOC nhanh, nhưng giữ nhiệt độ thấp.",
      "4. Thiết kế logic Thermal Management Controller (TMC) phân bổ lưu lượng bằng Van 3 ngã (3-way valve) hoặc Bơm biến tần.",
      "5. Tạo kịch bản ưu tiên: Khi Pin quá nhiệt, ưu tiên Chiller/lưu lượng cho Pin; Khi Inverter quá nhiệt (leo dốc dốc cao), ưu tiên Inverter.",
      "6. Đưa mô hình điều khiển này vào chạy vòng kín (Closed-loop) với các yêu cầu Traceability chặt chẽ.",
      "7. Đánh giá System-level verification: Chứng minh giải pháp điều khiển TMC giúp kéo dài quãng đường đi được so với việc bật bơm tối đa liên tục.",
      "8. Báo cáo đánh đổi (Tradeoff report) về Năng lượng tiêu thụ vs An toàn nhiệt."
    ],
    "artifacts": "SysML BDD/IBD/Parametric Diagrams, RTM, Trade-off Analysis Report.",
    "deliverables": ["SysML System Model", "Simscape Multi-physics Model", "Trade-off Report"],
    "criteria": ["Mô hình chạy mượt không bị nghẽn (solver errors)", "Kiến trúc SysML phản ánh đúng mô hình", "Báo cáo Trade-off định lượng rõ ràng"],
    "eval_skills": "Pass: Làm chủ hoàn toàn thiết kế đa miền hệ thống cấp chuyên gia. Fail: Không giải quyết được lỗi zero-crossing trong Simscape Fluids.",
    "tips": ["Chia hệ thống thành các Variant để có thể tắt bật từng nhánh làm mát, dễ debug."],
    "risks": ["Rủi ro: Mô phỏng vật lý đa miền cực phức tạp, không hội tụ. Biện pháp: Đơn giản hóa khối tản nhiệt (Radiator) thành LUT thay vì dùng khối trao đổi nhiệt vật lý nguyên bản."]
  },
  {
    "id": "OJT-30",
    "title": "System-level V&V for EV Sudden Acceleration Hazard",
    "domain": "Integration",
    "level": "Expert",
    "target_role": "MBSE Engineer",
    "duration": "48h",
    "effort_person_days": 6.0,
    "prerequisites": ["ISO 26262 ASIL D", "SysML", "System V&V"],
    "tools": "Cameo Systems Modeler; MATLAB/Simulink R2024a; Simulink Test",
    "data": "HARA System Hazard: Unintended Sudden Acceleration do lỗi giao tiếp BMS-VCU-Inverter.",
    "tasks": [
      "1. Sử dụng SysML tạo Safety Concept: Triển khai kiến trúc E-Gas Monitor Concept (Level 1: Function, Level 2: Plausibility, Level 3: MCU core check).",
      "2. Xây dựng khối giả lập mạng (Network fault injection) giữa VCU và Powertrain.",
      "3. Tạo mô hình tích hợp hệ thống chứa VCU, Powertrain, BMS (các phiên bản OJT trước đóng gói lại).",
      "4. Kịch bản lỗi: ECU VCU bị treo bộ đếm (freeze) và liên tục gửi Torque Command cực đại (Max Torque).",
      "5. Lập trình Logic Plausibility ở Inverter (Level 2): So sánh Torque Command với vị trí Pedal phanh cứng thực tế (Hardware wire) -> Nhận thấy mâu thuẫn.",
      "6. Inverter tự động bỏ qua lệnh CAN và cắt Torque, đưa xe vào Safe State (Coasting).",
      "7. Cấu hình RTM liên kết Safety Requirements SysML tới Logic xử lý cắt Torque.",
      "8. Chạy System-level mô phỏng (HIL mô phỏng phần cứng), đo thời gian từ lúc phát sinh lỗi đến lúc xe ngắt lực kéo (< 200ms). Báo cáo V&V."
    ],
    "artifacts": "SysML Safety Architecture, E-Gas Implementation, System V&V Report, Requirements Traceability Matrix.",
    "deliverables": ["SysML Architecture Model", "Fault Injection Integrated Model", "System Verification Report"],
    "criteria": ["Kiến trúc E-Gas 3 lớp thể hiện rõ ràng", "Thời gian ngắt Torque < 200ms theo chuẩn", "Traceability hoàn chỉnh từ ASIL D Requirements đến Test result"],
    "eval_skills": "Pass: Triển khai hoàn chỉnh V-cycle vòng phải (Verification) cấp độ hệ thống. Fail: Bỏ sót đường truyền tín hiệu chéo (Cross-check signal) khiến hệ thống bảo vệ không được kích hoạt.",
    "tips": ["Mô phỏng cả độ trễ mạng CAN và trễ cơ khí để tính tổng Response Time chính xác."],
    "risks": ["Rủi ro: Redundancy path gây ra lỗi False Positive khi đạp phanh và ga cùng lúc (Two-foot driving). Biện pháp: Phân bổ ưu tiên phanh rõ ràng (Brake override system)."]
  }
]
```
