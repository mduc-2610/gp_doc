# Scenario - Quản lý các Session

## Use Case: Xem danh sách Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Session |
| **Actor** | User |
| **Tiền điều kiện** | User đã đăng nhập thành công |
| **Hậu điều kiện** | Danh sách các Session của User được hiển thị trên giao diện |
| **Kịch bản chính** | 1. Tại giao diện chính, User click vào nút "Tài liệu" tại sidebar <br>2. Hệ thống load và hiển thị danh sách tất cả các Session của User |
| **Ngoại lệ** | - User không có Session nào: hiển thị thông báo "Không có Session nào"<br>- Lỗi load Session: hiển thị thông báo lỗi từ server |

---

## Use Case: Tạo Session mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo Session mới |
| **Actor** | User |
| **Tiền điều kiện** | User đã đăng nhập thành công và đang ở giao diện danh sách Session |
| **Hậu điều kiện** | Session mới được tạo và User được chuyển hướng đến chi tiết Session |
| **Kịch bản chính** | 1. Tại giao diện danh sách Session, User click nút "Tạo Session mới"<br>2. Hệ thống mở dialog tạo Session<br>3. User nhập các thông tin và click nút "Tạo":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên (tùy chọn)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Mô tả (tùy chọn)<br>4. Hệ thống gửi yêu cầu tạo Session đến backend<br>5. Backend tạo Session và trả về thông tin Session mới<br>6. Hệ thống đóng dialog và chuyển hướng User đến chi tiết Session |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo Session: hiển thị thông báo lỗi từ server |

---

## Use Case: Chỉnh sửa Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa Session |
| **Actor** | User |
| **Tiền điều kiện** | User đã đăng nhập thành công và đang ở giao diện danh sách Session |
| **Hậu điều kiện** | Thông tin Session được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện danh sách Session, User click icon chỉnh sửa trên Session<br>2. Hệ thống mở dialog chỉnh sửa với thông tin hiện tại<br>3. User cập nhật các thông tin và click nút "Lưu":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên<br>&nbsp;&nbsp;&nbsp;&nbsp;- Mô tả<br>4. Hệ thống gửi yêu cầu cập nhật đến backend<br>5. Backend cập nhật Session trong cơ sở dữ liệu<br>6. Hệ thống đóng dialog và hiển thị thông báo thành công |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Session không tồn tại: hiển thị thông báo lỗi<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server |

---

## Use Case: Xóa Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Session |
| **Actor** | User |
| **Tiền điều kiện** | User đã đăng nhập thành công và đang ở giao diện danh sách Session |
| **Hậu điều kiện** | Session được xóa khỏi hệ thống và danh sách được cập nhật |
| **Kịch bản chính** | 1. Tại giao diện danh sách Session, User click icon xóa trên Session<br>2. Hệ thống yêu cầu xác nhận xóa<br>3. User xác nhận muốn xóa<br>4. Hệ thống gửi yêu cầu xóa đến backend<br>5. Backend xóa Session khỏi cơ sở dữ liệu<br>6. Hệ thống loại bỏ item khỏi danh sách<br>7. Hiển thị thông báo "Xóa Session thành công" |
| **Ngoại lệ** | - Session không tồn tại: hiển thị thông báo lỗi<br>- Lỗi xóa: hiển thị thông báo lỗi từ server |
