# DANH SÁCH TEST CASE - CAB SYSTEM

Tài liệu này đặc tả các kịch bản kiểm thử (Test Scenarios) cho nền tảng CAB System. Mỗi Scenario bao phủ 5 khía cạnh: **Positive, Negative, Boundary, Null, và Invalid Format**.

## Gửi mã OTP xác thực (Send OTP)

| Test Case ID   | Test Case                                   | Preconditions                       | Test Steps                                         | Test Data                                               | Expected Result                           | Priority   |
|:---------------|:--------------------------------------------|:------------------------------------|:---------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------|
| TC-CAB-001     | [Positive] Gửi OTP với SDT VN hợp lệ        | Chưa đăng nhập, ở màn hình nhập SĐT | 1. Nhập SDT hợp lệ<br>2. Nhấn Gửi OTP              | {"phone_number": "0987654321", "purpose": "REGISTER"}   | Mã OTP được gửi thành công, HTTP 200      | High       |
| TC-CAB-002     | [Positive] Gửi OTP cho mục đích LOGIN       | Chưa đăng nhập, có tài khoản        | 1. Nhập SDT<br>2. Chọn purpose LOGIN<br>3. Gửi OTP | {"phone_number": "0987654321", "purpose": "LOGIN"}      | OTP gửi thành công, trả về otp_session_id | High       |
| TC-CAB-003     | [Negative] Mục đích gửi OTP không tồn tại   | Ở màn hình nhập SĐT                 | 1. Nhập SDT<br>2. Cố tình gửi purpose sai qua API  | {"phone_number": "0987654321", "purpose": "UPDATE"}     | Báo lỗi HTTP 400 do sai Enum              | Medium     |
| TC-CAB-004     | [Boundary] SDT có độ dài biên dưới (9 số)   | Ở màn hình nhập SĐT                 | 1. Nhập SĐT 9 số<br>2. Gửi OTP                     | {"phone_number": "098765432", "purpose": "REGISTER"}    | Báo lỗi validation SĐT quá ngắn           | High       |
| TC-CAB-005     | [Boundary] SDT có độ dài biên chuẩn (10 số) | Ở màn hình nhập SĐT                 | 1. Nhập SĐT 10 số<br>2. Gửi OTP                    | {"phone_number": "0987654321", "purpose": "REGISTER"}   | Thành công                                | High       |
| TC-CAB-006     | [Boundary] SDT có độ dài biên trên (11 số)  | Ở màn hình nhập SĐT                 | 1. Nhập SĐT 11 số<br>2. Gửi OTP                    | {"phone_number": "09876543210", "purpose": "REGISTER"}  | Báo lỗi validation SĐT quá dài            | High       |
| TC-CAB-007     | [Null] Bỏ trống trường SĐT                  | Ở màn hình nhập SĐT                 | 1. Không nhập SĐT<br>2. Gửi OTP                    | {"phone_number": "", "purpose": "REGISTER"}             | Báo lỗi HTTP 400 thiếu trường bắt buộc    | High       |
| TC-CAB-008     | [Null] Bỏ trống trường purpose              | Ở API gửi OTP                       | Gửi API thiếu field purpose                        | {"phone_number": "0987654321"}                          | Báo lỗi HTTP 400 thiếu trường bắt buộc    | High       |
| TC-CAB-009     | [Invalid Format] SĐT chứa ký tự chữ cái     | Màn hình nhập SĐT                   | Nhập SĐT chứa chữ cái                              | {"phone_number": "0987abcdef", "purpose": "REGISTER"}   | Báo lỗi định dạng SĐT không hợp lệ        | High       |
| TC-CAB-010     | [Invalid Format] SĐT chứa ký tự đặc biệt    | Màn hình nhập SĐT                   | Nhập SĐT có dấu gạch ngang                         | {"phone_number": "0987-654-321", "purpose": "REGISTER"} | Báo lỗi định dạng SĐT không hợp lệ        | High       |

---

## Dự toán cước phí & Tuyến đường

| Test Case ID   | Test Case                                           | Preconditions          | Test Steps                          | Test Data                                 | Expected Result                                     | Priority   |
|:---------------|:----------------------------------------------------|:-----------------------|:------------------------------------|:------------------------------------------|:----------------------------------------------------|:-----------|
| TC-CAB-011     | [Positive] Dự toán cước với tọa độ hợp lệ           | Đã đăng nhập App Khách | Nhập Điểm đón và Điểm đến hợp lệ    | pickup_lat: 10.7725, dropoff_lat: 10.8185 | Trả về danh sách CAB_4, CAB_7 và giá tiền           | High       |
| TC-CAB-012     | [Negative] Điểm đón và điểm đến trùng nhau          | Đã đăng nhập           | Nhập điểm đón và đến giống hệt nhau | pickup trùng dropoff (distance = 0)       | Báo lỗi khoảng cách quá ngắn                        | High       |
| TC-CAB-013     | [Negative] Khoảng cách ngoài khu vực phục vụ        | Đã đăng nhập           | Nhập điểm đón ở VN, điểm đến ở Mỹ   | Khoảng cách 15000km                       | Báo lỗi khu vực không hỗ trợ                        | Medium     |
| TC-CAB-014     | [Boundary] Vĩ độ đúng biên giới hạn (90 độ)         | Gọi API dự toán        | Truyền vĩ độ lớn nhất có thể        | pickup_lat: 90.0                          | Hợp lệ về tọa độ (có thể lỗi business nếu ngoài VN) | Medium     |
| TC-CAB-015     | [Boundary] Vĩ độ vượt biên (90.000001)              | Gọi API dự toán        | Truyền vĩ độ vượt ngưỡng            | pickup_lat: 90.000001                     | Lỗi validation tọa độ (HTTP 400)                    | High       |
| TC-CAB-016     | [Boundary] Khoảng cách đúng bằng 2km (Giá mở cửa)   | Đã đăng nhập           | Chọn 2 điểm cách nhau đúng 2km      | Khoảng cách = 2.0km                       | Tính cước theo mức giá mở cửa                       | High       |
| TC-CAB-017     | [Null] Bỏ trống vĩ độ điểm đón                      | Gọi API dự toán        | Bỏ field pickup_lat                 | Thiếu trường pickup_lat                   | Báo lỗi thiếu required field                        | High       |
| TC-CAB-018     | [Null] Bỏ trống kinh độ điểm trả                    | Gọi API dự toán        | Bỏ field dropoff_long               | Thiếu trường dropoff_long                 | Báo lỗi thiếu required field                        | High       |
| TC-CAB-019     | [Invalid Format] Tọa độ truyền dạng chữ (String)    | Gọi API dự toán        | Truyền tọa độ dưới dạng text        | pickup_lat: "mười phẩy năm"               | Lỗi Type Mismatch (HTTP 400)                        | High       |
| TC-CAB-020     | [Invalid Format] Định dạng JSON bị hỏng (Malformed) | Gọi API                | Gửi body JSON thiếu dấu ngoặc       | {pickup_lat: 10,}                         | Lỗi Parse JSON (HTTP 400)                           | High       |

---

## Khởi tạo yêu cầu Đặt xe (Create Booking)

| Test Case ID   | Test Case                                           | Preconditions   | Test Steps                           | Test Data                                               | Expected Result                            | Priority   |
|:---------------|:----------------------------------------------------|:----------------|:-------------------------------------|:--------------------------------------------------------|:-------------------------------------------|:-----------|
| TC-CAB-021     | [Positive] Đặt CAB_4 thanh toán Tiền mặt            | Đã dự toán cước | Chọn CAB_4, Tiền mặt -> Đặt xe       | {"vehicle_type": "CAB_4", "payment_method": "CASH"}     | Tạo thành công, cuốc chuyển sang SEARCHING | High       |
| TC-CAB-022     | [Positive] Đặt CAB_7 thanh toán Thẻ (PGW)           | Đã dự toán cước | Chọn CAB_7, Thẻ -> Đặt xe            | {"vehicle_type": "CAB_7", "payment_method": "PGW_CARD"} | Tạo thành công, cuốc chuyển sang SEARCHING | High       |
| TC-CAB-023     | [Negative] Đặt loại xe không hỗ trợ                 | Gọi API đặt xe  | Truyền loại xe sai                   | {"vehicle_type": "CAB_16"}                              | Báo lỗi Enum vehicle_type không hợp lệ     | High       |
| TC-CAB-024     | [Negative] Chọn PT thanh toán không tồn tại         | Gọi API đặt xe  | Truyền PTTT không có trong hệ thống  | {"payment_method": "CRYPTO"}                            | Báo lỗi Enum payment_method không hợp lệ   | High       |
| TC-CAB-025     | [Boundary] Tọa độ thập phân siêu dài                | Đã đăng nhập    | Truyền tọa độ với 15 số sau dấu phẩy | pickup_lat: 10.123456789012345                          | Hệ thống làm tròn và tạo cuốc bình thường  | Low        |
| TC-CAB-026     | [Null] Không truyền loại xe (vehicle_type)          | Gọi API đặt xe  | Bỏ field vehicle_type                | Không có "vehicle_type"                                 | Lỗi HTTP 400 thiếu trường bắt buộc         | High       |
| TC-CAB-027     | [Null] Không truyền phương thức thanh toán          | Gọi API đặt xe  | Bỏ field payment_method              | Không có "payment_method"                               | Lỗi HTTP 400 thiếu trường bắt buộc         | High       |
| TC-CAB-028     | [Invalid Format] payment_method truyền kiểu Boolean | Gọi API         | Truyền payment_method: true          | {"payment_method": true}                                | Lỗi sai kiểu dữ liệu (Expected String)     | High       |
| TC-CAB-029     | [Invalid Format] pickup_lat truyền dạng Mảng        | Gọi API         | Truyền lat dạng array                | {"pickup_lat": [10.1, 10.2]}                            | Lỗi sai kiểu dữ liệu (Expected Float)      | High       |

---

## Tài xế Cập nhật vị trí GPS

| Test Case ID   | Test Case                                             | Preconditions            | Test Steps                          | Test Data                                   | Expected Result                              | Priority   |
|:---------------|:------------------------------------------------------|:-------------------------|:------------------------------------|:--------------------------------------------|:---------------------------------------------|:-----------|
| TC-CAB-030     | [Positive] Gửi tọa độ khi đang Online & di chuyển     | Tài xế đang Online       | Ứng dụng gửi tọa độ ngầm lên server | latitude: 10.7, longitude: 106.6, speed: 40 | Server trả HTTP 200, ghi nhận GPS            | High       |
| TC-CAB-031     | [Negative] Gửi tọa độ khi đang Offline                | Tài xế đang Offline      | Gửi tọa độ lên máy chủ              | Tài xế status = OFFLINE                     | Server từ chối ghi nhận hoặc trả lỗi         | Medium     |
| TC-CAB-032     | [Boundary] Gửi GPS với Vận tốc = 0 (Đứng im)          | Tài xế đang Online       | Gửi GPS khi xe dừng đèn đỏ          | speed: 0.0                                  | Ghi nhận bình thường                         | Medium     |
| TC-CAB-033     | [Boundary] Gửi GPS với Vận tốc âm                     | Tài xế đang Online       | Gửi vận tốc < 0                     | speed: -1.5                                 | Hệ thống báo lỗi logic validation            | Low        |
| TC-CAB-034     | [Boundary] Tọa độ vượt mốc kinh độ (+181)             | Tài xế gửi GPS           | Kinh độ truyền 181                  | longitude: 181.0                            | Lỗi validation (kinh độ chỉ từ -180 đến 180) | Medium     |
| TC-CAB-035     | [Null] Bỏ trống trường Vĩ độ (Latitude)               | Gọi API /driver/location | Không gửi latitude                  | Thiếu "latitude"                            | Báo lỗi HTTP 400 required field              | High       |
| TC-CAB-036     | [Null] Bỏ trống trường Kinh độ (Longitude)            | Gọi API /driver/location | Không gửi longitude                 | Thiếu "longitude"                           | Báo lỗi HTTP 400 required field              | High       |
| TC-CAB-037     | [Invalid Format] Thời gian sai chuẩn ISO-8601         | Gửi định vị              | Truyền time format sai              | recorded_at: "2026/09/13 15:00"             | Lỗi parse thời gian (HTTP 400)               | High       |
| TC-CAB-038     | [Invalid Format] Tốc độ truyền kiểu String kèm đơn vị | Gửi định vị              | Truyền speed là chuỗi               | speed: "40 km/h"                            | Lỗi Type Mismatch (Expected Float)           | High       |

---

## Hoàn thành chuyến & Xử lý trừ tiền

| Test Case ID   | Test Case                                                 | Preconditions        | Test Steps                             | Test Data                       | Expected Result                                             | Priority   |
|:---------------|:----------------------------------------------------------|:---------------------|:---------------------------------------|:--------------------------------|:------------------------------------------------------------|:-----------|
| TC-CAB-039     | [Positive] Hoàn thành với PTTT Thẻ PGW thành công         | Cuốc đang IN_TRANSIT | Tài xế bấm hoàn thành chuyến           | PTTT: PGW_CARD, toll_fee: 0     | HTTP 200, trạng thái PAID, trừ tiền thành công              | High       |
| TC-CAB-040     | [Negative] Hoàn thành khi cuốc chưa được nhận (SEARCHING) | Cuốc đang SEARCHING  | Gọi API complete                       | booking_id của cuốc SEARCHING   | Lỗi logic: Cuốc chưa chạy không thể hoàn thành              | High       |
| TC-CAB-041     | [Negative] Hoàn thành với PGW thất bại (Cách ly lỗi)      | Cuốc IN_TRANSIT      | Gọi API complete, giả lập thẻ hết tiền | Thẻ hết hạn/hết tiền            | BR-04: Tự động đổi sang thu Tiền mặt, HTTP 200 kèm cảnh báo | Critical   |
| TC-CAB-042     | [Boundary] Không có phí cầu đường (toll_fee = 0)          | Cuốc IN_TRANSIT      | Gửi toll_fee = 0                       | toll_fee: 0                     | Hoàn thành và tính đúng cước gốc                            | High       |
| TC-CAB-043     | [Boundary] Phí cầu đường cực lớn (vượt ngưỡng)            | Cuốc IN_TRANSIT      | Gửi toll_fee vượt giới hạn             | toll_fee: 999999999             | Lỗi số tiền vượt ngưỡng cho phép                            | Medium     |
| TC-CAB-044     | [Null] Bỏ trống tọa độ trả khách thực tế                  | Gọi API complete     | Không truyền dropoff_lat_actual        | Thiếu trường dropoff_lat_actual | Lỗi HTTP 400                                                | High       |
| TC-CAB-045     | [Null] Bỏ trống cước phụ phí cầu đường                    | Gọi API complete     | Không truyền toll_fee                  | Thiếu trường toll_fee           | Lỗi HTTP 400 (hoặc hệ thống auto set = 0)                   | High       |
| TC-CAB-046     | [Invalid Format] toll_fee truyền kiểu Float (số thực)     | Gọi API complete     | Truyền phí dạng float                  | toll_fee: 10000.5               | Lỗi định dạng (Expected Integer) hoặc làm tròn              | Medium     |
| TC-CAB-047     | [Invalid Format] toll_fee truyền kèm ký tự chữ            | Gọi API complete     | Truyền chữ vào giá tiền                | toll_fee: "10k"                 | Lỗi Type Mismatch (HTTP 400)                                | High       |

---

