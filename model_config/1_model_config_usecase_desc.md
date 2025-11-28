# Use Case Description - Quản lý ModelConfig và ModelWrapper

Chức năng quản lý ModelConfig và ModelWrapper (ModelConfig & ModelWrapper Management) có các giao diện tương tác với User:
- **Đăng nhập** → đề xuất UC đăng nhập
- **Xem trang Model** → đề xuất UC xem trang Model
- **Xem danh sách ModelConfig** → đề xuất UC xem danh sách ModelConfig
- **Xem danh sách ModelWrapper** → đề xuất UC xem danh sách ModelWrapper
- **Chỉnh sửa ModelConfig** → đề xuất UC chỉnh sửa ModelConfig
- **Tạo ModelConfig mới** → đề xuất UC tạo ModelConfig mới
- **Xóa ModelConfig** → đề xuất UC xóa ModelConfig
- **Toggle trạng thái sử dụng ModelConfig** → đề xuất UC toggle trạng thái ModelConfig
- **Chỉnh sửa ModelWrapper** → đề xuất UC chỉnh sửa ModelWrapper (Admin only)
- **Tạo ModelWrapper mới** → đề xuất UC tạo ModelWrapper mới (Admin only)
- **Xóa ModelWrapper** → đề xuất UC xóa ModelWrapper (Admin only)
- **Preload ModelConfig** → đề xuất UC preload ModelConfig

**Quan hệ giữa các Use Case:**
Đăng nhập là bắt buộc để truy cập trang cấu hình Model và sử dụng các chức năng quản lý ModelConfig và Wrapper. Do đó:
- UC "Xem trang Model" include UC "Đăng nhập"
- UC "Xem trang Model" hiển thị giao diện với 2 tab: "Cấu hình mô hình" và "Cấu hình Wrapper"
- UC "Xem danh sách ModelConfig" được kích hoạt khi User click tab "Cấu hình mô hình"
- UC "Xem danh sách ModelConfig" có các extension points: Tạo ModelConfig, Chỉnh sửa ModelConfig, Xóa ModelConfig, Toggle trạng thái ModelConfig
- UC "Xem danh sách ModelWrapper" được kích hoạt khi User click tab "Cấu hình Wrapper"
- UC "Xem danh sách ModelWrapper" có các extension points: Tạo ModelWrapper (Admin only), Chỉnh sửa ModelWrapper (Admin only), Xóa ModelWrapper (Admin only)

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Đăng nhập** | UC này cho phép User đăng nhập vào hệ thống. |
| **Xem trang Model** | UC này cho phép User truy cập trang cấu hình Model. |
| **Xem danh sách ModelConfig** | UC này cho phép User xem tất cả các ModelConfig của mình được tổ chức theo task type. |
| **Xem danh sách ModelWrapper** | UC này cho phép User xem tất cả các ModelWrapper có sẵn trong hệ thống. |
| **Chỉnh sửa ModelConfig** | UC này cho phép User cập nhật thông tin của một ModelConfig hiện có. Trong quá trình chỉnh sửa, User có thể chọn Wrapper từ danh sách hiển thị. |
| **Tạo ModelConfig mới** | UC này cho phép User tạo một ModelConfig mới. User cần chọn Wrapper từ danh sách hiển thị trước khi tạo. |
| **Xóa ModelConfig** | UC này cho phép User xóa một ModelConfig khỏi hệ thống. |
| **Toggle trạng thái sử dụng ModelConfig** | UC này cho phép User bật/tắt trạng thái sử dụng của một ModelConfig cho task type cụ thể. |
| **Chỉnh sửa ModelWrapper** | UC này cho phép Admin cập nhật thông tin của một ModelWrapper hiện có. |
| **Tạo ModelWrapper mới** | UC này cho phép Admin tạo một ModelWrapper mới cho hệ thống. |
| **Xóa ModelWrapper** | UC này cho phép Admin xóa một ModelWrapper khỏi hệ thống. |
| **Preload ModelConfig** | UC này cho phép User tải trước các ModelConfig mặc định vào hệ thống. |
