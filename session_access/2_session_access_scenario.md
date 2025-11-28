# Scenario - Quản lý Session Access

## Use Case: Xem danh sách yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - User đang xem chi tiết Session |
| **Hậu điều kiện** | - Danh sách yêu cầu truy cập được hiển thị |
| **Kịch bản chính** | 1. User click "Chia sẻ" tại Header<br>2. Hệ thống hiển thị dialog chia sẻ<br>3. User click tab "Yêu cầu"<br>4. Hệ thống load và hiển thị danh sách yêu cầu truy cập |
| **Ngoại lệ** | - Không có yêu cầu: hiển thị thông báo "Chưa có yêu cầu truy cập nào"<br>- Lỗi load: hiển thị thông báo "Không thể tải danh sách yêu cầu"<br>- User không có quyền Manager: không hiển thị tab "Yêu cầu" |

---

## Use Case: Chấp nhận yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chấp nhận yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách yêu cầu truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Yêu cầu được chấp nhận và User nhận quyền truy cập |
| **Kịch bản chính** | 1. User chọn quyền từ dropdown và click "Cấp quyền"<br>2. Hệ thống gửi yêu cầu accept<br>3. Backend cập nhật quyền và gửi thông báo<br>4. User được cấp quyền nhận thông báo realtime |
| **Ngoại lệ** | - Yêu cầu không tồn tại: hiển thị thông báo "Không tìm thấy yêu cầu truy cập"<br>- User không có quyền Manager: hiển thị thông báo "Bạn không có quyền chấp nhận yêu cầu"<br>- Lỗi cập nhật: hiển thị thông báo "Không thể chấp nhận yêu cầu truy cập" |

---

## Use Case: Từ chối yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Từ chối yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách yêu cầu truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Yêu cầu bị từ chối |
| **Kịch bản chính** | 1. User click "Từ chối" request to User<br>2. Hệ thống gửi yêu cầu reject<br>3. Backend xóa yêu cầu và gửi thông báo |
| **Ngoại lệ** | - Yêu cầu không tồn tại: hiển thị thông báo "Không tìm thấy yêu cầu truy cập"<br>- User không có quyền Manager: hiển thị thông báo "Bạn không có quyền từ chối yêu cầu"<br>- Lỗi xóa: hiển thị thông báo "Không thể từ chối yêu cầu truy cập" |

---

## Use Case: Xem các thông tin về quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem các thông tin về quyền truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin quyền truy cập được hiển thị |
| **Kịch bản chính** | 1. User click "Chia sẻ" tại Header<br>2. Hệ thống hiển thị dialog với tab "Chia sẻ"<br>3. Hệ thống load và hiển thị danh sách User đã được chia sẻ |
| **Ngoại lệ** | - Không có User được chia sẻ: hiển thị thông báo "Chưa chia sẻ với ai"<br>- Lỗi load: hiển thị thông báo "Không thể tải thông tin chia sẻ"<br>- User không có quyền xem: không hiển thị dialog chia sẻ |

---

## Use Case: Chia sẻ Session với User

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chia sẻ Session với User |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem các thông tin về quyền truy cập" đã hoàn thành |
| **Hậu điều kiện** | - User được chia sẻ nhận quyền truy cập |
| **Kịch bản chính** | 1. User click "Add people"<br>2. Hệ thống hiển thị dialog tìm kiếm<br>3. User tìm kiếm, chọn User và quyền, click "Add"<br>4. Hệ thống tạo quyền và gửi thông báo |
| **Ngoại lệ** | - User không tồn tại: hiển thị thông báo "Không tìm thấy người dùng"<br>- User đã được chia sẻ: hiển thị thông báo "Người dùng đã có quyền truy cập"<br>- Vượt quá giới hạn chia sẻ (10 người): hiển thị thông báo "Không thể chia sẻ với hơn 10 người"<br>- User không có quyền Manager: hiển thị thông báo "Bạn không có quyền chia sẻ Session"<br>- Lỗi tạo quyền: hiển thị thông báo "Không thể chia sẻ Session" |

---

## Use Case: Cập nhật quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Cập nhật quyền truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem các thông tin về quyền truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Quyền truy cập được cập nhật |
| **Kịch bản chính** | 1. User chọn quyền từ dropdown của User<br>2. Hệ thống gửi yêu cầu cập nhật<br>3. Backend cập nhật quyền và gửi thông báo |
| **Ngoại lệ** | - Quyền không tồn tại: hiển thị thông báo "Không tìm thấy quyền truy cập"<br>- Cập nhật làm mất Manager cuối cùng: hiển thị thông báo "Phải có ít nhất một Manager"<br>- User không có quyền Manager: hiển thị thông báo "Bạn không có quyền cập nhật quyền truy cập"<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật quyền truy cập" |

---

## Use Case: Thu hồi quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Thu hồi quyền truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem các thông tin về quyền truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Quyền truy cập bị xóa |
| **Kịch bản chính** | 1. User click "Xóa" từ dropdown của User<br>2. Hệ thống gửi yêu cầu revoke<br>3. Backend xóa quyền và gửi thông báo |
| **Ngoại lệ** | - Quyền không tồn tại: hiển thị thông báo "Không tìm thấy quyền truy cập"<br>- Xóa Manager cuối cùng: hiển thị thông báo "Không thể xóa Manager cuối cùng"<br>- User không có quyền Manager: hiển thị thông báo "Bạn không có quyền thu hồi quyền truy cập"<br>- Lỗi xóa: hiển thị thông báo "Không thể thu hồi quyền truy cập" |

---

## Use Case: Quản lý liên kết chia sẻ

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Quản lý liên kết chia sẻ |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem các thông tin về quyền truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Liên kết chia sẻ được cập nhật |
| **Kịch bản chính** | 1. User chọn "Bật kỳ ai có liên kết" trong dropdown<br>2. Hệ thống cập nhật trạng thái liên kết<br>3. User chọn quyền liên kết trong dropdown<br>4. Hệ thống cập nhật trạng thái về quyền của liên kết  |
| **Ngoại lệ** | - Liên kết không tồn tại: hiển thị thông báo "Không tìm thấy liên kết chia sẻ"<br>- User không có quyền Manager: hiển thị thông báo "Bạn không có quyền quản lý liên kết chia sẻ"<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật liên kết chia sẻ" |

---

## Use Case: Yêu cầu truy cập Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Yêu cầu truy cập Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành<br>- User chưa có quyền hoặc quyền bị hạn chế |
| **Hậu điều kiện** | - Yêu cầu truy cập được gửi |
| **Kịch bản chính** | 1. User xem chi tiết Session<br>2. Hệ thống hiển thị RequestAccessState với danh sách yêu cầu<br>3. User click "Tạo" hoặc "Yêu cầu" để tạo yêu cầu mới<br>4. Backend tạo yêu cầu và gửi thông báo |
| **Ngoại lệ** | - Session không tồn tại: hiển thị thông báo "Không tìm thấy Session"<br>- User đã có yêu cầu chờ xử lý: hiển thị thông báo "Yêu cầu của bạn đang chờ phê duyệt"<br>- User đã có quyền truy cập: không hiển thị RequestAccessState<br>- Lỗi tạo yêu cầu: hiển thị thông báo "Không thể gửi yêu cầu truy cập" |

---

## Use Case: Hủy yêu cầu truy cập Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Hủy yêu cầu truy cập Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Yêu cầu truy cập Session" đã hoàn thành |
| **Hậu điều kiện** | - Yêu cầu truy cập bị hủy |
| **Kịch bản chính** | 1. User click "Hủy" yêu cầu truy cập của mình<br>2. Hệ thống gửi yêu cầu delete<br>3. Backend xóa yêu cầu |
| **Ngoại lệ** | - Yêu cầu không tồn tại: hiển thị thông báo "Không tìm thấy yêu cầu truy cập"<br>- Yêu cầu đã được xử lý: hiển thị thông báo "Yêu cầu đã được xử lý"<br>- Lỗi xóa: hiển thị thông báo "Không thể hủy yêu cầu truy cập" |


