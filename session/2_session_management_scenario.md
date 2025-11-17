# Scenario - Quản lý các Session

## Use Case: Xem danh sách Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Session |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công |
| **Hậu điều kiện** | - Danh sách các Session của User được hiển thị trên giao diện |
| **Kịch bản chính** | 1. Tại giao diện chính, User click vào nút "Tài liệu" tại sidebar <br>2. Hệ thống load và hiển thị danh sách tất cả các Session của User |
| **Ngoại lệ** | - User không có Session nào: hiển thị thông báo "Không tìm thấy phiên nào"<br>- Lỗi load Session: hiển thị thông báo "Không thể tải phiên" |

---

## Use Case: Tạo Session mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo Session mới |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công |
| **Hậu điều kiện** | - Session mới được tạo trong hệ thống<br>- User được chuyển hướng đến giao diện chi tiết Session |
| **Kịch bản chính** | 1. User click vào nút "Tài liệu" tại sidebar<br>2. Hệ thống gửi yêu cầu lấy danh sách Session đến backend<br>3. Backend trả về danh sách Session<br>4. Hệ thống hiển thị danh sách Session<br>5. User click nút "Tạo phiên mới"<br>6. Hệ thống mở dialog tạo Session<br>7. User nhập các thông tin và click nút "Tạo phiên":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên (tùy chọn)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Mô tả (tùy chọn)<br>8. Hệ thống gửi yêu cầu tạo Session đến backend<br>9. Backend tạo Session và trả về thông tin Session mới<br>10. Hệ thống đóng dialog và chuyển hướng User đến chi tiết Session |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo Session: hiển thị thông báo "Không thể tạo phiên" |

---

## Use Case: Chỉnh sửa Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa Session |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công |
| **Hậu điều kiện** | - Thông tin Session được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. User click vào nút "Tài liệu" tại sidebar<br>2. Hệ thống gửi yêu cầu lấy danh sách Session đến backend<br>3. Backend trả về danh sách Session<br>4. Hệ thống hiển thị danh sách Session<br>5. User click icon chỉnh sửa trên Session<br>6. Hệ thống mở dialog chỉnh sửa với thông tin hiện tại<br>7. User cập nhật các thông tin và click nút "Cập nhật":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên<br>&nbsp;&nbsp;&nbsp;&nbsp;- Mô tả<br>8. Hệ thống gửi yêu cầu cập nhật đến backend<br>9. Backend cập nhật Session trong cơ sở dữ liệu và trả về Session mới<br>10. Hệ thống cập nhật item trong danh sách, đóng dialog và hiển thị thông báo "Cập nhật phiên thành công" |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Session không tồn tại: hiển thị thông báo "Không tìm thấy phiên"<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật phiên" |

---

## Use Case: Xóa Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Session |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công |
| **Hậu điều kiện** | - Session được xóa khỏi hệ thống<br>- Danh sách Session được cập nhật |
| **Kịch bản chính** | 1. User click vào nút "Tài liệu" tại sidebar<br>2. Hệ thống gửi yêu cầu lấy danh sách Session đến backend<br>3. Backend trả về danh sách Session<br>4. Hệ thống hiển thị danh sách Session<br>5. User click icon xóa trên Session<br>6. Hệ thống yêu cầu xác nhận xóa<br>7. User xác nhận muốn xóa<br>8. Hệ thống gửi yêu cầu xóa đến backend<br>9. Backend xóa Session khỏi cơ sở dữ liệu và trả về trạng thái thành công<br>10. Hệ thống loại bỏ item khỏi danh sách<br>11. Hiển thị thông báo "Đã xóa phiên thành công" |
| **Ngoại lệ** | - Session không tồn tại: hiển thị thông báo "Không tìm thấy phiên"<br>- Lỗi xóa: hiển thị thông báo "Không thể xóa phiên" |
