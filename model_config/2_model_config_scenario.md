# Scenario - Quản lý ModelConfig và ModelWrapper

## Use Case: Xem danh sách ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập<br>- User click "Mô hình" từ sidebar |
| **Hậu điều kiện** | - Danh sách ModelConfig được hiển thị |
| **Kịch bản chính** | 1. User click "Mô hình" từ sidebar<br>2. Hệ thống hiển thị ModelConfigPage.tsx<br>3. User click tab "Cấu hình mô hình"<br>4. Hệ thống gọi ModelConfigTab.tsx<br>5. ModelConfigTab.tsx gọi loadModelsFromUser()<br>6. Hệ thống gọi ModelConfigService.getModelsByUser()<br>7. Hệ thống gửi GET request đến backend<br>8. Backend trả về danh sách ModelConfig<br>9. Hệ thống hiển thị danh sách ModelConfig cho User |

---

## Use Case: Tạo ModelConfig mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo ModelConfig mới |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách ModelConfig" đã hoàn thành |
| **Hậu điều kiện** | - ModelConfig mới được tạo và thêm vào danh sách |
| **Kịch bản chính** | 1. User click "Tạo Mô Hình"<br>2. Hệ thống hiển thị dialog và load danh sách Wrapper<br>3. User chọn Wrapper và nhập thông tin<br>4. User click "Lưu Thay Đổi"<br>5. Hệ thống tạo ModelConfig và cập nhật danh sách |

---

## Use Case: Chỉnh sửa ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách ModelConfig" đã hoàn thành |
| **Hậu điều kiện** | - ModelConfig được cập nhật |
| **Kịch bản chính** | 1. User click icon update trên ModelConfig<br>2. Hệ thống hiển thị dialog với thông tin hiện tại<br>3. User chỉnh sửa thông tin và click "Lưu Thay Đổi"<br>4. Hệ thống cập nhật ModelConfig |

---

## Use Case: Xóa ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách ModelConfig" đã hoàn thành |
| **Hậu điều kiện** | - ModelConfig bị xóa khỏi hệ thống |
| **Kịch bản chính** | 1. User click icon delete trên ModelConfig<br>2. Hệ thống hiển thị dialog xác nhận<br>3. User click "Xác nhận"<br>4. Hệ thống xóa ModelConfig |

---

## Use Case: Toggle trạng thái sử dụng ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Toggle trạng thái sử dụng ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách ModelConfig" đã hoàn thành |
| **Hậu điều kiện** | - Trạng thái sử dụng ModelConfig được cập nhật |
| **Kịch bản chính** | 1. User click Switch toggle trên ModelConfig<br>2. Hệ thống cập nhật trạng thái is_in_use |

---

## Use Case: Preload ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Preload ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách ModelConfig" đã hoàn thành<br>- User chưa có ModelConfig nào |
| **Hậu điều kiện** | - ModelConfig mặc định được tải vào hệ thống |
| **Kịch bản chính** | 1. User click "Tải Mô Hình Mẫu"<br>2. Hệ thống gửi yêu cầu preload<br>3. Backend tạo ModelConfig mặc định<br>4. Hệ thống load lại danh sách ModelConfig |

---

## Use Case: Xem danh sách ModelWrapper

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách ModelWrapper |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập<br>- User click "Mô hình" từ sidebar |
| **Hậu điều kiện** | - Danh sách ModelWrapper được hiển thị |
| **Kịch bản chính** | 1. User click "Mô hình" từ sidebar<br>2. Hệ thống hiển thị ModelConfigPage.tsx<br>3. User click tab "Cấu hình Wrapper"<br>4. Hệ thống gọi ModelWrapperTab.tsx<br>5. ModelWrapperTab.tsx gọi loadWrappers()<br>6. Hệ thống gọi ModelConfigService.getWrappers()<br>7. Hệ thống gửi GET request đến backend<br>8. Backend trả về danh sách ModelWrapper<br>9. Hệ thống hiển thị danh sách ModelWrapper cho User |

---

## Use Case: Tạo ModelWrapper mới (Admin only)

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo ModelWrapper mới |
| **Actor** | Admin |
| **Tiền điều kiện** | - UC "Xem danh sách ModelWrapper" đã hoàn thành<br>- User có role Admin |
| **Hậu điều kiện** | - ModelWrapper mới được tạo |
| **Kịch bản chính** | 1. Admin click "Tạo Wrapper"<br>2. Hệ thống hiển thị dialog tạo Wrapper<br>3. Admin nhập thông tin và click "Tạo"<br>4. Hệ thống tạo ModelWrapper và cập nhật danh sách |

---

## Use Case: Chỉnh sửa ModelWrapper (Admin only)

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa ModelWrapper |
| **Actor** | Admin |
| **Tiền điều kiện** | - UC "Xem danh sách ModelWrapper" đã hoàn thành<br>- User có role Admin |
| **Hậu điều kiện** | - ModelWrapper được cập nhật |
| **Kịch bản chính** | 1. Admin click icon update trên ModelWrapper<br>2. Hệ thống hiển thị dialog với thông tin hiện tại<br>3. Admin chỉnh sửa và click "Cập nhật"<br>4. Hệ thống cập nhật ModelWrapper |

---

## Use Case: Xóa ModelWrapper (Admin only)

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa ModelWrapper |
| **Actor** | Admin |
| **Tiền điều kiện** | - UC "Xem danh sách ModelWrapper" đã hoàn thành<br>- User có role Admin |
| **Hậu điều kiện** | - ModelWrapper bị xóa khỏi hệ thống |
| **Kịch bản chính** | 1. Admin click icon delete trên ModelWrapper<br>2. Hệ thống hiển thị dialog xác nhận<br>3. Admin click "Cập nhật"<br>4. Hệ thống xóa ModelWrapper |
