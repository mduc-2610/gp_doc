# Scenario - Quản lý Session

## Use Case: Xem danh sách Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Session |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công |
| **Hậu điều kiện** | - Danh sách các Session của User được hiển thị trên giao diện |
| **Kịch bản chính** | 1. User click "Tài liệu" tại sidebar<br>2. Hệ thống gọi hàm loadSessions()<br>3. Hệ thống gửi yêu cầu GET "/session" đến backend<br>4. Backend trả về danh sách Session<br>5. Hệ thống hiển thị danh sách Session cho User |
| **Ngoại lệ** | - User không có Session nào: hiển thị thông báo "Không tìm thấy phiên nào"<br>- Lỗi load Session: hiển thị thông báo "Không thể tải phiên" |

---

## Use Case: Tạo Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Session" đã hoàn thành |
| **Hậu điều kiện** | - Session mới được tạo và thêm vào danh sách<br>- Hệ thống hiển thị thông báo tạo thành công |
| **Kịch bản chính** | 1. User click "Tạo phiên mới"<br>2. Hệ thống hiển thị dialog SessionCreateDialog<br>3. User nhập thông tin tên Session, mô tả Session và click "Tạo phiên"<br>4. Hệ thống validate dữ liệu nhập vào<br>5. Hệ thống gọi hàm handleCreateSession()<br>6. Hệ thống gửi yêu cầu POST "/session" đến backend<br>7. Backend tạo Session mới và trả về thông tin Session<br>8. Hệ thống cập nhật danh sách Session và hiển thị thông báo thành công |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo Session: hiển thị thông báo "Không thể tạo phiên" |

---

## Use Case: Xóa Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Session" đã hoàn thành |
| **Hậu điều kiện** | - Session được xóa khỏi hệ thống<br>- Danh sách Session được cập nhật |
| **Kịch bản chính** | 1. User click icon delete trên Session<br>2. Hệ thống hiển thị dialog SessionDeleteDialog xác nhận<br>3. User click "Xác nhận"<br>4. Hệ thống gọi hàm handleDeleteSession()<br>5. Hệ thống gửi yêu cầu DELETE "/session/{session_id}" đến backend<br>6. Backend xóa Session và trả về thông báo thành công<br>7. Hệ thống loại bỏ Session khỏi danh sách và hiển thị thông báo |
| **Ngoại lệ** | - Session không tồn tại: hiển thị thông báo "Không tìm thấy phiên"<br>- Lỗi xóa: hiển thị thông báo "Không thể xóa phiên" |

---

## Use Case: Xem chi tiết Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem chi tiết Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Session" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin chi tiết Session được hiển thị trên giao diện |
| **Kịch bản chính** | 1. User click vào 1 Session trong danh sách<br>2. Hệ thống gọi hàm loadSession()<br>3. Hệ thống gửi yêu cầu GET "/session/{session_id}" đến backend<br>4. Backend trả về thông tin chi tiết Session<br>5. Hệ thống hiển thị chi tiết Session cho User |
| **Ngoại lệ** | - Session không tồn tại: hiển thị thông báo "Không tìm thấy phiên"<br>- Lỗi load Session: hiển thị thông báo "Không thể tải phiên" |

---

## Use Case: Chỉnh sửa Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin Session được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. User click icon update tại header<br>2. Hệ thống hiển thị dialog SessionUpdateDialog với thông tin hiện tại<br>3. User nhập thông tin tên và mô tả mới, click "Lưu phiên"<br>4. Hệ thống validate dữ liệu nhập vào<br>5. Hệ thống gọi hàm handleUpdateSession()<br>6. Hệ thống gửi yêu cầu PUT "/session/{session_id}" đến backend<br>7. Backend cập nhật Session và trả về thông tin Session mới<br>8. Hệ thống cập nhật giao diện và hiển thị thông báo thành công |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Session không tồn tại: hiển thị thông báo "Không tìm thấy phiên"<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật phiên" |
