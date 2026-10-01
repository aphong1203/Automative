# AUTOSAR Classic End-to-End: Từ ARXML đến ECU

> Tài liệu tổng hợp trả lời 10 câu hỏi cốt lõi về toàn bộ workflow AUTOSAR Classic,
> tập trung vào mối quan hệ: ARXML ↔ Simulink/AUTOSAR Blockset ↔ RTE ↔ BSW ↔ DaVinci/EB tresos.

---

## Mục lục

1. [ARXML thực sự là gì?](#1-arxml-thực-sự-là-gì)
2. [Tại sao phải Import ARXML vào Simulink?](#2-tại-sao-phải-import-arxml-vào-simulink)
3. [Simulink thực sự làm nhiệm vụ gì trong AUTOSAR?](#3-simulink-thực-sự-làm-nhiệm-vụ-gì-trong-autosar)
4. [AUTOSAR Mapping thực sự là gì?](#4-autosar-mapping-thực-sự-là-gì)
5. [Runnable và Simulink Function liên hệ thế nào?](#5-runnable-và-simulink-function-liên-hệ-thế-nào)
6. [RTE nằm ở đâu trong toàn bộ workflow?](#6-rte-nằm-ở-đâu-trong-toàn-bộ-workflow)
7. [DaVinci và EB tresos nằm chính xác ở đâu?](#7-davinci-và-eb-tresos-nằm-chính-xác-ở-đâu)
8. [C code được generate ở đâu?](#8-c-code-được-generate-ở-đâu)
9. [ARXML output sau khi generate là gì?](#9-arxml-output-sau-khi-generate-là-gì)
10. [Toàn bộ project thực tế chạy như thế nào?](#10-toàn-bộ-project-thực-tế-chạy-như-thế-nào)

---

## 1. ARXML thực sự là gì?

### Định nghĩa

ARXML (AUTOSAR XML) là định dạng XML chuẩn do tổ chức AUTOSAR định nghĩa, dùng để mô tả toàn bộ hệ thống phần mềm automotive ở mức architecture. Nó không phải code, không phải executable — nó là **metadata** mô tả cấu trúc phần mềm.

### ARXML có phải là input bắt buộc cho AUTOSAR Blockset?

**Không bắt buộc.** Theo AUTOSAR Blockset User's Guide R2025b, có 3 cách tạo AUTOSAR SWC trong Simulink:

| Cách | Mô tả | Cần ARXML? |
|------|--------|------------|
| **Bottom-Up** | Tạo Simulink model mới, dùng AUTOSAR Component Quick Start để map | Không cần ARXML đầu vào |
| **Round-Trip** | Import ARXML từ AAT (Authoring Tool) khác vào Simulink, phát triển, rồi xuất ARXML trả về | Cần ARXML đầu vào |
| **Template** | Dùng AUTOSAR Blockset template từ Simulink Start Page | Không cần ARXML đầu vào |

Tuy nhiên, trong dự án thực tế ở các OEM/Tier-1, **round-trip workflow là phổ biến nhất**, vì architecture thường được định nghĩa bởi system engineer ở tool khác (DaVinci, SystemDesk, ARXML text editor), rồi mới giao ARXML cho model-based development team.

### ARXML do ai tạo?

| Role | Tool | Tạo ARXML loại nào |
|------|------|-------------------|
| **OEM System Engineer** | DaVinci Configurator / SystemDesk / PREEvision | System ARXML, ECU Extract ARXML, Composition ARXML |
| **Tier-1 BSW Team** | DaVinci / EB tresos Studio | BSW MD (Module Description) ARXML, EcuConfiguration ARXML |
| **Application Team** | Simulink AUTOSAR Blockset | SWC ARXML (component, interface, datatype, implementation) |
| **Integration Team** | DaVinci / EB tresos | Merged ARXML cho RTE generation |

**Source of truth**: OEM hoặc Tier-1 system architect là người định nghĩa ARXML ở mức System/ECU. Application developer nhận ARXML mô tả SWC (ports, interfaces, data types) làm input.

### Một project có 1 ARXML hay rất nhiều ARXML?

**Rất nhiều.** Một project ECU thực tế có thể có hàng chục đến hàng trăm file ARXML, phân loại theo chức năng:

```
Project_ARXML/
├── SystemDescription.arxml        # Mô tả toàn bộ system (ECU, network, topology)
├── ECUExtract.arxml               # Extract ra cho 1 ECU cụ thể
├── Composition/
│   ├── Composition_Top.arxml      # Composition SWC (chứa nhiều SWC)
│   └── Composition_Sub.arxml
├── SWC/
│   ├── SWC_EngineCtrl.arxml       # Mô tả Application SWC
│   ├── SWC_BrakeCtrl.arxml
│   └── ...
├── Interfaces/
│   ├── If_PowerTrain.arxml        # Sender-Receiver, Client-Server interfaces
│   └── If_Chassis.arxml
├── DataTypes/
│   ├── DT_Common.arxml            # Application data types, CompuMethods
│   └── DT_Specific.arxml
├── BSW/
│   ├── BSWMD_Can.arxml            # BSW Module Description
│   ├── BSWMD_Pwm.arxml
│   └── ...
└── EcuConfiguration.arxml         # Cấu hình BSW modules
```

### ARXML chứa những gì?

ARXML có thể chứa nhiều loại nội dung khác nhau, tuỳ thuộc vào file:

| Loại nội dung ARXML          | Mô tả                                                                     | Ví dụ element                                                       |
| ---------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **System**                   | Topology ECU, network mapping, ECU-to-ECU communication                   | `SYSTEM`, `SYSTEM-MAPPING`, `ECU-INSTANCE`                          |
| **ECU Configuration**        | Cấu hình BSW modules cho 1 ECU cụ thể                                     | `ECU-CONFIGURATION`, `MODULE-CONFIGURATION`                         |
| **SWC (Software Component)** | Định nghĩa Application SWC: ports, runnables, internal behavior           | `SWC-IMPLEMENTATION`, `SWC-INTERNAL-BEHAVIOR`, `RUNNABLE-ENTITY`    |
| **RTE**                      | Không có "RTE ARXML" riêng — RTE được generate từ SWC + Composition ARXML | (RTE generator đọc SWC ARXML)                                       |
| **BSW**                      | BSW Module Description (MD) và BSW Module Configuration                   | `BSW-MODULE-DESCRIPTION`, `BSW-MODULE-ENTRY`                        |
| **Network**                  | CAN/LIN/FlexRay/Ethernet frame definitions, signal mapping                | `CAN-CLUSTER`, `FRAME`, `PDU-GROUPING`                              |
| **Data Type**                | Application data types, implementation data types, CompuMethods           | `APPLICATION-DATA-TYPE`, `IMPLEMENTATION-DATA-TYPE`, `COMPU-METHOD` |

### Phần nào của ARXML thực sự được Simulink import?

Simulink AUTOSAR Blockset **chỉ import phần SWC-related**:

- `SWC-IMPLEMENTATION` — định nghĩa SWC
- `SWC-INTERNAL-BEHAVIOR` — runnables, ports, IRVs
- `APPLICATION-DATA-TYPE` / `IMPLEMENTATION-DATA-TYPE` — data types
- `SENDER-RECEIVER-INTERFACE` — communication interfaces
- `CLIENT-SERVER-INTERFACE` — service interfaces
- `MODE-SWITCH-INTERFACE` — mode interfaces
- `COMPU-METHOD` — computation methods
- `SWC-COMPOSITION` — composition (nếu có)

### Phần nào của ARXML không liên quan đến Simulink?

Simulink **không quan tâm** đến:

- `SYSTEM` / `SYSTEM-MAPPING` — topology level
- `ECU-INSTANCE` — ECU hardware mapping
- `BSW-MODULE-DESCRIPTION` — BSW module definitions
- `BSW-MODULE-CONFIGURATION` — BSW configuration
- `CAN-CLUSTER` / `FRAME` / `PDU` — network configuration
- `ECU-CONFIGURATION` — ECU-level BSW configuration

Tất cả những phần này là việc của **DaVinci Configurator** hoặc **EB tresos Studio**.

---

## 2. Tại sao phải Import ARXML vào Simulink?

### Câu trả lời cốt lõi

Import ARXML vào Simulink **không phải để generate code trực tiếp**. Mục đích chính là **tạo Simulink model skeleton** — một mô hình Simulink đã được cấu hình sẵn với các AUTOSAR elements (ports, interfaces, runnables, data types) — để developer chỉ cần tập trung vào việc **viết thuật toán (algorithm)**.

### Tại sao không viết C code trực tiếp?

| Tiêu chí | Viết C trực tiếp | Dùng Simulink + ARXML |
|----------|------------------|----------------------|
| Thuật toán phức tạp | Khó debug, khó verify | Model-based, simulate được |
| Traceability | Thủ công | Tự động từ requirement → model → code |
| MISRA-C compliance | Phải tự tuân thủ | Embedded Coder tự generate MISRA-C compliant code |
| Calibration | Phải tự viết A2L | Tự động generate A2L/ARXML calibration |
| B2B Testing | Phải viết test framework riêng | Simulink Test + B2B testing tích hợp |
| Code review | Code review manual | Model review + code generation report |
| Maintainability | Khó maintain khi logic phức tạp | Model dễ đọc, dễ modify |

### Tại sao không dùng DaVinci/EB tresos để viết Application?

DaVinci Configurator và EB tresos Studio là **BSW configuration tools**, không phải **algorithm development tools**. Chúng:

- Không có khả năng mô phỏng (simulate) thuật toán
- Không có Model-Based Design capabilities
- Không hỗ trợ Stateflow, lookup tables, fixed-point arithmetic
- Không generate application algorithm code — chỉ generate BSW configuration code và RTE code

**DaVinci/EB tresos = BSW + RTE configuration. Simulink = Application algorithm development.**

### Import ARXML có mục đích gì cụ thể?

Khi import ARXML, Simulink thực hiện **tất cả** những việc sau:

1. **Tạo Simulink model skeleton**: Model mới với Inport/Outport blocks tương ứng với AUTOSAR ports
2. **Tạo AUTOSAR Dictionary entries**: Ports, interfaces, data types, CompuMethods được nạp vào AUTOSAR Dictionary của model
3. **Tạo Code Mappings**: Simulink elements (inports, outports) được tự động map sang AUTOSAR elements (ports, data elements)
4. **Tạo Runnable structure**: Nếu ARXML có nhiều runnables, Simulink tạo các subsystem/function tương ứng
5. **Tạo Data Types**: Application data types, implementation data types, enumerated types được import vào model workspace
6. **Tạo Inter-Runnable Variables**: Signal lines giữa các runnables được tạo tự động
7. **Preserve ARXML structure**: Cho round-trip — giữ nguyên cấu trúc ARXML gốc để export trả về sau này

### Sau khi import, Simulink tạo ra những object/block nào?

```
ARXML import
    │
    ├── Simulink Model (.slx)
    │   ├── Inport blocks  ← AUTOSAR Require Ports (RPort)
    │   ├── Outport blocks ← AUTOSAR Provide Ports (PPort)
    │   ├── Subsystem/Function blocks ← Runnables
    │   ├── Signal lines ← Inter-Runnable Variables (IRV)
    │   └── (trống) ← Developer điền thuật toán vào đây
    │
    ├── AUTOSAR Dictionary
    │   ├── SWC definition
    │   ├── Ports (PPort, RPort, PRPort)
    │   ├── Interfaces (S/R, C/S, Mode, NvM, Parameter, Trigger)
    │   ├── Data Types (Application, Implementation, Enum)
    │   ├── CompuMethods
    │   ├── SwAddrMethods
    │   └── Runnables + RTEEvents
    │
    └── Code Mappings
        ├── Inport → RPort + Data Element
        ├── Outport → PPort + Data Element
        ├── Entry-point function → Runnable
        ├── Parameter → Constant Memory / Shared Parameter
        ├── Data Store → Per-Instance Memory
        └── Signal/State → Per-Instance Memory / IRV
```

---

## 3. Simulink thực sự làm nhiệm vụ gì trong AUTOSAR?

### Vai trò của Simulink

Simulink chịu trách nhiệm **chỉ ở Application Layer** — cụ thể là phát triển **Application SWC (ASWC)** behavior.

```
┌─────────────────────────────────────────────────┐
│                AUTOSAR Layer Stack               │
├─────────────────────────────────────────────────┤
│  Application Layer (Simulink chịu trách nhiệm)  │
│  ┌─────────────────────────────────────────┐     │
│  │  ASWC (Atomic Software Component)       │     │
│  │  ├── Algorithm logic (developer viết)   │     │
│  │  ├── Runnable structure (map)           │     │
│  │  ├── Port access (map)                  │     │
│  │  └── Internal behavior (map)            │     │
│  └─────────────────────────────────────────┘     │
├─────────────────────────────────────────────────┤
│  RTE (DaVinci/EB tresos generate)               │
├─────────────────────────────────────────────────┤
│  BSW (DaVinci/EB tresos configure)              │
├─────────────────────────────────────────────────┤
│  MCAL (Vendor-specific)                          │
├─────────────────────────────────────────────────┤
│  Hardware (MCU)                                  │
└─────────────────────────────────────────────────┘
```

### Phân biệt trách nhiệm chi tiết

Trong Application SWC, có nhiều phần khác nhau, mỗi phần do một tool/role khác nhau chịu trách nhiệm:

| Thành phần | Ai tạo/định nghĩa | Tool |
|------------|-------------------|------|
| **Algorithm** (logic điều khiển) | Developer viết trong Simulink | Simulink |
| **Runnable** (entry-point function) | Developer map từ Simulink function | Simulink + AUTOSAR Blockset |
| **Port** (PPort, RPort) | System engineer định nghĩa trong ARXML → Import vào Simulink | ARXML → Simulink |
| **Interface** (S/R, C/S, Mode) | System engineer định nghĩa trong ARXML | ARXML → Simulink Dictionary |
| **Data Type** | System engineer định nghĩa trong ARXML | ARXML → Simulink |
| **RTE API calls** (Rte_Read, Rte_Write) | Embedded Coder generate | Simulink AUTOSAR Blockset |
| **RTE skeleton** (actual RTE C code) | RTE Generator trong DaVinci/EB tresos | DaVinci/EB tresos |
| **BSW modules** (Can, Pwm, Adc, ...) | BSW engineer configure | DaVinci/EB tresos |
| **MCAL** | Chip vendor provide | Vendor SDK |

### Cái nào do developer viết trong Simulink?

Developer **chỉ viết phần thuật toán (algorithm)** — tức là logic điều khiển, state machines, calculations, lookup tables, v.v. Tất cả phần còn lại (port access, runnable scheduling, RTE API calls) được AUTOSAR Blockset tự động generate.

### Cái nào do AUTOSAR Blockset generate?

- AUTOSAR-compliant C code cho algorithm
- RTE API wrapper calls (`Rte_Read`, `Rte_Write`, `Rte_Call`, `Rte_Result`)
- ARXML output files (component, interface, datatype, implementation)
- AUTOSAR Dictionary entries
- Code generation report + Code Interface Report

### Cái nào do RTE Generator generate?

- RTE C code (communication layer giữa SWC và BSW)
- `Rte.c` / `Rte.h` — actual implementation của `Rte_Read()`, `Rte_Write()`, v.v.
- Runnable scheduling infrastructure (task-to-runnable mapping)
- Inter-runnable variable management
- Port-to-port communication routing

### Cái nào do DaVinci/EB tresos generate?

- BSW module configuration C code
- BSW scheduler configuration
- RTE configuration (sử dụng ARXML từ Simulink output làm input)
- OS task configuration (AUTOSAR OS)
- Memory mapping configuration
- Build integration files

---

## 4. AUTOSAR Mapping thực sự là gì?

### Định nghĩa

AUTOSAR Mapping là quá trình **liên kết (link) các Simulink model elements với các AUTOSAR elements**. Đây là bước **bắt buộc** trước khi generate code — nếu không map, code generation sẽ fail (lỗi `RTWCGAutosarEmptyConfigurationError` mà bạn đã gặp).

### Bảng mapping chuẩn

Theo AUTOSAR Blockset User's Guide R2025b, bảng mapping là:

| Simulink Element | AUTOSAR Element | Giải thích |
|-----------------|-----------------|------------|
| Entry-point function | Runnable | Function C ↔ Runnable entity |
| Input port (Inport) | Data-read-access hoặc Inter-Runnable Variable | Đọc data từ RPort |
| Output port (Outport) | Data-write-access hoặc Inter-Runnable Variable | Ghi data qua PPort |
| State hoặc signal line | Per-instance memory | Dữ liệu lưu giữa các execution cycle |
| Model parameter | Constant memory hoặc Shared parameter | Calibration data |
| Model parameter argument | Port parameter hoặc Per-instance parameter | Tham số truyền qua port |
| Data store | Per-instance memory | Dữ liệu persistent |
| Function caller | Client port và operation | Client-Server communication |
| Rate-Transition block | Inter-runnable variable | Chuyển data giữa các rate khác nhau |

### Mapping được thực hiện khi nào?

```
Timeline:
    ARXML Import ──► Auto-mapping (Quick Start) ──► Developer chỉnh sửa ──► Code Generation
         ↑                    ↑                          ↑                        ↑
    Tạo ports,         Mapping tự động           Refine mapping          Mapping được dùng
    interfaces,        (Inport→RPort,             (thêm IRV,               để generate
    data types         Outport→PPort,             data store,              C code + ARXML
                       Function→Runnable)         parameter mapping)
```

**Mapping được thực hiện SAU import ARXML** (nếu dùng round-trip) hoặc **SAU khi tạo model** (nếu dùng bottom-up).

### Mapping được lưu ở đâu?

Mapping được lưu **trong chính Simulink model file (.slx)**, cụ thể:

- **Code Mappings Editor** — lưu trong model metadata
- **AUTOSAR Dictionary** — lưu cấu hình AUTOSAR elements trong model
- Có thể truy cập programmatically qua `autosar.api.getAUTOSARProperties()` và `autosar.api.setAUTOSARProperties()`

### Mapping có nằm trong ARXML không?

**Có, một phần.** Khi Simulink export ARXML, mapping được thể hiện qua:

- `RUNNABLE-ENTITY` có `DATA-WRITE-ACCESS` / `DATA-READ-ACCESS` — thể hiện port access trong runnable
- `SWC-INTERNAL-BEHAVIOR` chứa `RUNNABLE-ENTITY` list — thể hiện function-to-runnable mapping
- `PORT-DEFINED-ARGUMENT-VALUES` — thể hiện parameter mapping

Nhưng **mapping giữa Simulink signal line và AUTOSAR IRV** chỉ tồn tại trong model (.slx), không export ra ARXML.

### Khi generate code, mapping ảnh hưởng thế nào?

Mapping quyết định **cấu trúc C code được generate**:

```c
// Nếu Inport "In1_1s" được map sang RPort "RPort1" + DataElement "DE1":
// Generated code sẽ là:
void Runnable_1s(void) {
    // Rte_Read được generate dựa trên mapping
    float RPort1_DE1;
    Rte_Read_RPort1_DE1(&RPort1_DE1);  // ← RTE API call tự động generate
    // ... algorithm code ...
    Rte_Write_PPort1_Result(result);   // ← RTE API call cho Outport
}
```

### Nếu mapping sai thì C code khác thế nào?

| Mapping sai                      | Hậu quả                                                     |
| -------------------------------- | ----------------------------------------------------------- |
| Inport không map                 | Code generation fail: `RTWCGAutosarEmptyConfigurationError` |
| Function không map sang Runnable | Function bị ignore, không xuất hiện trong ARXML             |
| Data type mapping sai            | Type mismatch, code generation error                        |
| Port mapping sai                 | RTE API call sai tên, runtime error                         |
| IRV mapping thiếu                | Data loss giữa runnables, logic sai                         |

---

## 5. Runnable và Simulink Function liên hệ thế nào?

### Runnable có phải là một function C không?

**Đúng.** Mỗi Runnable tương ứng với một C function entry-point trong generated code. Ví dụ:

| AUTOSAR Runnable | Generated C Function | Loại |
|------------------|---------------------|------|
| `Runnable_Initialize` | `void Runnable_Initialize(void)` | Initialization |
| `Runnable_1s` | `void Runnable_1s(void)` | Periodic (1s) |
| `Runnable_2s` | `void Runnable_2s(void)` | Periodic (2s) |
| `Runnable_Trigger` | `void Runnable_Trigger(void)` | Asynchronous (event-triggered) |

### Simulink model có tương ứng 1:1 với Runnable không?

**Không nhất thiết.** Có 2 pattern chính:

#### Pattern 1: Rate-Based Component (1 model → nhiều Runnables)

```
Simulink Model (1 SWC)
├── Subsystem rate 1s  → Runnable_1s
├── Subsystem rate 2s  → Runnable_2s
└── Initialize block   → Runnable_Initialize
```

Trong pattern này, **1 model = 1 SWC** chứa **nhiều Runnables**, mỗi runnable tương ứng với 1 rate khác nhau. Simulink tự động phân chia dựa trên sample time.

#### Pattern 2: Function-Call-Based Component

```
Simulink Model (1 SWC)
├── Simulink Function block "Runnable_A" → Runnable_A
├── Simulink Function block "Runnable_B" → Runnable_B
└── Initialize Function block            → Runnable_Initialize
```

Pattern này dùng cho asynchronous runnables hoặc JMAAB type beta architecture.

### Một SWC có nhiều Runnable thì Simulink tổ chức thế nào?

```
┌─────────────── Simulink Model (SWC) ───────────────┐
│                                                     │
│  ┌─────────────┐     ┌─────────────┐               │
│  │ Runnable_1s │     │ Runnable_2s │               │
│  │ (Subsystem  │     │ (Subsystem  │               │
│  │  rate=1s)   │     │  rate=2s)   │               │
│  └──────┬──────┘     └──────┬──────┘               │
│         │                   │                       │
│         └───────┬───────────┘                       │
│                 │                                   │
│          Rate Transition                           │
│          (Inter-Runnable Variable)                  │
│                                                     │
│  ┌─────────────────┐                                │
│  │ Initialize Func │ → Runnable_Initialize          │
│  └─────────────────┘                                │
└─────────────────────────────────────────────────────┘
```

- Mỗi Runnable là một **Subsystem** hoặc **Simulink Function** ở root level
- Data chuyển giữa các Runnable qua **signal line** = **Inter-Runnable Variable (IRV)**
- **Rate Transition block** đại diện cho IRV khi các runnable chạy ở rate khác nhau

### 10ms nằm trong ARXML hay Simulink?

| Thông tin | Nằm ở đâu | Cụ thể |
|-----------|-----------|--------|
| **Timing period (10ms)** | ARXML | `TIMING-EVENT` trong `SWC-INTERNAL-BEHAVIOR` có thuộc tính `period` = 0.01 |
| **Sample time (10ms)** | Simulink | Inport/Subsystem sample time = 0.01 |
| **OS task period** | DaVinci/EB tresos | OS task configuration (AUTOSAR OS alarm) |
| **RTE event** | RTE Generator | `Rte_ReRunnable_10ms` — RTE tạo event dựa trên ARXML |

**Flow**: ARXML định nghĩa `TimingEvent(period=10ms)` → Simulink import tạo Inport với sample time = 0.01 → DaVinci/EB tresos tạo OS task 10ms → RTE generate `Rte_ReRunnable_10ms` event → RTE gọi `Runnable_10ms()` khi event fires.

### Event gọi Runnable được generate ở đâu?

- **ARXML** định nghĩa `RTE-EVENT` (ví dụ: `TimingEvent` với period)
- **RTE Generator** (trong DaVinci/EB tresos) đọc ARXML và tạo C code cho event handling
- **RTE C code** chứa logic: khi timer expires → set event → call runnable function

### Ai gọi Runnable: RTE hay application code?

**RTE gọi Runnable.** Application code không bao giờ tự gọi runnable function. Flow:

```
AUTOSAR OS (timer interrupt)
    │
    ▼
RTE (Rte_ReRunnable_10ms event fires)
    │
    ▼
Runnable_10ms()  ← C function do Simulink generate
    │
    ├── Rte_Read_xxx()   ← RTE API
    ├── Algorithm code   ← Simulink generated
    └── Rte_Write_xxx()   ← RTE API
```

---

## 6. RTE nằm ở đâu trong toàn bộ workflow?

### RTE do Simulink generate hay DaVinci generate?

**DaVinci/EB tresos generate RTE, KHÔNG PHẢI Simulink.**

Simulink chỉ generate **RTE API stub headers** (`Rte_ASWC.h`, `Rte_Type.h`) để cho phép testing (SIL/PIL) trong môi trường Simulink. Đây là **stub implementations** — không phải real RTE code.

| Thành phần | Ai generate | Mục đích |
|-----------|-------------|----------|
| `Rte_ASWC.h` (stub) | Simulink AUTOSAR Blockset | Cho SIL/PIL testing trong Simulink |
| `Rte_Type.h` (stub) | Simulink AUTOSAR Blockset | Data type definitions cho testing |
| `Rte.c` / `Rte.h` (real) | DaVinci RTE Generator / EB tresos | Real RTE runtime code |
| `Rte_ReRunnable_xxx` events | DaVinci RTE Generator | Event-triggered scheduling |

### Simulink generate phần nào của RTE interface?

Simulink generate **RTE API calls trong application C code** — tức là **caller side**, không phải **implementation side**:

```c
// Simulink generates (caller side):
void Runnable_10ms(void) {
    uint8 value;
    Rte_Read_RPort1_DataElement(&value);  // ← Simulink generate call này
    // ... algorithm ...
    Rte_Write_PPort1_Result(result);       // ← Simulink generate call này
}

// DaVinci/EB tresos generates (implementation side):
Std_ReturnType Rte_Read_RPort1_DataElement(uint8* data) {
    *data = Rte_Irv_RPort1_DataElement;  // ← Real implementation
    return RTE_E_OK;
}
```

### RTE Generator lấy thông tin từ ARXML nào?

RTE Generator đọc **SWC ARXML output từ Simulink** + **Composition ARXML** + **System ARXML**:

```
Input cho RTE Generator:
├── SWC_Component.arxml     ← Từ Simulink output
├── SWC_Interface.arxml     ← Từ Simulink output
├── SWC_Datatype.arxml      ← Từ Simulink output
├── SWC_Implementation.arxml ← Từ Simulink output
├── Composition.arxml        ← Từ system engineer
├── ECU_Extract.arxml        ← Từ system engineer
└── BSW_Configuration.arxml  ← Từ BSW team
```

### C code của Simulink gọi API nào của RTE?

Các RTE API chính mà Simulink generated code gọi:

| RTE API | Mục đích | Khi nào dùng |
|---------|---------|-------------|
| `Rte_Read_xxx()` | Đọc data từ Require Port | Inport mapped to RPort |
| `Rte_Write_xxx()` | Ghi data qua Provide Port | Outport mapped to PPort |
| `Rte_Call_xxx()` | Gọi Server operation | Function Caller → Client-Server |
| `Rte_Result_xxx()` | Nhận kết quả từ Server | Client-Server response |
| `Rte_Switch_xxx()` | Mode switch | Mode-Switch interface |
| `Rte_IrvWrite_xxx()` | Ghi Inter-Runnable Variable | Signal line → IRV |
| `Rte_IrvRead_xxx()` | Đọc Inter-Runnable Variable | Signal line → IRV |

### Rte_Read() / Rte_Write() xuất hiện từ đâu?

1. **Simulink generate** các `Rte_Read()` / `Rte_Write()` calls trong `SWC.c` — dựa trên AUTOSAR Mapping
2. **DaVinci/EB tresos RTE Generator** đọc ARXML (chứa port/IRV definitions) và generate **implementation** của các hàm này trong `Rte.c`
3. **Compiler** link 2 phần lại: `SWC.c` (caller) + `Rte.c` (implementation)

---

## 7. DaVinci và EB tresos nằm chính xác ở đâu?

### Nếu Simulink đã import ARXML rồi, tại sao DaVinci vẫn cần ARXML?

**Vì Simulink và DaVinci xử lý các phần KHÁC NHAU của ARXML.**

```
                    ARXML (toàn bộ hệ thống)
                   /                          \
                  /                            \
     ┌──────────────────────┐     ┌──────────────────────┐
     │    Simulink           │     │    DaVinci/EB tresos  │
     │    AUTOSAR Blockset   │     │                      │
     ├──────────────────────┤     ├──────────────────────┤
     │ Đọc: SWC ARXML       │     │ Đọc: System ARXML     │
     │   - SWC definition   │     │   - ECU topology      │
     │   - Ports/Interfaces │     │   - Network mapping   │
     │   - Data types       │     │   - BSW MD            │
     │   - Runnables        │     │   - BSW Config        │
     │                      │     │   - SWC ARXML (từ      │
     │ Output:              │     │     Simulink output)  │
     │   - SWC C code       │     │                      │
     │   - SWC ARXML output │────►│ Output:              │
     │   - Rte stub headers │     │   - RTE C code        │
     │                      │     │   - BSW C code        │
     │                      │     │   - OS configuration  │
     │                      │     │   - Build files       │
     └──────────────────────┘     └──────────────────────┘
```

### Source of truth là ai?

**ARXML là source of truth.** Không phải Simulink, không phải DaVinci.

- System ARXML (OEM tạo) = source of truth cho system topology
- SWC ARXML (Simulink output) = source of truth cho application behavior
- DaVinci/EB tresos consume tất cả ARXML để generate integration code

### ARXML nào đi vào Simulink?

Chỉ **SWC-level ARXML**:
- SWC component description
- SWC composition (nếu có)
- Interface definitions
- Data type definitions
- CompuMethods

### ARXML nào đi vào DaVinci/EB tresos?

**Tất cả**:
- System ARXML (topology, ECU mapping)
- ECU Extract ARXML
- BSW Module Description ARXML
- BSW Module Configuration ARXML
- **SWC ARXML output từ Simulink** (để generate RTE)
- Composition ARXML

### Có phải hai tool import cùng một ARXML không?

**Không cùng một file, nhưng có thể overlap.** Cụ thể:

| ARXML File | Simulink đọc? | DaVinci đọc? |
|-----------|:---:|:---:|
| System_Description.arxml | Không | Có |
| ECU_Extract.arxml | Không | Có |
| SWC_Component.arxml (input) | Có | Không (lấy output từ Simulink) |
| SWC_Interface.arxml (input) | Có | Không (lấy output từ Simulink) |
| SWC_Component.arxml (Simulink output) | Không | **Có** (cho RTE gen) |
| BSW_MD_Can.arxml | Không | Có |
| BSW_Config_Can.arxml | Không | Có |

### Sau khi mỗi tool generate xong, đồng bộ thế nào?

```
Round-Trip Sync Flow:

1. System Engineer → tạo ARXML (SWC description) → giao cho App team
2. App team → Import ARXML vào Simulink → phát triển algorithm → Generate code
   → Output: SWC C code + SWC ARXML (updated)
3. App team → giao SWC ARXML output cho Integration team
4. Integration team → Nạp SWC ARXML vào DaVinci/EB tresos
   → Generate RTE + Configure BSW → Build
5. Nếu có thay đổi ở system level → System Engineer export updated ARXML
   → App team re-import → Update model → Re-generate
   → (Quay lại bước 2)
```

---

## 8. C code được generate ở đâu?

### Tổng quan ai generate cái gì

```
┌──────────────────────────────────────────────────────────┐
│                    C Code Sources                        │
├──────────────┬──────────────────┬─────────────────────────┤
│  Simulink    │  DaVinci/EB      │  Chip Vendor            │
│  (Embedded   │  tresos          │                         │
│  Coder)      │                  │                         │
├──────────────┼──────────────────┼─────────────────────────┤
│ Application  │ RTE C code       │ MCAL C code             │
│ Algorithm C  │ (Rte.c, Rte.h)   │ (Can.c, Adc.c,         │
│ (SWC.c,     │ OS configuration │  Pwm.c, Gpt.c, ...)     │
│  SWC.h)      │ Task definitions │                         │
│              │ BSW module C code│ Hardware abstraction     │
│ Rte stub     │ (Can.c, Pwm.c,   │                         │
│ headers      │  Adc.c, ...)     │                         │
│ (Rte_*.h)    │                  │                         │
└──────────────┴──────────────────┴─────────────────────────┘
                         │
                         ▼
                 ┌──────────────┐
                 │  Integration  │
                 │  (Makefile/   │
                 │   CMake)      │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │   Compiler   │
                 │  (GCC/IAR/   │
                 │   GreenHills)│
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │  ELF / HEX   │
                 └──────────────┘
```

### Chi tiết từng phần

| Code Part | Generated By | Tool | File Examples | Vai trò |
|-----------|-------------|------|---------------|---------|
| **Application C** | Embedded Coder | Simulink AUTOSAR Blockset | `SWC.c`, `SWC.h`, `SWC_private.h` | Algorithm logic + RTE API calls |
| **RTE C** | RTE Generator | DaVinci / EB tresos | `Rte.c`, `Rte.h`, `Rte_*.c` | Communication layer giữa SWC và BSW |
| **BSW C** | BSW Generator | DaVinci / EB tresos | `Can.c`, `Pwm.c`, `Adc.c`, `Dio.c` | Basic Software modules |
| **MCAL C** | Chip Vendor | Vendor SDK | `Can_Hw.c`, `Adc_Hw.c` | Microcontroller abstraction |
| **OS** | OS Configurator | DaVinci / EB tresos + AUTOSAR OS | `Os.c`, `Os.h`, `Tasks.c` | Task scheduling, interrupts |
| **Rte stubs** | Embedded Coder | Simulink AUTOSAR Blockset | `Rte_ASWC.h`, `Rte_Type.h` | Stub cho SIL/PIL testing only |

### Cấu trúc project thực tế

```
ECU_Project/
├── Application/           ← Simulink generated
│   ├── SWC_EngineCtrl.c
│   ├── SWC_EngineCtrl.h
│   ├── SWC_BrakeCtrl.c
│   └── SWC_BrakeCtrl.h
├── RTE/                   ← DaVinci/EB tresos generated
│   ├── Rte.c
│   ├── Rte.h
│   ├── Rte_EngineCtrl.c   ← RTE cho từng SWC
│   └── Rte_BrakeCtrl.c
├── BSW/                   ← DaVinci/EB tresos configured
│   ├── Can/
│   │   ├── Can.c
│   │   └── Can_Cfg.h
│   ├── Pwm/
│   │   ├── Pwm.c
│   │   └── Pwm_Cfg.h
│   └── ...
├── MCAL/                  ← Chip vendor
│   ├── Can_Hw.c
│   └── Adc_Hw.c
├── OS/                    ← AUTOSAR OS (DaVinci/EB tresos configured)
│   ├── Os.c
│   └── Os_Cfg.h
├── Rte_Stubs/             ← Simulink generated (cho testing only)
│   ├── Rte_ASWC.h
│   └── Rte_Type.h
├── Makefile / CMake       ← Build system
└── LinkerScript.ld        ← Memory layout
```

### Simulink generate algorithm C code hay cả AUTOSAR wrapper?

Simulink generate **cả hai**:

1. **Algorithm C code**: Logic thực sự (calculations, state machines, lookup tables)
2. **AUTOSAR wrapper**: RTE API calls (`Rte_Read`, `Rte_Write`), runnable function signatures, initialization functions

```c
// Generated by Simulink - example SWC.c
#include "Rte_ASWC.h"  // ← Simulink generates stub header

/* Runnable: Runnable_10ms */
void Runnable_10ms(void) {
    /* RTE Read - generated based on mapping */
    float RPort_SensorValue;
    Rte_Read_RPort_SensorValue(&RPort_SensorValue);  // ← AUTOSAR wrapper

    /* Algorithm - generated from Simulink blocks */
    float result = RPort_SensorValue * 2.0 + 1.0;     // ← Algorithm code

    /* RTE Write - generated based on mapping */
    Rte_Write_PPort_Result(result);                   // ← AUTOSAR wrapper
}
```

---

## 9. ARXML output sau khi generate là gì?

### ARXML output có thay thế ARXML gốc không?

**Không thay thế.** ARXML output là **bổ sung/cập nhật** cho ARXML gốc, tập trung vào phần implementation.

### Flow ARXML

```
Original ARXML (từ System Engineer)
    │
    │  Chứa: SWC definition, ports, interfaces, data types
    │  KHÔNG chứa: implementation details, runnable behavior
    │
    ▼
Import vào Simulink
    │
    ▼
Developer viết algorithm
    │
    ▼
Generate (Ctrl+B)
    │
    ├── C code (SWC.c, SWC.h, ...)
    │
    └── ARXML Output (4 files):
        ├── SWC_component.arxml      ← SWC definition (updated)
        ├── SWC_interface.arxml       ← Communication interfaces
        ├── SWC_datatype.arxml        ← Data type definitions
        └── SWC_implementation.arxml  ← Implementation details (NEW)
```

### Bảng so sánh: ARXML Input vs Output

| Thông tin | ARXML Input (từ AAT) | ARXML Output (từ Simulink) |
|-----------|---------------------|---------------------------|
| SWC definition | Có (cơ bản) | Có (cập nhật với mapping) |
| Ports | Có | Có (giữ nguyên) |
| Interfaces | Có | Có (giữ nguyên) |
| Data types | Có | Có (giữ nguyên + internal types) |
| Runnables | Có (định nghĩa) | Có (cập nhật với access points) |
| **Implementation** | **Không** | **Có** (mô tả internal behavior chi tiết) |
| **Data access points** | **Không** | **Có** (Rte_Read/Rte_Write positions) |
| **Inter-runnable variables** | **Không** | **Có** |
| **IncludedDataTypeSet** | **Không** | **Có** (nếu có internal data types) |
| **Calibration parameters** | **Không** | **Có** (SwAddrMethods, CompuMethods) |
| System topology | Không liên quan | Không liên quan |
| BSW configuration | Không liên quan | Không liên quan |

### ARXML output chứa gì cụ thể?

Theo AUTOSAR Blockset User's Guide, Simulink generate 4 file ARXML:

1. **`SWC_component.arxml`**: `SWC-IMPLEMENTATION`, `SWC-INTERNAL-BEHAVIOR`, `RUNNABLE-ENTITY`, `RTE-EVENT`
2. **`SWC_interface.arxml`**: `SENDER-RECEIVER-INTERFACE`, `CLIENT-SERVER-INTERFACE`, `MODE-SWITCH-INTERFACE`, data elements
3. **`SWC_datatype.arxml`**: `APPLICATION-DATA-TYPE`, `IMPLEMENTATION-DATA-TYPE`, `COMPU-METHOD`, `SW-DATA-DEF-PROPS`
4. **`SWC_implementation.arxml`**: `DATA-WRITE-ACCESS`, `DATA-READ-ACCESS`, `PARAMETER-ACCESS`, `RUNNABLE-ENTITY` mapping

### DaVinci consume ARXML output như thế nào?

DaVinci/EB tresos đọc ARXML output từ Simulink để:

1. **Generate RTE**: Đọc `SWC_implementation.arxml` để biết runnable nào access port nào → generate `Rte_Read_xxx()` / `Rte_Write_xxx()` implementations
2. **Generate OS tasks**: Đọc `SWC_component.arxml` để biết runnable nào có `TimingEvent` → tạo OS task với period tương ứng
3. **Map SWC to ECU**: Đọc `SWC_component.arxml` để biết SWC nào nằm trên ECU nào → tạo task-to-SWC mapping
4. **Generate BSW configuration**: Đọc interface ARXML để biết SWC cần communication channel nào → configure BSW modules tương ứng

### Round-trip preservation

Simulink **giữ nguyên cấu trúc ARXML gốc** khi export — nghĩa là nếu ARXML input có element X, ARXML output cũng có element X (có thể thêm thông tin nhưng không xóa). Điều này cho phép DaVinci re-import ARXML output mà không mất thông tin.

---

## 10. Toàn bộ project thực tế chạy như thế nào?

### End-to-End Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                     OEM / Tier-1                             │
│                                                              │
│  System Engineer                                            │
│  (DaVinci / SystemDesk / PREEvision)                        │
│  ├── Định nghĩa ECU topology                                 │
│  ├── Định nghĩa network (CAN/LIN/FlexRay)                    │
│  ├── Định nghĩa System ARXML                                 │
│  ├── Extract ECU-specific ARXML                              │
│  └── Định nghĩa SWC description ARXML                        │
│         │                                                    │
│         ▼                                                    │
│  ┌─────────────────┐              ┌─────────────────┐       │
│  │   Application    │              │   BSW Team       │       │
│  │   Team           │              │                  │       │
│  │   (Simulink)     │              │   (DaVinci/EB)   │       │
│  │                  │              │                  │       │
│  │  1. Import ARXML │              │  1. Import BSW   │       │
│  │     (SWC desc)   │              │     MD ARXML     │       │
│  │                  │              │                  │       │
│  │  2. AUTOSAR      │              │  2. Configure    │       │
│  │     Quick Start  │              │     BSW modules  │       │
│  │     (auto-map)   │              │     (Can, Pwm,   │       │
│  │                  │              │      Adc, ...)   │       │
│  │  3. Write algo   │              │                  │       │
│  │     (Stateflow,  │              │  3. Configure    │       │
│  │      LUT, calc)  │              │     OS tasks     │       │
│  │                  │              │                  │       │
│  │  4. Simulate     │              │                  │       │
│  │     (B2B test)   │              │                  │       │
│  │                  │              │                  │       │
│  │  5. Generate     │              │                  │       │
│  │     C code +     │──── ARXML ──►│  4. Import SWC   │       │
│  │     ARXML output │     output   │     ARXML output │       │
│  │                  │              │     (from App)   │       │
│  └─────────────────┘              │                  │       │
│                                   │  5. Generate RTE │       │
│                                   │     (Rte.c/h)    │       │
│                                   │                  │       │
│                                   │  6. Generate BSW │       │
│                                   │     C code       │       │
│                                   │                  │       │
│                                   │  7. Integration  │       │
│                                   │     (Makefile)   │       │
│                                   └────────┬─────────┘       │
│                                            │                  │
│                                            ▼                  │
│                                   ┌────────────────┐         │
│                                   │  Compiler       │         │
│                                   │  (GCC/IAR/GHS)  │         │
│                                   └────────┬────────┘         │
│                                            │                  │
│                                            ▼                  │
│                                   ┌────────────────┐         │
│                                   │  ELF / HEX      │         │
│                                   │  → Flash vào    │         │
│                                   │    ECU          │         │
│                                   └────────────────┘         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Timeline thực tế của một dự án

| Phase | Hoạt động | Tool | Output |
|-------|-----------|------|--------|
| **1. System Design** | Định nghĩa topology, SWC, interfaces | DaVinci/SystemDesk | System ARXML, SWC ARXML |
| **2. SWC Import** | Import SWC ARXML vào Simulink | Simulink | Model skeleton |
| **3. Auto-Mapping** | Quick Start auto-map ports/functions | Simulink | Code Mappings |
| **4. Algorithm Dev** | Viết thuật toán trong Simulink | Simulink | Complete model |
| **5. Simulation** | Simulate, B2B test | Simulink Test | Test results |
| **6. Code Generation** | Generate C code + ARXML output | Simulink (Embedded Coder) | SWC.c, SWC ARXML |
| **7. BSW Configuration** | Configure BSW modules | DaVinci/EB tresos | BSW C code |
| **8. RTE Generation** | Generate RTE from ARXML | DaVinci/EB tresos | Rte.c, Rte.h |
| **9. Integration** | Combine all code, create Makefile | DaVinci/EB tresos | Build project |
| **10. Compilation** | Compile → ELF/HEX | Compiler | Binary |
| **11. HIL/Flash** | Flash vào ECU, test trên bench/vehicle | HIL / ECU | Validated ECU |
| **12. Round-Trip** | Nếu có thay đổi → quay lại Phase 1 hoặc 2 | — | Updated ARXML |

### Cấu trúc phần mềm cuối cùng trong ECU

```
ECU Flash Memory
├── .text (Code section)
│   ├── Application SWC code (from Simulink)
│   │   ├── SWC_EngineCtrl.c → Runnable_10ms(), Runnable_100ms()
│   │   └── SWC_BrakeCtrl.c → Runnable_20ms()
│   ├── RTE code (from DaVinci/EB tresos)
│   │   ├── Rte.c → Rte_Read(), Rte_Write(), Rte_Call()
│   │   └── Rte_EngineCtrl.c → Event handling
│   ├── BSW code (from DaVinci/EB tresos)
│   │   ├── Can.c → CAN communication
│   │   ├── Pwm.c → PWM output
│   │   ├── Adc.c → ADC input
│   │   ├── Dio.c → Digital I/O
│   │   └── ...
│   ├── MCAL code (from chip vendor)
│   │   ├── Can_Hw.c → Register access
│   │   └── Adc_Hw.c
│   ├── OS code (AUTOSAR OS)
│   │   ├── Os.c → Scheduler
│   │   └── Tasks.c → Task definitions
│   └── Startup code
│       └── startup.s → Boot sequence
├── .data (Initialized data)
├── .bss (Uninitialized data)
├── .cal (Calibration data - A2L mapped)
│   └── Parameters accessible via XCP/CCP
└── .rodata (Constants, lookup tables)
```

### Runtime execution flow

```
Power ON
    │
    ▼
Hardware Reset
    │
    ▼
Startup Code (assembly)
    │
    ▼
Os_Init() ── OS initialization
    │
    ▼
Rte_Start() ── RTE initialization
    │
    ▼
Runnable_Initialize() ── SWC initialization (from Simulink)
    │
    ▼
┌─── Main Loop (OS Scheduler) ───────────────────────────┐
│                                                        │
│  Timer Interrupt (10ms)                                │
│    │                                                   │
│    ▼                                                   │
│  OS Task: Task_10ms                                    │
│    │                                                   │
│    ▼                                                   │
│  RTE Event: Rte_ReRunnable_10ms fires                  │
│    │                                                   │
│    ▼                                                   │
│  Runnable_10ms() (from Simulink generated code)        │
│    ├── Rte_Read_SensorValue() ──► RTE ──► BSW ──► MCAL │
│    ├── Algorithm (calculate, state machine)            │
│    └── Rte_Write_ControlOutput() ──► RTE ──► BSW ──►   │
│                                    MCAL ──► Hardware   │
│                                                        │
│  Timer Interrupt (100ms)                               │
│    │                                                   │
│    ▼                                                   │
│  OS Task: Task_100ms                                   │
│    │                                                   │
│    ▼                                                   │
│  RTE Event: Rte_ReRunnable_100ms fires                 │
│    │                                                   │
│    ▼                                                   │
│  Runnable_100ms() (from Simulink generated code)       │
│    └── ...                                            │
│                                                        │
└────────────────────────────────────────────────────────┘
```

---

## Tóm tắt: Bản đồ tổng thể

```
     OEM / System Engineer
            │
            ▼
     System ARXML (topology, SWC desc, interfaces, data types)
            │
     ┌──────┴──────┐
     │             │
     ▼             ▼
  Simulink     DaVinci/EB tresos
  (App team)   (BSW team)
     │             │
     │ Import      │ Import BSW MD
     │ SWC ARXML   │ + System ARXML
     │             │
     │ Write algo  │ Configure BSW
     │ Simulate    │ Configure OS
     │ B2B test    │
     │             │
     │ Generate    │
     │ C code +    │
     │ SWC ARXML ──┼──► Import SWC ARXML output
     │ (output)    │              │
     │             │              ▼
     │             │    Generate RTE (Rte.c/h)
     │             │    Generate BSW C code
     │             │
     │             ▼
     │    Integration
     │    (Application + RTE + BSW + MCAL + OS)
     │             │
     │             ▼
     │        Compiler
     │             │
     │             ▼
     │        ELF / HEX
     │             │
     │             ▼
     │        Flash to ECU
     │             │
     │             ▼
     │        HIL / Vehicle Test
     │             │
     │     ┌───────┘
     │     │
     ▼     ▼
  Round-Trip: Nếu cần thay đổi → Update ARXML → Re-import → Re-generate
```

### 3 nguyên tắc cốt lõi cần nhớ

1. **ARXML là source of truth** — không phải Simulink, không phải DaVinci. ARXML định nghĩa cấu trúc, các tool chỉ consume và generate.

2. **Mỗi tool có trách nhiệm riêng, không overlap**:
   - Simulink: Application algorithm + SWC ARXML output
   - DaVinci/EB tresos: RTE + BSW + OS configuration
   - Compiler: Binary
   - Chip vendor: MCAL

3. **Mapping là cầu nối giữa Simulink và AUTOSAR** — không có mapping, Simulink model chỉ là model bình thường, không thể generate AUTOSAR-compliant code.

---

*Tham khảo: AUTOSAR Blockset User's Guide R2025b (MathWorks), AUTOSAR Classic Platform specifications (AUTOSAR.org)*
