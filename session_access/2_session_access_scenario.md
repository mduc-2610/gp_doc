# Scenario - Quản lý Session Access

## Use Case: Đăng nhập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Đăng nhập |
| **Actor** | User |
| **Tiền điều kiện** | - User chưa đăng nhập |
| **Hậu điều kiện** | - User đăng nhập thành công và có quyền truy cập hệ thống |
| **Kịch bản chính** | 1. User truy cập trang đăng nhập<br>2. Hệ thống hiển thị form đăng nhập<br>3. User nhập thông tin đăng nhập<br>4. Hệ thống xác thực và chuyển đến trang chính |

---

## Use Case: Xem danh sách Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Đăng nhập" đã hoàn thành |
| **Hậu điều kiện** | - Danh sách Session được hiển thị |
| **Kịch bản chính** | 1. User truy cập trang danh sách Session<br>2. Hệ thống hiển thị danh sách các Session của User |

---

## Use Case: Xem chi tiết Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem chi tiết Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Session" đã hoàn thành |
| **Hậu điều kiện** | - Chi tiết Session được hiển thị |
| **Kịch bản chính** | 1. User chọn một Session từ danh sách<br>2. Hệ thống hiển thị chi tiết Session |

---

## Use Case: Xem danh sách yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Danh sách yêu cầu truy cập được hiển thị |
| **Kịch bản chính** | 1. User click "Chia sẻ" tại Header<br>2. Hệ thống hiển thị dialog chia sẻ<br>3. User click tab "Yêu cầu"<br>4. Hệ thống load và hiển thị danh sách yêu cầu truy cập |

---

## Use Case: Chấp nhận yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chấp nhận yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách yêu cầu truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Yêu cầu được chấp nhận và User nhận quyền truy cập |
| **Kịch bản chính** | 1. User chọn quyền từ dropdown và click "Cấp quyền"<br>2. Hệ thống gửi yêu cầu accept<br>3. Backend cập nhật quyền và gửi thông báo<br>4. User được cấp quyền nhận thông báo realtime |

---

## Use Case: Từ chối yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Từ chối yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách yêu cầu truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Yêu cầu bị từ chối |
| **Kịch bản chính** | 1. User click "Từ chối" request to User<br>2. Hệ thống gửi yêu cầu reject<br>3. Backend xóa yêu cầu và gửi thông báo |

---

## Use Case: Xem các thông tin về quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem các thông tin về quyền truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin quyền truy cập được hiển thị |
| **Kịch bản chính** | 1. User click "Chia sẻ" tại Header<br>2. Hệ thống hiển thị dialog với tab "Chia sẻ"<br>3. Hệ thống load và hiển thị danh sách User đã được chia sẻ |

---

## Use Case: Chia sẻ Session với User cụ thể

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chia sẻ Session với User cụ thể |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem các thông tin về quyền truy cập" đã hoàn thành |
| **Hậu điều kiện** | - User được chia sẻ nhận quyền truy cập |
| **Kịch bản chính** | 1. User click "Add people"<br>2. Hệ thống hiển thị dialog tìm kiếm<br>3. User tìm kiếm, chọn User và quyền, click "Add"<br>4. Hệ thống tạo quyền và gửi thông báo |

---

## Use Case: Cập nhật quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Cập nhật quyền truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem các thông tin về quyền truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Quyền truy cập được cập nhật |
| **Kịch bản chính** | 1. User chọn quyền từ dropdown của User<br>2. Hệ thống gửi yêu cầu cập nhật<br>3. Backend cập nhật quyền và gửi thông báo |

---

## Use Case: Thu hồi quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Thu hồi quyền truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem các thông tin về quyền truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Quyền truy cập bị xóa |
| **Kịch bản chính** | 1. User click "Xóa" từ dropdown của User<br>2. Hệ thống gửi yêu cầu revoke<br>3. Backend xóa quyền và gửi thông báo |

---

## Use Case: Quản lý liên kết chia sẻ

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Quản lý liên kết chia sẻ |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem các thông tin về quyền truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Liên kết chia sẻ được cập nhật |
| **Kịch bản chính** | 1. User chọn "Bật kỳ ai có liên kết" trong dropdown<br>2. Hệ thống cập nhật trạng thái liên kết<br>3. User chọn quyền liên kết nếu cần |

---

## Use Case: Yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành<br>- User chưa có quyền hoặc quyền bị hạn chế |
| **Hậu điều kiện** | - Yêu cầu truy cập được gửi |
| **Kịch bản chính** | 1. User xem chi tiết Session<br>2. Hệ thống hiển thị RequestAccessState với danh sách yêu cầu<br>3. User click "Tạo" hoặc "Yêu cầu" để tạo yêu cầu mới<br>4. Backend tạo yêu cầu và gửi thông báo |

---

## Use Case: Hủy yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Hủy yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Yêu cầu truy cập" đã hoàn thành |
| **Hậu điều kiện** | - Yêu cầu truy cập bị hủy |
| **Kịch bản chính** | 1. User click "Từ chối" request to User<br>2. Hệ thống gửi yêu cầu delete<br>3. Backend xóa yêu cầu |

---

## Use Case: Nhận cập nhật quyền

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Nhận cập nhật quyền |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành<br>- WebSocket connection được thiết lập |
| **Hậu điều kiện** | - User nhận thông báo về thay đổi quyền |
| **Kịch bản chính** | 1. Hệ thống tự động kết nối WebSocket khi vào Session<br>2. Backend gửi event qua WebSocket khi có thay đổi quyền<br>3. Frontend xử lý event và hiển thị thông báo |
