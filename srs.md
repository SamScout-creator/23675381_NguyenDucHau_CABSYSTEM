# BÁO CÁO ĐẶC TẢ YÊU CẦU PHẦN MỀM (SRS)
## ĐỀ TÀI: NỀN TẢNG ĐẶT XE TRỰC TUYẾN - CAB SYSTEM
- **Đơn vị đầu tư:** Công ty Cổ phần Vận tải ABC
- **Thời hạn triển khai:** 7 tuần (Triển khai MVP)
- **Phương pháp luận:** Phân tích & Đặc tả yêu cầu phần mềm theo tiến trình chuẩn Business Analysis (BABOK / Software Engineering Lifecycle)

---

## BƯỚC 1: XÁC ĐỊNH PHẠM VI DỰ ÁN & MỤC TIÊU KINH DOANH (PROJECT SCOPE & OBJECTIVES)

### 1.1 Vấn đề tồn đọng của hiện trạng (Problem Statement)
- **Điều phối thủ công:** Tổng đài viên và ứng dụng sơ khai gán cuốc thủ công gây nghẽn cổ chai, chậm trễ trong giờ cao điểm.
- **Trải nghiệm thiếu minh bạch:** Khách hàng không xem được vị trí tài xế theo thời gian thực (real-time tracking), không rõ thời gian dự kiến đón (ETA).
- **Quản lý dữ liệu phân tán:** Thông tin chuyến đi, cước phí và thanh toán chưa được đồng bộ tập trung, tiềm ẩn rủi ro sai lệch tài chính.
- **Khả năng mở rộng hạn chế:** Hệ thống cũ không hỗ trợ mở rộng phương thức thanh toán, thêm dịch vụ mới hoặc chịu tải đột biến.

### 1.2 Mục tiêu dự án (Business Objectives)
- Xây dựng nền tảng CAB System tự động hóa toàn bộ quy trình: Đặt xe $ightarrow$ Điều phối $ightarrow$ Di chuyển $ightarrow$ Tính cước $ightarrow$ Thanh toán $ightarrow$ Đánh giá.
- Rút ngắn thời gian gán tài xế xuống dưới 3 giây bằng thuật toán tự động.
- Triển khai và bàn giao phiên bản MVP hoạt động ổn định trong **7 tuần**.

---

## BƯỚC 2: PHÂN TÍCH CÁC BÊN LIÊN QUAN & MA TRẬN STAKEHOLDER (STAKEHOLDER MATRIX)

### 2.1 Bảng phân tích chi tiết Stakeholders
| Nhóm Stakeholder | Vai trò trong hệ thống | Kỳ vọng / Mục tiêu cốt lõi | Quyền lực (Power) | Mức độ quan tâm (Interest) |
| :--- | :--- | :--- | :---: | :---: |
| **Ban Giám Đốc ABC** | Chủ đầu tư (Sponsor) | Dự án kịp tiến độ 7 tuần, có khả năng mở rộng, báo cáo doanh thu & tỷ lệ hủy minh bạch. | Rất cao | Rất cao |
| **Trưởng phòng Vận hành** | Quản lý nghiệp vụ (Business Owner) | Giám sát toàn diện xe trên tuyến, phân quyền rõ ràng, hỗ trợ can thiệp cuốc lỗi tức thời. | Cao | Rất cao |
| **Khách hàng (Rider)** | Người dùng dịch vụ | Đặt xe nhanh chóng, cước phí rõ ràng, tài xế đón đúng giờ, an toàn. | Thấp | Rất cao |
| **Tài xế (Driver)** | Đối tác cung cấp dịch vụ | Nhận cuốc công bằng, giao diện 1-chạm an toàn khi lái xe, thu nhập rõ ràng. | Thấp | Rất cao |
| **Nhân viên Vận hành / CS** | Người vận hành tác nghiệp | Giao diện điều phối trực quan, tra cứu sự cố tức thì, thao tác nhanh. | Thấp | Cao |
| **Đối tác PGW / Map / Push** | Bên cung cấp hạ tầng thứ ba | Tích hợp đúng chuẩn API, bảo mật thông tin tài khoản thẻ theo chuẩn SLA. | Cao | Vừa |

### 2.2 Ma trận Phân tích Stakeholder (Mermaid Quadrant Chart)

```mermaid
quadrantChart
    title Ma trận Stakeholder CAB System (Power vs Interest)
    x-axis "Mức độ quan tâm thấp" --> "Mức độ quan tâm cao"
    y-axis "Quyền lực thấp" --> "Quyền lực cao"
    quadrant-1 "Làm hài lòng (Keep Satisfied)"
    quadrant-2 "Quản lý chặt chẽ (Manage Closely)"
    quadrant-3 "Theo dõi tối thiểu (Monitor)"
    quadrant-4 "Thông báo thường xuyên (Keep Informed)"
    "Ban Giám Đốc ABC": [0.88, 0.92]
    "Trưởng phòng Vận hành": [0.85, 0.82]
    "Đối tác Cổng Thanh Toán / Map": [0.35, 0.70]
    "Khách hàng": [0.85, 0.28]
    "Tài xế": [0.82, 0.30]
    "Nhân viên Điều hành / CS": [0.75, 0.38]
    "Đội ngũ Dev / BA / QA": [0.90, 0.50]
```

---

## BƯỚC 3: XÁC ĐỊNH CÁC TÁC NHÂN HỆ THỐNG (SYSTEM ACTORS)

1. **Khách hàng (Customer / Rider):** Người đăng ký tài khoản, gửi yêu cầu đặt xe, theo dõi vị trí xe, thanh toán và đánh giá cuốc xe.
2. **Tài xế (Driver):** Đối tác nhận chuyến, cập nhật trạng thái di chuyển (Đã đến điểm đón $ightarrow$ Đã đón khách $ightarrow$ Hoàn thành), truyền tọa độ GPS định kỳ.
3. **Nhân viên Vận hành (Operations Staff):** Giám sát trạng thái xe trên bản đồ, hỗ trợ giải quyết sự cố, tra cứu lịch sử giao dịch.
4. **Quản trị viên (Admin):** Quản trị danh mục xe/tài xế, cấu hình biểu giá cước, quản lý phân quyền RBAC và trích xuất báo cáo kinh doanh.
5. **Cổng thanh toán bên ngoài (External Payment Gateway):** Hệ thống của bên thứ ba xử lý giao dịch trừ tiền thẻ/ví điện tử.
6. **Dịch vụ Bản đồ & Định vị (Map & Routing Service):** Cung cấp API tính lộ trình, gợi ý địa chỉ và ước tính khoảng cách/thời gian di chuyển.

---

## BƯỚC 4: CÁC QUY TẮC NGHIỆP VỤ (BUSINESS RULES - BR)
*Hệ thống bắt buộc tuân thủ 5 quy tắc nghiệp vụ bất biến sau:*

- **BR-01 (Điều kiện sẵn sàng của tài xế):** Tài xế chỉ được nhận tín hiệu cuốc xe mới khi thỏa mãn đồng thời: (1) Tài khoản đang ở trạng thái kích hoạt, (2) Đã bật chế độ `Online`, (3) Không đang trong bất kỳ hành trình nào (`Active Trip = False`).
- **BR-02 (Cơ chế chuyển tiếp đơn cuốc - Failover Sequential Dispatch):** Mỗi cuốc xe chỉ được chuyển tới **duy nhất 01 tài xế** tối ưu nhất tại một thời điểm. Nếu tài xế chủ động ấn "Từ chối" hoặc không bấm phản hồi trong thời gian quy định (15 giây), hệ thống tự động loại tài xế này và chuyển tiếp cuốc xe đến tài xế phù hợp tiếp theo mà không yêu cầu khách hàng đặt lại.
- **BR-03 (Bảo vệ thông tin tài chính nhạy cảm):** Hệ thống CAB tuyệt đối không lưu trữ số thẻ tín dụng (PAN), ngày hết hạn hay mã CVV/CVC trên máy chủ nội bộ. Toàn bộ thông tin thẻ được xử lý qua cơ chế Token hóa (Tokenization) từ Cổng thanh toán được cấp phép.
- **BR-04 (Cách ly lỗi thanh toán điện tử):** Nếu giao dịch qua cổng thanh toán trực tuyến bị lỗi hoặc từ chối, hệ thống không được hủy chuyến đi mà tự động kích hoạt phương thức thanh toán tiền mặt dự phòng và gửi thông báo cho cả tài xế và khách hàng.
- **BR-05 (Điều kiện đóng cuốc xe):** Tài xế chỉ được bấm "Hoàn thành chuyến" khi tọa độ xe nằm trong bán kính cho phép của điểm đến (sai số tối đa 150m) hoặc có sự can thiệp xác nhận đặc biệt từ bộ phận Vận hành.

---

## BƯỚC 5: YÊU CẦU NGHIỆP VỤ (BUSINESS REQUIREMENTS)

- **BRQ-01 (Yêu cầu Đặt xe & Theo dõi):** Hệ thống phải cho phép khách hàng đặt xe theo loại dịch vụ mong muốn, nhận diện được trạng thái tìm kiếm, biết thông tin tài xế tiếp nhận và theo dõi di chuyển thời gian thực.
- **BRQ-02 (Yêu cầu Tự động phân công tài xế):** Hệ thống phải tự động quét, xếp hạng và gán tài xế gần nhất theo thuật toán tuần tự, đảm bảo không bỏ sót yêu cầu của khách hàng khi tài xế từ chối.
- **BRQ-03 (Yêu cầu Quản lý hành trình di chuyển):** Cung cấp cơ chế cập nhật trạng thái hành trình theo thời gian thực cho tài xế với thao tác 1-chạm, đồng bộ ngay lập tức sang ứng dụng của khách hàng.
- **BRQ-04 (Yêu cầu Tính cước & Thanh toán đa kênh):** Tự động tính cước minh bạch dựa trên dữ liệu hành trình thực tế, hỗ trợ linh hoạt cả tiền mặt và ví điện tử/thẻ ngân hàng.
- **BRQ-05 (Yêu cầu Thông báo sự kiện tức thời):** Tự động bắn thông báo đẩy (Push notification) tới đúng đối tượng tại từng mốc sự kiện quan trọng của cuốc xe.
- **BRQ-06 (Yêu cầu Giám sát vận hành & Báo cáo tổng hợp):** Cung cấp bảng điều khiển quản trị trung tâm để theo dõi các chuyến đi đang chạy, quản lý phương tiện/tài xế và trích xuất báo cáo doanh thu, tỷ lệ hoàn thành, tỷ lệ hủy.

---

## BƯỚC 6: MÔ HÌNH HÓA NGHIỆP VỤ (BUSINESS PROCESS MODELING)
*(Hiện thực hóa Yêu cầu nghiệp vụ thành các mô hình trực quan: Sơ đồ Use Case tổng quan, Biểu đồ luồng trạng thái cuốc xe, Sơ đồ hoạt động Activity Diagram và Sơ đồ tuần tự)*

### 6.1 Sơ đồ Tổng quan Use Case Hệ thống (System Use Case Diagram)

```mermaid
graph LR
    subgraph "Các Tác Nhân (Actors)"
        Rider["Khách hàng (Rider)"]
        Driver["Tài xế (Driver)"]
        Ops["Nhân viên Vận hành"]
        Admin["Quản trị viên (Admin)"]
        PGW["Cổng Thanh Toán"]
    end

    subgraph "Hệ Thống CAB System"
        UC1(["UC01: Đăng ký / Xác thực"])
        UC2(["UC02: Tạo yêu cầu Đặt xe"])
        UC3(["UC03: Theo dõi xe thời gian thực"])
        UC4(["UC04: Đánh giá tài xế"])
        UC5(["UC05: Bật/Tắt Online & Gửi vị trí GPS"])
        UC6(["UC06: Tiếp nhận / Từ chối chuyến"])
        UC7(["UC07: Cập nhật tiến độ cuốc xe"])
        UC8(["UC08: Tự động điều phối (Auto-dispatch)"])
        UC9(["UC09: Xử lý thanh toán cước"])
        UC10(["UC10: Giám sát bản đồ xe trực tiếp"])
        UC11(["UC11: Quản lý biểu cước & Xem báo cáo"])
    end

    Rider --> UC1
    Rider --> UC2
    Rider --> UC3
    Rider --> UC4

    Driver --> UC1
    Driver --> UC5
    Driver --> UC6
    Driver --> UC7

    Ops --> UC10
    Admin --> UC11

    UC2 -.-> UC8
    UC7 -.-> UC9
    UC9 -.-> PGW
```

### 6.2 Mô hình Vòng đời Trạng thái Cuốc xe (State Transition Diagram)
Mô tả các chuyển dịch trạng thái bắt buộc của một yêu cầu đặt xe từ lúc khởi tạo đến lúc kết thúc:

```mermaid
stateDiagram-v2
    [*] --> REQUESTED: Khách gửi yêu cầu đặt xe
    REQUESTED --> SEARCHING: Hệ thống quét tài xế gần nhất
    SEARCHING --> DRIVER_ASSIGNED: Tài xế bấm Chấp nhận
    SEARCHING --> NO_DRIVER_FOUND: Hết tài xế khả dụng (Thông báo khách)
    NO_DRIVER_FOUND --> [*]

    DRIVER_ASSIGNED --> ARRIVED_AT_PICKUP: Tài xế đến điểm hẹn
    DRIVER_ASSIGNED --> CANCELLED: Khách / Tài xế hủy chuyến hợp lệ
    CANCELLED --> [*]

    ARRIVED_AT_PICKUP --> IN_TRANSIT: Tài xế xác nhận Đã đón khách
    IN_TRANSIT --> COMPLETED: Tới điểm đến & Bấm hoàn thành
    
    COMPLETED --> PAYMENT_PROCESSING: Hệ thống chốt cước & trừ tiền
    PAYMENT_PROCESSING --> PAID: Thanh toán thành công (Tiền mặt / Thẻ)
    PAID --> [*]: Khách đánh giá & Kết thúc
```

### 6.3 Sơ đồ Hoạt động Nghiệp vụ: Đặt xe & Tự động điều phối (Activity Diagram)

```mermaid
flowchart TD
    Start([Khách hàng nhập lộ trình & ấn Đặt xe]) --> CalcEst[Hệ thống tính cước ước tính & ETA]
    CalcEst --> Confirm{Khách xác nhận đặt?}
    Confirm -- Không --> EndCancel([Hủy yêu cầu])
    
    Confirm -- Có --> Search[Bộ máy tìm kiếm tài xế Online thỏa mãn BR-01 trong bán kính R]
    Search --> CheckAvail{Tìm thấy tài xế?}
    
    CheckAvail -- Không --> AlertNoDriver[Gửi thông báo: Không có tài xế khả dụng] --> EndFail([Kết thúc])
    
    CheckAvail -- Có --> PushDriver[Phát tín hiệu mời cuốc tới Tài xế tối ưu nhất kèm Timer 15s]
    PushDriver --> DriverAction{Tài xế phản hồi?}
    
    DriverAction -- Chấp nhận --> Assign[Khóa cuốc cho tài xế & Thông báo cho khách thông tin xe/tài xế]
    Assign --> Navigation[Chuyển sang chế độ theo dõi đón khách] --> EndSuccess([Bắt đầu hành trình])
    
    DriverAction -- Từ chối hoặc Hết 15s --> Exclude[Áp dụng BR-02: Loại tài xế này ra khỏi lượt tìm hiện tại]
    Exclude --> SearchNext[Quét tài xế phù hợp tiếp theo trong danh sách]
    SearchNext --> CheckAvail
```

### 6.4 Sơ đồ Tuần tự Nghiệp vụ Thanh toán & Cách ly lỗi (Sequence Diagram)

```mermaid
sequenceDiagram
    autonumber
    actor D as Tài xế
    participant APP as CAB Core Engine
    participant PAY as Payment Service
    participant PGW as Cổng Thanh Toán Ngoài
    actor C as Khách hàng

    D->>APP: Xác nhận "Hoàn thành chuyến đi" (BR-05)
    APP->>APP: Tính cước cuối cùng dựa trên lộ trình
    APP->>C: Hiển thị hóa đơn chuyến đi
    
    alt Khách chọn Tiền mặt
        APP->>D: Yêu cầu thu tiền mặt từ khách
        D->>APP: Xác nhận đã nhận tiền mặt
        APP->>APP: Cập nhật trạng thái: ĐÃ THANH TOÁN
    else Khách chọn Thanh toán Điện tử (Thẻ/Ví)
        APP->>PAY: Yêu cầu thanh toán (Charge Token)
        PAY->>PGW: Gửi lệnh trừ tiền qua API bảo mật (BR-03)
        alt Trừ tiền thành công
            PGW-->>PAY: Phản hồi Thành công (Mã TxID)
            PAY-->>APP: Chốt đơn hàng: Hoàn tất
            APP->>C: Thông báo trừ tiền thành công
        else Trừ tiền thất bại (Hết hạn / Lỗi mạng PGW)
            PGW-->>PAY: Phản hồi Thất bại (Error Code)
            PAY-->>APP: Kích hoạt BR-04: Cách ly lỗi
            APP->>C: Thông báo lỗi thẻ, tự động chuyển sang thu Tiền mặt
            APP->>D: Thông báo cuốc đổi sang thu Tiền mặt
            D->>APP: Xác nhận đã thu tiền mặt trực tiếp
        end
    end
    APP->>C: Hiển thị màn hình đánh giá số sao (1-5 sao)
```

---

## BƯỚC 7: YÊU CẦU PHI CHỨC NĂNG (NON-FUNCTIONAL REQUIREMENTS)

- **NFR-01 (Hiệu năng & Thời gian đáp ứng):** Thời gian phát cuốc tới tài xế đầu tiên không quá 3 giây. Thời gian tải bản đồ và tọa độ phản hồi dưới 1 giây.
- **NFR-02 (Tính chịu lỗi & Cách ly sự cố):** Khi Cổng thanh toán hoặc dịch vụ Push Notification bên thứ ba gặp sự cố gián đoạn, dịch vụ điều phối chuyến đi vẫn phải hoạt động độc lập và không bị gián đoạn.
- **NFR-03 (Khả năng mở rộng):** Kiến trúc hệ thống thiết kế dạng Modular để dễ dàng tích hợp thêm các dịch vụ mới (giao hàng, loại xe mới) hoặc đổi nhà cung cấp bản đồ mà không phải viết lại mã nguồn cốt lõi.
- **NFR-04 (Bảo mật & Phân quyền):** Áp dụng mã hóa HTTPS/TLS cho đường truyền, cơ chế phân quyền RBAC cho nhân viên vận hành và ghi log kiểm toán bất biến cho các hành vi thay đổi cước hoặc chỉnh sửa trạng thái cuốc.

---

## BƯỚC 8: DANH MỤC VẤN ĐỀ CẦN LÀM RÕ VỚI KHÁCH HÀNG (OPEN ISSUES FOR BA)

Trong Tuần 1, Business Analyst cần làm việc với Lãnh đạo Công ty ABC để thống nhất các nội dung sau:
1. **Chi tiết cấu trúc cước:** Công thức cước cố định 2km đầu là bao nhiêu? Đơn giá/km và phụ phí đêm/giờ cao điểm được cấu hình động ra sao?
2. **Quy định phạt tài xế:** Tài xế không phản hồi/từ chối bao nhiêu cuốc liên tiếp thì bị hạ điểm uy tín hoặc khóa tài khoản tạm thời?
3. **Chính sách hủy chuyến:** Thời gian hủy miễn phí là bao nhiêu phút sau khi xe nhận? Phí hủy cuốc (nếu có) được tính và trừ vào tài khoản khách bằng cách nào?
4. **Xử lý mất kết nối mạng:** Cơ chế lưu đệm tọa độ ngoại tuyến trên thiết bị tài xế và gửi bù tọa độ khi có lại sóng 4G.
5. **Thời hạn lưu vết dữ liệu:** Quy định lưu trữ tọa độ GPS chi tiết (đề xuất 30 ngày) và dữ liệu kế toán/giao dịch (đề xuất tối thiểu 5 năm).

---

## BƯỚC 9: KẾ HOẠCH BÀN GIAO 7 TUẦN (7-WEEK GANTT ROADMAP)

```mermaid
gantt
    title Kế hoạch Triển khai Nền tảng CAB System (7 Tuần)
    dateFormat  YYYY-MM-DD
    section Tuần 1: Khởi động & BA
    Khảo sát & Làm rõ 5 vấn đề mở       :done,    w1_1, 2026-09-07, 3d
    Chốt SRS & Mô hình hóa nghiệp vụ    :active,  w1_2, 2026-09-10, 4d
    section Tuần 2: Kiến trúc & Thiết kế
    Thiết kế Database & Kiến trúc API   :         w2_1, 2026-09-14, 4d
    Wireframe UI/UX App Khách & Tài xế  :         w2_2, 2026-09-16, 4d
    section Tuần 3-4: Lập trình Core
    Phát triển App Khách & Tài xế       :         w3_1, 2026-09-21, 12d
    Bộ máy Điều phối (Auto-Dispatch)    :         w3_2, 2026-09-23, 10d
    section Tuần 5: Thanh toán & Quản trị
    Tích hợp Cổng thanh toán & Push Notif:        w5_1, 2026-10-05, 5d
    Xây dựng Portal Vận hành & RBAC     :         w5_2, 2026-10-07, 5d
    section Tuần 6: Kiểm thử & Đóng băng
    Kiểm thử Tải giờ cao điểm & Bảo mật :         w6_1, 2026-10-12, 5d
    Nghiệm thu UAT với Vận hành         :         w6_2, 2026-10-14, 4d
    section Tuần 7: Triển khai & Go-Live
    Triển khai hạ tầng Production       :         w7_1, 2026-10-19, 4d
    Đào tạo vận hành & Bàn giao sản phẩm:         w7_2, 2026-10-22, 3d
```
