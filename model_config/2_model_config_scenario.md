# Scenario - Quản lý ModelConfig và ModelWrapper

## Use Case: Xem danh sách ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập<br>- User đang ở trang quản lý mô hình |
| **Hậu điều kiện** | - Danh sách ModelConfig được hiển thị trên giao diện |
| **Kịch bản chính** | 1. User click "Mô hình" từ sidebar<br>2. Hệ thống hiển thị trang quản lý mô hình<br>3. User click tab "Cấu hình mô hình"<br>4. Hệ thống tự động gửi yêu cầu lấy danh sách ModelConfig đến backend<br>5. Backend trả về danh sách ModelConfig của User<br>6. Hệ thống hiển thị danh sách ModelConfig với thông tin: tên, wrapper, API key |
| **Ngoại lệ** | - Không có ModelConfig: hiển thị thông báo "Chưa có cấu hình mô hình nào" và nút "Tải Mô Hình Mẫu"<br>- Lỗi load: hiển thị thông báo "Không thể tải danh sách mô hình" |

---

## Use Case: Tạo ModelConfig mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo ModelConfig mới |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách ModelConfig" đã hoàn thành |
| **Hậu điều kiện** | - ModelConfig mới được tạo và thêm vào danh sách<br>- User có thể sử dụng ModelConfig mới trong hệ thống |
| **Kịch bản chính** | 1. User click "Tạo Mô Hình"<br>2. Hệ thống mở dialog tạo mô hình và tự động load danh sách Wrapper khả dụng<br>3. User chọn Wrapper từ dropdown<br>4. User nhập tên mô hình, API key và các thông tin cấu hình<br>5. User click "Lưu Thay Đổi"<br>6. Hệ thống validate thông tin và gửi yêu cầu tạo ModelConfig đến backend<br>7. Backend tạo ModelConfig mới và trả về thông tin đã lưu<br>8. Hệ thống cập nhật danh sách ModelConfig và hiển thị thông báo thành công<br>9. Dialog đóng lại |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation tương ứng<br>- Tên mô hình trùng: hiển thị thông báo "Tên mô hình đã tồn tại"<br>- Lỗi tạo: hiển thị thông báo "Không thể tạo mô hình" |

---

## Use Case: Chỉnh sửa ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách ModelConfig" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin ModelConfig được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. User click icon chỉnh sửa trên ModelConfig<br>2. Hệ thống mở dialog cập nhật với thông tin hiện tại và load danh sách Wrapper<br>3. User chỉnh sửa thông tin (tên, wrapper, API key, cấu hình)<br>4. User click "Lưu Thay Đổi"<br>5. Hệ thống validate thông tin và gửi yêu cầu cập nhật đến backend<br>6. Backend cập nhật ModelConfig và trả về thông tin mới<br>7. Hệ thống cập nhật item trong danh sách và hiển thị thông báo thành công<br>8. Dialog đóng lại |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- ModelConfig không tồn tại: hiển thị thông báo "Không tìm thấy mô hình"<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật mô hình" |

---

## Use Case: Xóa ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách ModelConfig" đã hoàn thành |
| **Hậu điều kiện** | - ModelConfig được xóa khỏi hệ thống<br>- Danh sách ModelConfig được cập nhật |
| **Kịch bản chính** | 1. User click icon xóa trên ModelConfig<br>2. Hệ thống mở dialog xác nhận xóa với tên ModelConfig<br>3. User click "Xác nhận"<br>4. Hệ thống gửi yêu cầu xóa đến backend<br>5. Backend xóa ModelConfig và trả về trạng thái thành công<br>6. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo thành công<br>7. Dialog đóng lại |
| **Ngoại lệ** | - ModelConfig không tồn tại: hiển thị thông báo "Không tìm thấy mô hình"<br>- ModelConfig đang được sử dụng: hiển thị thông báo "Mô hình đang được sử dụng, không thể xóa"<br>- Lỗi xóa: hiển thị thông báo "Không thể xóa mô hình" |

---

## Use Case: Preload ModelConfig

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Preload ModelConfig |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách ModelConfig" đã hoàn thành<br>- User chưa có ModelConfig nào trong hệ thống |
| **Hậu điều kiện** | - ModelConfig mặc định được tạo tự động<br>- Danh sách ModelConfig được cập nhật với các mô hình mẫu |
| **Kịch bản chính** | 1. User click "Tải Mô Hình Mẫu"<br>2. Hệ thống hiển thị dialog xác nhận<br>3. User click "Xác nhận"<br>4. Hệ thống gửi yêu cầu preload đến backend<br>5. Backend tạo các ModelConfig mặc định (GPT-3.5, GPT-4, Claude, v.v.)<br>6. Backend trả về trạng thái thành công<br>7. Hệ thống tự động load lại danh sách ModelConfig<br>8. Hệ thống hiển thị thông báo "Đã tải các mô hình mẫu thành công"<br>9. Dialog đóng lại |
| **Ngoại lệ** | - User đã có ModelConfig: nút "Tải Mô Hình Mẫu" bị ẩn<br>- Lỗi preload: hiển thị thông báo "Không thể tải mô hình mẫu" |

---

## Use Case: Xem danh sách ModelWrapper

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách ModelWrapper |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập<br>- User đang ở trang quản lý mô hình |
| **Hậu điều kiện** | - Danh sách ModelWrapper được hiển thị trên giao diện |
| **Kịch bản chính** | 1. User click "Mô hình" từ sidebar<br>2. Hệ thống hiển thị trang quản lý mô hình<br>3. User click tab "Cấu hình Wrapper"<br>4. Hệ thống tự động gửi yêu cầu lấy danh sách ModelWrapper đến backend<br>5. Backend trả về danh sách tất cả ModelWrapper trong hệ thống<br>6. Hệ thống hiển thị danh sách ModelWrapper với thông tin: tên, mô tả, provider |
| **Ngoại lệ** | - Không có ModelWrapper: hiển thị thông báo "Chưa có wrapper nào"<br>- Lỗi load: hiển thị thông báo "Không thể tải danh sách wrapper" |

---

## Use Case: Tạo ModelWrapper mới (Admin only)

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo ModelWrapper mới |
| **Actor** | Admin |
| **Tiền điều kiện** | - UC "Xem danh sách ModelWrapper" đã hoàn thành<br>- User có quyền Admin |
| **Hậu điều kiện** | - ModelWrapper mới được tạo và thêm vào hệ thống<br>- User có thể sử dụng Wrapper mới khi tạo ModelConfig |
| **Kịch bản chính** | 1. Admin click "Tạo Wrapper"<br>2. Hệ thống mở dialog tạo Wrapper<br>3. Admin nhập tên, mô tả, provider và các thông tin cấu hình<br>4. Admin click "Tạo"<br>5. Hệ thống validate thông tin và gửi yêu cầu tạo Wrapper đến backend<br>6. Backend tạo ModelWrapper mới và trả về thông tin đã lưu<br>7. Hệ thống cập nhật danh sách ModelWrapper và hiển thị thông báo thành công<br>8. Dialog đóng lại |
| **Ngoại lệ** | - User không có quyền: nút "Tạo Wrapper" bị ẩn<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Tên Wrapper trùng: hiển thị thông báo "Tên wrapper đã tồn tại"<br>- Lỗi tạo: hiển thị thông báo "Không thể tạo wrapper" |

---

## Use Case: Chỉnh sửa ModelWrapper (Admin only)

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa ModelWrapper |
| **Actor** | Admin |
| **Tiền điều kiện** | - UC "Xem danh sách ModelWrapper" đã hoàn thành<br>- User có quyền Admin |
| **Hậu điều kiện** | - Thông tin ModelWrapper được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Admin click icon chỉnh sửa trên ModelWrapper<br>2. Hệ thống mở dialog cập nhật với thông tin hiện tại<br>3. Admin chỉnh sửa thông tin (tên, mô tả, provider, cấu hình)<br>4. Admin click "Cập nhật"<br>5. Hệ thống validate thông tin và gửi yêu cầu cập nhật đến backend<br>6. Backend cập nhật ModelWrapper và trả về thông tin mới<br>7. Hệ thống cập nhật item trong danh sách và hiển thị thông báo thành công<br>8. Dialog đóng lại |
| **Ngoại lệ** | - User không có quyền: icon chỉnh sửa bị ẩn<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- ModelWrapper không tồn tại: hiển thị thông báo "Không tìm thấy wrapper"<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật wrapper" |

---

## Use Case: Xóa ModelWrapper (Admin only)

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa ModelWrapper |
| **Actor** | Admin |
| **Tiền điều kiện** | - UC "Xem danh sách ModelWrapper" đã hoàn thành<br>- User có quyền Admin |
| **Hậu điều kiện** | - ModelWrapper được xóa khỏi hệ thống<br>- Danh sách ModelWrapper được cập nhật |
| **Kịch bản chính** | 1. Admin click icon xóa trên ModelWrapper<br>2. Hệ thống mở dialog xác nhận xóa với tên ModelWrapper<br>3. Admin click "Xác nhận"<br>4. Hệ thống gửi yêu cầu xóa đến backend<br>5. Backend xóa ModelWrapper và trả về trạng thái thành công<br>6. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo thành công<br>7. Dialog đóng lại |
| **Ngoại lệ** | - User không có quyền: icon xóa bị ẩn<br>- ModelWrapper không tồn tại: hiển thị thông báo "Không tìm thấy wrapper"<br>- ModelWrapper đang được sử dụng: hiển thị thông báo "Wrapper đang được sử dụng bởi các mô hình, không thể xóa"<br>- Lỗi xóa: hiển thị thông báo "Không thể xóa wrapper" |
