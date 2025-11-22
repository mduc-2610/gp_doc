# Scenario - Quản lý ModelConfig và ModelWrapper

## Use Case: Xem danh sách ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công |
| **Hậu điều kiện** | - Danh sách các ModelConfig của User được hiển thị trên giao diện |
| **Kịch bản chính** | 1. Tại giao diện chính, User click vào nút "Mô hình" tại sidebar <br>2. Hệ thống hiển thị giao diện cấu hình Model với tab "Cấu Hình Mô Hình" <br>3. Hệ thống load và hiển thị danh sách tất cả các ModelConfig của User |
| **Ngoại lệ** | - User không có ModelConfig nào: hiển thị thông báo "Chưa Có Mô Hình Nào Được Cấu Hình" với nút "Tải Mô Hình Mẫu"<br>- Lỗi load ModelConfig: hiển thị thông báo "Lỗi tải mô hình" |

---

## Use Case: Tạo ModelConfig mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo ModelConfig mới |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện cấu hình Model |
| **Hậu điều kiện** | - ModelConfig mới được tạo trong hệ thống<br>- ModelConfig được thêm vào danh sách |
| **Kịch bản chính** | 1. Tại giao diện cấu hình Model, User click vào tab "Cấu Hình Mô Hình"<br>2. Hệ thống hiển thị giao diện của tab "Cấu Hình Mô Hình" với danh sách ModelConfig theo task type<br>3. User chọn task type cần tạo model<br>4. Hệ thống hiển thị danh sách ModelConfig cho task type đó<br>5. User click nút "Tạo Mô Hình"<br>6. Hệ thống mở dialog tạo ModelConfig mới và gọi API để lấy danh sách Wrapper<br>7. Backend trả về danh sách Wrapper theo task type<br>8. Hệ thống hiển thị bảng Wrapper trong dialog<br>9. User chọn một Wrapper từ bảng<br>10. User nhập các thông tin và click nút "Lưu Thay Đổi":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên ModelConfig<br>&nbsp;&nbsp;&nbsp;&nbsp;- Wrapper (đã chọn)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Model Type (Remote/Local)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Language<br>&nbsp;&nbsp;&nbsp;&nbsp;- Extra Config (JSON, optional)<br>11. Hệ thống gửi yêu cầu tạo ModelConfig đến backend<br>12. Backend tạo ModelConfig và trả về ModelConfig mới<br>13. Hệ thống cập nhật danh sách và hiển thị thông báo "Mô hình đã được tạo thành công" |
| **Ngoại lệ** | - Không chọn Wrapper: hiển thị lỗi validation<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo ModelConfig: hiển thị thông báo "Tạo mô hình thất bại" |

## Use Case: Chỉnh sửa ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện cấu hình Model |
| **Hậu điều kiện** | - Thông tin ModelConfig được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện cấu hình Model, User click vào tab "Cấu Hình Mô Hình"<br>2. Hệ thống hiển thị giao diện của tab "Cấu Hình Mô Hình"<br>3. User chọn task type<br>4. Hệ thống hiển thị danh sách ModelConfig cho task type đó<br>5. User click nút "Chỉnh Sửa" trên ModelConfig muốn chỉnh sửa<br>6. Hệ thống mở dialog chỉnh sửa với thông tin hiện tại và gọi API để lấy danh sách Wrapper<br>7. Backend trả về danh sách Wrapper theo task type<br>8. Hệ thống hiển thị bảng Wrapper với Wrapper hiện tại được chọn<br>9. User có thể thay đổi Wrapper từ bảng<br>10. User cập nhật thông tin và click nút "Lưu Thay Đổi"<br>11. Hệ thống gửi yêu cầu cập nhật đến backend<br>12. Backend cập nhật ModelConfig và trả về ModelConfig đã cập nhật<br>13. Hệ thống cập nhật danh sách và hiển thị thông báo "Mô hình đã được cập nhật thành công" |
| **Ngoại lệ** | - ModelConfig không tồn tại: hiển thị lỗi "Không tìm thấy mô hình"<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi cập nhật: hiển thị thông báo "Cập nhật mô hình thất bại" |

## Use Case: Xóa ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện cấu hình Model |
| **Hậu điều kiện** | - ModelConfig được xóa khỏi hệ thống |
| **Kịch bản chính** | 1. Tại giao diện cấu hình Model, User click vào tab "Cấu Hình Mô Hình"<br>2. Hệ thống hiển thị giao diện của tab "Cấu Hình Mô Hình"<br>3. User chọn task type<br>4. Hệ thống hiển thị danh sách ModelConfig<br>5. User click nút "Xóa" trên ModelConfig muốn xóa<br>6. Hệ thống hiển thị dialog xác nhận xóa<br>7. User xác nhận xóa<br>8. Hệ thống gửi yêu cầu xóa đến backend<br>9. Backend xóa ModelConfig khỏi database và trả về trạng thái thành công<br>10. Hệ thống cập nhật danh sách và hiển thị thông báo "Mô hình đã được xóa thành công" |
| **Ngoại lệ** | - ModelConfig không tồn tại: hiển thị lỗi "Không tìm thấy mô hình"<br>- ModelConfig đang được sử dụng: hiển thị lỗi "Mô hình đang được sử dụng"<br>- Lỗi xóa: hiển thị thông báo "Xóa mô hình thất bại" |

## Use Case: Toggle trạng thái sử dụng ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Toggle trạng thái sử dụng ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện cấu hình Model |
| **Hậu điều kiện** | - Trạng thái sử dụng của ModelConfig được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện cấu hình Model, User click vào tab "Cấu Hình Mô Hình"<br>2. Hệ thống hiển thị giao diện của tab "Cấu Hình Mô Hình"<br>3. User chọn task type<br>4. Hệ thống hiển thị danh sách ModelConfig<br>5. User toggle switch trạng thái "Is Used" của ModelConfig<br>6. Hệ thống gửi yêu cầu cập nhật trạng thái đến backend<br>7. Backend cập nhật trạng thái is_used của ModelConfig và tự động set is_used=false cho các ModelConfig khác cùng task type<br>8. Hệ thống cập nhật danh sách và hiển thị thông báo cập nhật thành công |
| **Ngoại lệ** | - ModelConfig không tồn tại: hiển thị lỗi "Không tìm thấy mô hình"<br>- Lỗi cập nhật: hiển thị thông báo "Cập nhật mô hình thất bại" |

## Use Case: Preload ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Preload ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện cấu hình Model |
| **Hậu điều kiện** | - Các ModelConfig mặc định được tạo trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện cấu hình Model, User click vào tab "Model Configurations"<br>2. Hệ thống hiển thị giao diện của tab "Model Configurations"<br>3. User chọn task type<br>4. Hệ thống hiển thị danh sách ModelConfig (có thể rỗng)<br>5. User click nút "Preload Models"<br>6. Hệ thống hiển thị dialog xác nhận preload<br>7. User xác nhận<br>8. Hệ thống gửi yêu cầu preload đến backend<br>9. Backend tạo các ModelConfig mặc định từ template và trả về danh sách ModelConfig mới<br>10. Hệ thống cập nhật danh sách và hiển thị thông báo "Models preloaded successfully" |
| **Ngoại lệ** | - Lỗi preload: hiển thị thông báo lỗi từ server |

---

## Use Case: Xem danh sách ModelWrapper

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách ModelWrapper |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công |
| **Hậu điều kiện** | - Danh sách các ModelWrapper được hiển thị trên giao diện |
| **Kịch bản chính** | 1. Tại giao diện chính, User click vào nút "Mô hình" tại sidebar <br>2. Hệ thống hiển thị giao diện cấu hình Model, User click vào tab "Model Wrappers" <br>3. Hệ thống load và hiển thị danh sách tất cả các ModelWrapper |
| **Ngoại lệ** | - Không có ModelWrapper nào: hiển thị thông báo "Không có ModelWrapper nào"<br>- Lỗi load ModelWrapper: hiển thị thông báo lỗi từ server |

---

## Use Case: Tạo ModelWrapper mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo ModelWrapper mới |
| **Actor** | Admin |
| **Tiền điều kiện** | - Admin đã đăng nhập thành công<br>- Admin đang ở giao diện cấu hình Model |
| **Hậu điều kiện** | - ModelWrapper mới được tạo trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện cấu hình Model, Admin click vào tab "Model Wrappers"<br>2. Hệ thống hiển thị giao diện của tab "Model Wrappers" với danh sách Wrapper<br>3. Admin click nút "Add Wrapper"<br>4. Hệ thống mở dialog tạo Wrapper mới<br>5. Admin nhập các thông tin và click nút "Save":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên Wrapper<br>&nbsp;&nbsp;&nbsp;&nbsp;- Task Type<br>&nbsp;&nbsp;&nbsp;&nbsp;- Instruction (hướng dẫn cấu hình)<br>6. Hệ thống gửi yêu cầu tạo Wrapper đến backend<br>7. Backend tạo Wrapper và trả về Wrapper mới<br>8. Hệ thống cập nhật danh sách và hiển thị thông báo "Wrapper created successfully" |
| **Ngoại lệ** | - User Role không phải Admin: không hiển thị nút tạo<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Wrapper đã tồn tại (duplicate name + task_type): hiển thị lỗi<br>- Lỗi tạo Wrapper: hiển thị thông báo lỗi từ server |

## Use Case: Chỉnh sửa ModelWrapper

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa ModelWrapper |
| **Actor** | Admin |
| **Tiền điều kiện** | - Admin đã đăng nhập thành công<br>- Admin đang ở giao diện cấu hình Model |
| **Hậu điều kiện** | - Thông tin ModelWrapper được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện cấu hình Model, Admin click vào tab "Model Wrappers"<br>2. Hệ thống hiển thị giao diện của tab "Model Wrappers"<br>3. Admin click nút "Edit" trên Wrapper muốn chỉnh sửa<br>4. Hệ thống mở dialog chỉnh sửa với thông tin hiện tại<br>5. Admin cập nhật thông tin và click nút "Save"<br>6. Hệ thống gửi yêu cầu cập nhật đến backend<br>7. Backend cập nhật Wrapper và trả về Wrapper đã cập nhật<br>8. Hệ thống cập nhật danh sách và hiển thị thông báo "Wrapper updated successfully" |
| **Ngoại lệ** | - User Role không phải Admin: không hiển thị nút edit<br>- Wrapper không tồn tại: hiển thị lỗi<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server |

## Use Case: Xóa ModelWrapper

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa ModelWrapper |
| **Actor** | Admin |
| **Tiền điều kiện** | - Admin đã đăng nhập thành công<br>- Admin đang ở giao diện cấu hình Model |
| **Hậu điều kiện** | - ModelWrapper được xóa khỏi hệ thống |
| **Kịch bản chính** | 1. Tại giao diện cấu hình Model, Admin click vào tab "Model Wrappers"<br>2. Hệ thống hiển thị giao diện của tab "Model Wrappers"<br>3. Admin click nút "Delete" trên Wrapper muốn xóa<br>4. Hệ thống hiển thị dialog xác nhận xóa<br>5. Admin xác nhận xóa<br>6. Hệ thống gửi yêu cầu xóa đến backend<br>7. Backend xóa Wrapper và các ModelConfig liên quan (cascade delete) và trả về trạng thái thành công<br>8. Hệ thống cập nhật danh sách và hiển thị thông báo "Wrapper deleted successfully" |
| **Ngoại lệ** | - User Role không phải Admin: không hiển thị nút delete<br>- Wrapper không tồn tại: hiển thị lỗi<br>- Lỗi xóa: hiển thị thông báo lỗi từ server |
