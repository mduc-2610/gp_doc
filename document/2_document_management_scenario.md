# Scenario - Quản lý Document

## Use Case: Xem danh sách Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Document |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Danh sách Document được hiển thị trên giao diện |
| **Kịch bản chính** | 1. Hệ thống tự động gửi yêu cầu lấy danh sách Document đến backend<br>2. Backend trả về danh sách Document<br>3. Hệ thống hiển thị danh sách Document trong sidebar |
| **Ngoại lệ** | - Không có Document: hiển thị thông báo "Chưa có tài liệu nào"<br>- Lỗi load: hiển thị thông báo "Không thể tải nội dung tài liệu" |

---

## Use Case: Upload Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Upload Document |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Document mới được upload và xử lý<br>- Danh sách Document được cập nhật |
| **Kịch bản chính** | 1. User click icon plus để thêm Document<br>2. Hệ thống mở dialog upload với 2 tab: "Tệp" và "Liên kết"<br>3. User chọn file hoặc nhập URL và click "Tải lên"<br>4. Hệ thống gửi yêu cầu upload đến backend<br>5. Backend xử lý upload, parse document, tạo embedding và lưu vào database<br>6. Backend trả về thông tin Document đã xử lý<br>7. Hệ thống cập nhật danh sách Document |
| **Ngoại lệ** | - File vượt quá kích thước: hiển thị lỗi validation<br>- Định dạng không hỗ trợ: hiển thị thông báo "Loại tệp này không được hỗ trợ"<br>- URL không hợp lệ: hiển thị lỗi validation<br>- Lỗi xử lý: hiển thị thông báo "Xử lý tài liệu thất bại" |

---

## Use Case: Cập nhật Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Cập nhật Document |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Document" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin Document được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. User click icon cập nhật trên Document<br>2. Hệ thống mở dialog cập nhật với thông tin hiện tại<br>3. User nhập tên mới và click "Đổi tên"<br>4. Hệ thống gửi yêu cầu cập nhật đến backend<br>5. Backend cập nhật Document và trả về Document mới<br>6. Hệ thống cập nhật item trong danh sách và hiển thị thông báo thành công |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Document không tồn tại: hiển thị thông báo "Không tìm thấy tài liệu"<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật tài liệu" |

---

## Use Case: Xóa Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Document |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Document" đã hoàn thành |
| **Hậu điều kiện** | - Document được xóa khỏi hệ thống<br>- Danh sách Document được cập nhật |
| **Kịch bản chính** | 1. User click icon xóa trên Document<br>2. Hệ thống mở dialog xác nhận xóa<br>3. User nhập tên Document và click "Xóa"<br>4. Hệ thống gửi yêu cầu xóa đến backend<br>5. Backend xóa Document và trả về trạng thái thành công<br>6. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo thành công |
| **Ngoại lệ** | - Document không tồn tại: hiển thị thông báo "Không tìm thấy tài liệu"<br>- Lỗi xóa: hiển thị thông báo "Không thể xóa tài liệu" |
