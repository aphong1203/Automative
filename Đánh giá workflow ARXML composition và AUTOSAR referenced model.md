# Đánh giá workflow ARXML composition và AUTOSAR referenced model

## Kết luận trực tiếp

Nhận định tổng thể là **đúng hướng**, đặc biệt ở hai điểm: không nên ép ARXML cấp composition/top vào một model con vốn chỉ là thuật toán nội bộ; và model chọn **Model referenced from AUTOSAR software component model** không đại diện cho một atomic SWC độc lập có AUTOSAR ports riêng. Tuy nhiên, cần chỉnh ba ý để tránh kết luận quá mạnh:

1. Import thất bại không phải chỉ vì “Model Reference không có AUTOSAR port”, mà vì đang dùng **sai cấp mô tả** hoặc model hiện có không thỏa mapping/interface của atomic component cần liên kết.
2. “Data Dictionary trống” chưa đủ để kết luận thiếu mapping; cần xác định đang xem **Simulink Data Dictionary**, **Architectural Data**, hay **AUTOSAR Dictionary/Code Mappings**.
3. Chọn `autosar.tlc`, Validate thành công và build được chỉ chứng minh model hiện tại hợp lệ theo cấu hình AUTOSAR của chính nó; các bước này **không chứng minh model tương đương với ARXML gốc**.[^1][^2][^3]

## Cấp composition và component

ARXML composition mô tả cấu trúc hệ thống: composition ports, component prototypes và connectors. MathWorks cung cấp workflow riêng để import composition vào AUTOSAR architecture model hoặc tạo composition model; khi import đầy đủ internal behavior, công cụ có thể tạo model cho từng atomic software component bên trong.[^4][^5][^6]

Vì vậy, một model con được tách từ `Subsystem Reference` không tự động trở thành atomic SWC tương ứng trong ARXML. Nếu subsystem chỉ triển khai một phần thuật toán bên trong SWC, không có component prototype tương ứng trong ARXML, thì không tồn tại một AUTOSAR component-level contract để import trực tiếp vào model con. Khi đó, việc ép composition ARXML vào model này là sai abstraction level.

Tuy vậy, câu “Model Reference không thể khớp ARXML top nên import fail là đúng bản chất” cần diễn đạt mềm hơn. AUTOSAR workflow vẫn cho phép composition import **liên kết các atomic component bên trong với các component models có sẵn**; điều kiện là model hiện có thực sự triển khai đúng component tương ứng và có mapping/interface đầy đủ. Nói cách khác, `Model Reference` không tự nó là nguyên nhân thất bại; nguyên nhân là model con không đại diện đúng atomic SWC hoặc không tương thích với contract mà importer đang cố áp dụng.[^7][^8]

## Hai lựa chọn Quick Start

| Lựa chọn | Ý nghĩa | Có AUTOSAR component ports riêng? | Nơi cần kiểm tra |
|---|---|---:|---|
| **Application** + **Create defaults based on the Simulink model** | Tạo một AUTOSAR software component độc lập từ nội dung model; Quick Start áp dụng các AUTOSAR default để tạo component interface properties.[^9] | Có; root Inport/Outport có thể được map thành receiver/sender ports và interface elements.[^10][^11] | AUTOSAR Dictionary và các tab Inports/Outports/Runnables trong Code Mappings |
| **Model referenced from AUTOSAR software component model** | Cấu hình submodel dùng bên trong một AUTOSAR component model; tập trung vào internal data của submodel.[^12][^13] | Không tạo một AUTOSAR atomic component boundary độc lập cho submodel | Parameters, Signals/States và Data Stores trong Code Mappings |

Do đó, phần nhận định về option thứ hai là đúng: submodel referenced có thể map parameter thành `PerInstanceParameter`, và signal/state/data store thành `ArTypedPerInstanceMemory`; ARXML/code hỗ trợ calibration của phần này được tạo khi build component model cha.[^12][^14]

Cần sửa nhẹ câu “option này chỉ map data nội bộ”: đây là **phạm vi AUTOSAR mapping mà workflow cho submodel cung cấp**, không có nghĩa model con không thể có Simulink root Inport/Outport để nối qua Model block. Các Inport/Outport ấy là interface của model-reference trong Simulink, không phải AUTOSAR R-Port/P-Port độc lập của một SWC.

## Dictionary trống nghĩa gì

“Data Dictionary” trong Simulink có thể chỉ các nơi lưu trữ khác nhau, nên đây là điểm cần xác minh trước tiên:

| Nơi đang xem | Nội dung kỳ vọng | Trống có bất thường không? |
|---|---|---|
| **Simulink Data Dictionary – Design Data** | `Simulink.Parameter`, bus/type objects, constants và dữ liệu thiết kế | Không nhất thiết bất thường; Quick Start không buộc phải đẩy AUTOSAR component ports vào mục này |
| **Architectural Data** trong `.sldd` | Interfaces, data types, constants và AUTOSAR-specific architectural data dùng chung giữa component/composition.[^15][^16] | Trống nếu model chưa link/import architectural dictionary hoặc đang dùng AUTOSAR elements cục bộ trong model |
| **AUTOSAR Dictionary** | AUTOSAR component, interfaces, ports, runnables, internal behavior và XML options của model.[^1] | Với Application/defaults, thường phải có component/interface/port được tạo; với referenced submodel, không nên kỳ vọng component ports riêng |
| **Code Mappings** | Quan hệ giữa Simulink elements và AUTOSAR elements; với component model có Inports/Outports, với referenced submodel chủ yếu là internal data.[^17][^18] | Đây mới là nơi quyết định model element nào thực sự được map |

Vì vậy, câu “Application + defaults thì biến phải hiện trong Data Dictionary” chưa hoàn toàn chính xác. Kỳ vọng đúng là **AUTOSAR Dictionary và Code Mappings phải có cấu hình component/interface/port tương ứng**; còn `.sldd` Design Data có thể vẫn trống. Nếu đang dùng Architectural Data workflow thì interface/type có thể nằm trong phần Architectural Data của `.sldd`, nhưng điều đó phụ thuộc cách model được cấu hình.[^15][^19]

## `autosar.tlc` có đảm bảo không

Không. `autosar.tlc` là điều kiện để model dùng AUTOSAR Classic code-generation workflow, nhưng nó không mang theo identity hay semantic traceability tới ARXML ban đầu. Code Mappings mới quy định Simulink Inports, Outports và entry-point functions được map tới AUTOSAR ports, data elements và runnables nào; các mapping đó ảnh hưởng tới C code và ARXML được export.[^2][^10]

Nút **Validate** hoặc `autosar.api.validateModel(model)` kiểm tra AUTOSAR properties và Simulink-to-AUTOSAR mapping của model hiện tại. Validation không phải phép so sánh equivalence với file ARXML composition đã nhận từ OEM/Tier-1. Vì thế, “build pass + Validate pass” là **necessary**, nhưng chưa **sufficient** để tuyên bố mapping đúng với ARXML gốc.[^3]

Muốn xác nhận contract phải đối chiếu ít nhất:

- Atomic SWC/component prototype đúng, không dùng nhầm composition boundary.
- R-Port/P-Port direction và tên/qualified path.
- Port interface kind: Sender-Receiver, Client-Server, Mode-Switch, Parameter, Trigger, v.v.[^20]
- Data element/operation và data access mode.
- Application data type, implementation data type, mapping set, dimension, enum, scaling, unit và constraints; importer bảo tồn data-type mapping khi import ARXML.[^21]
- Runnables, events, timing và mapping entry-point.
- Calibration/PIM/memory-section attributes nếu thuộc phạm vi verification.

## Đánh giá hướng SIL

Hướng SIL cho referenced submodel là **đúng theo tài liệu MathWorks**: đặt Model block ở `Software-in-the-Loop (SIL)` hoặc `Processor-in-the-Loop (PIL)`, đặt **Code interface = Model reference**, và build parent AUTOSAR component trước để sinh RTE headers. Nếu chưa build parent thì SIL/PIL của subcomponent sẽ thất bại.[^22]

Có một điểm cần gọi tên chính xác: “model cha” ở đây nên là **AUTOSAR atomic component model trực tiếp sở hữu Model block**, không nhất thiết là composition/top architecture model ngoài cùng. Composition có thể chứa nhiều SWC, nhưng RTE-facing boundary và generated component code thuộc atomic SWC implementation chứa submodel.

Trong workflow này, unit SIL của referenced submodel phù hợp để verify thuật toán/code của phần con trong context code-generation của parent. Nó không biến submodel thành một AUTOSAR SWC độc lập và không chứng minh rằng submodel tự nó có ARXML port contract.

## Đánh giá hướng standalone

Nếu chọn **Application + defaults**, model con sẽ trở thành một AUTOSAR component độc lập do Simulink author ra. Defaults tạo cấu hình interface dựa trên root ports hiện có, nhưng đó là một **contract mới được suy ra từ model**, không phải contract được chứng minh là giống component trong ARXML gốc.[^9][^2]

Do đó, nhận định “B2B mới verify logic thuật toán, chưa chắc đúng 100% interface thật” là chính xác. Thậm chí nên nói rõ hai phạm vi kiểm thử:

| Phạm vi | Standalone Application/defaults có thể chứng minh | Không tự chứng minh |
|---|---|---|
| Algorithm/B2B | Output numerically tương đương cho cùng input, calibration và initial state | Integration semantics ngoài model |
| Generated code | Code standalone build và chạy được | RTE API đúng với component contract gốc |
| Interface | Root port shape/type của contract tự tạo | Port/interface identity, data mapping set, runnable/event và RTE semantics từ ARXML gốc |

Nếu ARXML có atomic SWC tương ứng, hướng tốt hơn là import chính atomic component đó bằng `createComponentAsModel`, hoặc import composition rồi link đúng existing component model. `arxml.importer` phân biệt rõ `createComponentAsModel` cho atomic SWC và `createCompositionAsModel` cho composition.[^23][^24]

Nếu ARXML không có atomic SWC tương ứng vì subsystem chỉ là internal decomposition của một SWC, thì không nên đặt mục tiêu “mapping model con với ARXML top”. Contract có thẩm quyền vẫn là mapping của **parent SWC**; model con chỉ cần đúng model-reference interface và internal-data configuration.

## Workflow khuyến nghị

### Trường hợp A: Model con chỉ là thuật toán nội bộ

1. Giữ **Model referenced from AUTOSAR software component model**.
2. Kiểm tra nó được reference bởi đúng atomic AUTOSAR parent model.
3. Trong submodel, kiểm tra Code Mappings cho Parameters, Signals/States và Data Stores nếu cần calibration/measurement.[^14][^12]
4. Trong parent, kiểm tra AUTOSAR Dictionary và Code Mappings cho ports, interfaces, runnables và events.
5. Validate parent bằng UI hoặc `autosar.api.validateModel(parentModel)`.[^3]
6. Build parent để tạo RTE headers.
7. Trên Model block, đặt **Simulation mode = SIL**, **Code interface = Model reference**, rồi chạy unit test.[^22]
8. Chạy thêm integration SIL ở parent SWC để kiểm tra RTE-facing behavior.

### Trường hợp B: Model con phải là SWC độc lập

1. Xác định atomic SWC qualified name trong bộ ARXML; không dùng composition qualified name.
2. Nếu atomic SWC tồn tại, ưu tiên import component đó hoặc link model hiện có vào component tương ứng của composition.[^7][^23]
3. Chỉ dùng **Application + defaults** khi chấp nhận author một contract mới.
4. Nếu dùng defaults, lập interface cross-check giữa contract mới và ARXML nguồn: ports, interfaces, elements, types, access modes, runnables và events.
5. Validate, build, export ARXML, rồi review/diff về semantic contract; không chỉ so text vì package ordering và file partition có thể khác.

## Các kiểm tra nhanh

Có thể dùng các lệnh sau để xác nhận trạng thái, với tên model được thay bằng model thực tế:

```matlab
mdl = "YourModel";
open_system(mdl);

% Xác nhận target
get_param(mdl,"SystemTargetFile")

% Nếu model có AUTOSAR mapping, lấy mapping object
slMap = autosar.api.getSimulinkMapping(mdl);

% Validate AUTOSAR properties và mappings hiện tại
autosar.api.validateModel(mdl);
```

Đối với component model độc lập, UI cần có các tab Inports/Outports và mapping tới AUTOSAR sender/receiver ports. AUTOSAR sender-receiver workflow yêu cầu root Inport/Outport được map rõ tới port và data element, sau đó Validate.[^10][^11]

Đối với referenced submodel, kỳ vọng chính là internal-data mappings. Việc không thấy AUTOSAR sender/receiver ports riêng trong submodel là phù hợp thiết kế; việc parent không có port mapping mới là vấn đề.

## Phán quyết cho từng nhận định

| Nhận định | Đánh giá | Điều chỉnh |
|---|---|---|
| ARXML top/composition không map 1:1 vào subsystem/model con | **Đúng** | Nêu rõ khác abstraction level và model con có thể không phải atomic SWC |
| Import fail là tất yếu vì Model Reference không có AUTOSAR port | **Đúng một phần** | Model Reference vẫn có thể implement atomic component khi được link đúng; fail phụ thuộc target component và compatibility |
| Application + defaults tạo interface từ model | **Đúng** | Interface nằm trong AUTOSAR configuration; không nhất thiết xuất hiện trong `.sldd` Design Data |
| Referenced-submodel option không tạo AUTOSAR component ports riêng | **Đúng** | Nó vẫn có Simulink model-reference I/O; AUTOSAR-facing ports thuộc parent SWC |
| Dictionary trống là by-design | **Có điều kiện** | Đúng nếu nói AUTOSAR ports trong referenced submodel; chưa chắc đúng nếu đang nói internal mappings hoặc parent dictionary |
| Unit SIL phải build parent, Code interface = Model reference | **Đúng** | Parent là atomic component model chứa Model block.[^22] |
| `autosar.tlc` đảm bảo mapping với ARXML | **Sai** | Chỉ là target; cần mapping, validation và contract-level đối chiếu |
| Defaults chỉ đủ cho algorithm B2B, chưa đủ chứng minh integration interface | **Đúng** | Cần thêm parent/integration SIL và đối chiếu atomic component ARXML |

Như vậy, phương án nên ưu tiên là: **nếu model con chỉ là internal decomposition, giữ referenced-submodel và lấy parent atomic SWC làm AUTOSAR contract authority; nếu model con thật sự phải là SWC độc lập, phải có hoặc xây dựng component-level ARXML tương ứng, không dùng composition-level ARXML để chứng minh interface.**

---

## References

1. [Configure AUTOSAR Elements and Properties](https://www.mathworks.com/help/autosar/ug/configure-autosar-component-using-autosar-properties-explorer.html) - The component view in the AUTOSAR Dictionary displays the name and type of the selected component, a...

2. [Code Mappings Editor - Map AUTOSAR elements for code ...](https://www.mathworks.com/help/autosar/ref/autosarcodemappingseditor.html) - The mappings that you configure are reflected in generated AUTOSAR-compliant C or C++ code and expor...

3. [autosar.api.validateModel](https://www.mathworks.com/help/autosar/ref/autosar.api.validatemodel.html) - This MATLAB function validates the AUTOSAR properties and Simulink to AUTOSAR mapping of model. the ...

4. [Import AUTOSAR Classic Composition from ARXML to ...](https://www.mathworks.com/help/autosar/ug/import-autosar-composition-from-arxml.html) - To import a composition, open the AUTOSAR Importer app or use function importFromARXML . If you do n...

5. [Import AUTOSAR Composition into Architecture Model](https://www.mathworks.com/help/autosar/ug/example-import-autosar-composition-into-architecture-model.html) - In the open architecture model, on the Modeling tab, in the Component menu, select Import from ARXML...

6. [Import AUTOSAR Composition to Simulink](https://www.mathworks.com/help/autosar/ug/example-import-autosar-composition-to-simulink.html) - Use the MATLAB function createCompositionAsModel to import the AUTOSAR XML (ARXML) description and c...

7. [importFromARXML - Import compositions and components ...](https://www.mathworks.com/help/autosar/ref/autosar.arch.composition.model.importfromarxml.html) - Import AUTOSAR compositions and link them to existing component models. Create an AUTOSAR architectu...

8. [createCompositionAsModel](https://www.mathworks.com/help/autosar/ref/arxml.importer.createcompositionasmodel.html) - Import AUTOSAR software composition /pkg/rootComposition from ARXML file mySWCs.arxml and create an ...

9. [Create AUTOSAR Software Component in Simulink](https://www.mathworks.com/help/autosar/ug/create-an-autosar-software-component-in-simulink.html) - If you select Create defaults based on the Simulink model, the software creates component interface ...

10. [Configure AUTOSAR Sender-Receiver Communication - MathWorks](https://www.mathworks.com/help/autosar/ug/configure-autosar-sender-receiver-communication.html) - Read and write AUTOSAR data by using port-based sender-receiver communication.

11. [Model AUTOSAR Communication - MATLAB & Simulink](https://www.mathworks.com/help/autosar/ug/autosar-communication.html) - Model communication between AUTOSAR software components with AUTOSAR ports and interfaces.

12. [Map Calibration Data for Submodels Referenced from ...](https://www.mathworks.com/help/autosar/ug/map-calibration-data-for-submodels.html) - Use Code Mappings editor to view and map internal data for submodels referenced from AUTOSAR compone...

13. [autosar.api.create - Create or update mapped ...](https://www.mathworks.com/help/autosar/ref/autosar.api.create.html) - This MATLAB function creates or updates mapped AUTOSAR software component model model.

14. [Configure Subcomponent Data for AUTOSAR Calibration ...](https://www.mathworks.com/help/autosar/ug/example-configure-subcomponent-data.html) - For any model in an AUTOSAR model reference hierarchy, you can configure the model data for run-time...

15. [Manage AUTOSAR Architectural Data With Data Dictionaries](https://www.mathworks.com/help/autosar/ug/manage-autosar-architectural-data.html) - Manage architectural data in AUTOSAR models using the Architectural Data Editor.

16. [Programmatically Manage AUTOSAR Architectural Data](https://www.mathworks.com/help/autosar/ug/program-manage-autosar-architectural-data.html) - This example shows how to programmatically manage shared data types, interfaces, constants, software...

17. [Map AUTOSAR Elements for Code Generation - MathWorks](https://www.mathworks.com/help/autosar/ug/map-model-elements-using-simulink-autosar-mapping-explorer.html) - Use Code Mappings editor to view and map AUTOSAR software component from Simulink perspective.

18. [AUTOSAR Component Configuration - MATLAB & Simulink](https://www.mathworks.com/help/autosar/ug/autosar-interface-configuration.html) - In the Code Mappings editor, click the Validate button to validate the AUTOSAR component configurati...

19. [Update Architectural Data Section of Data Dictionary from ...](https://www.mathworks.com/help/autosar/ug/updating-an-autosar-software-component.html) - You link the updated data dictionary to component or composition models. ... Import AUTOSAR Package ...

20. [AUTOSAR Communication - MATLAB & Simulink](https://www.mathworks.com/help/autosar/autosar-communication.html) - Configure communication ports and interfaces

21. [Model AUTOSAR Data Types - MATLAB & Simulink](https://www.mathworks.com/help/autosar/ug/data-types.html) - When you import AUTOSAR components from ARXML files into Simulink, the AUTOSAR data type description...

22. [Verify AUTOSAR Code with SIL and PIL Simulations - MathWorks](https://www.mathworks.com/help/autosar/ug/verifying-the-autosar-code-with-sil-and-pil-simulations.html) - Verify AUTOSAR software component code with software-in-the-loop (SIL) and processor-in-the-loop (PI...

23. [arxml.importer - Import AUTOSAR XML descriptions of ...](https://www.mathworks.com/help/autosar/ref/arxml.importer.html) - Use arxml.importer functions to import AUTOSAR software components, compositions, or packages of sha...

24. [Simulink への AUTOSAR XML 記述のインポート](https://jp.mathworks.com/help/autosar/ug/importing-an-autosar-software-component.html) - AUTOSAR ソフトウェア コンポーネント、コンポジション、または共有要素のパッケージを ARXML ファイルから Simulink にインポートする。

