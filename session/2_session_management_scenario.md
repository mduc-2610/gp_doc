# Scenario - Quản lý Document

## Use Case: Xem danh sách Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Session |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công |
| **Hậu điều kiện** | - Danh sách các Session của User được hiển thị trên giao diện |
| **Kịch bản chính** | 1. User click "Tài liệu" tại sidebar<br>2. Hệ thống gửi yêu cầu lấy danh sách Session đến backend<br>3. Backend trả về danh sách Session<br>4. Hệ thống hiển thị danh sách Session cho User |
| **Ngoại lệ** | - User không có Session nào: hiển thị thông báo "Không tìm thấy phiên nào"<br>- Lỗi load Session: hiển thị thông báo "Không thể tải phiên" |

---

## Use Case: Xem chi tiết Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem chi tiết Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Session" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin chi tiết Session được hiển thị trên giao diện |
| **Kịch bản chính** | 1. User click vào 1 Session trong danh sách<br>2. Hệ thống gửi yêu cầu lấy chi tiết Session đến backend<br>3. Backend trả về thông tin Session<br>4. Hệ thống hiển thị chi tiết Session cho User |
| **Ngoại lệ** | - Session không tồn tại: hiển thị thông báo "Không tìm thấy phiên"<br>- Lỗi load Session: hiển thị thông báo "Không thể tải phiên" |

---

## Use Case: Upload Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Upload Document |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Document được upload vào Session<br>- Danh sách Document được cập nhật |
| **Kịch bản chính** | 1. User click "Upload Document"<br>2. Hệ thống hiển thị dialog upload<br>3. User chọn file và click "Upload"<br>4. Hệ thống gửi yêu cầu upload đến backend<br>5. Backend lưu Document và trả về thông tin Document mới<br>6. Hệ thống hiển thị thông báo "Upload thành công" |
| **Ngoại lệ** | - File không hợp lệ: hiển thị lỗi validation<br>- Lỗi upload: hiển thị thông báo "Không thể upload tài liệu" |

---

## Use Case: Xem danh sách Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Document |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Danh sách các Document trong Session được hiển thị |
| **Kịch bản chính** | 1. User click "Tài liệu" từ sidebar (trong màn hình chi tiết Session)<br>2. Hệ thống gửi yêu cầu lấy danh sách Document đến backend<br>3. Backend trả về danh sách Document<br>4. Hệ thống hiển thị danh sách Document cho User |
| **Ngoại lệ** | - Session không có Document nào: hiển thị thông báo "Không tìm thấy tài liệu nào"<br>- Lỗi load Document: hiển thị thông báo "Không thể tải tài liệu" |

---

## Use Case: Cập nhật Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Cập nhật Document |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Document" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin Document được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. User click icon cập nhật trên Document<br>2. Hệ thống mở dialog cập nhật với thông tin hiện tại<br>3. User cập nhật thông tin và click "Lưu phiên"<br>4. Hệ thống gửi yêu cầu cập nhật đến backend<br>5. Backend cập nhật Document và trả về Document mới<br>6. Hệ thống cập nhật item trong danh sách và hiển thị thông báo thành công |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Document không tồn tại: hiển thị thông báo "Không tìm thấy tài liệu"<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật tài liệu" |

---

## Use Case: Xóa Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Document |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Document" đã hoàn thành |
| **Hậu điều kiện** | - Document được xóa khỏi hệ thống<br>- Danh sách Document được cập nhật |
| **Kịch bản chính** | 1. User click icon xóa trên Document<br>2. Hệ thống yêu cầu xác nhận xóa<br>3. User xác nhận muốn xóa<br>4. Hệ thống gửi yêu cầu xóa đến backend<br>5. Backend xóa Document và trả về trạng thái thành công<br>6. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo thành công |
| **Ngoại lệ** | - Document không tồn tại: hiển thị thông báo "Không tìm thấy tài liệu"<br>- Lỗi xóa: hiển thị thông báo "Không thể xóa tài liệu" |
