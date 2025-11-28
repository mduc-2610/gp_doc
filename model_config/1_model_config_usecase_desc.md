# Use Case Description - Quản lý ModelConfig và ModelWrapper

Chức năng quản lý ModelConfig và ModelWrapper (ModelConfig & ModelWrapper Management) có các giao diện tương tác với User:
- **Đăng nhập** → đề xuất UC đăng nhập
- **Xem danh sách ModelConfig** → đề xuất UC xem danh sách ModelConfig
- **Tạo ModelConfig** → đề xuất UC tạo ModelConfig
- **Chỉnh sửa ModelConfig** → đề xuất UC chỉnh sửa ModelConfig
- **Xóa ModelConfig** → đề xuất UC xóa ModelConfig
- **Preload ModelConfig** → đề xuất UC tải mô hình mẫu
- **Xem danh sách ModelWrapper** → đề xuất UC xem danh sách ModelWrapper
- **Tạo ModelWrapper** → đề xuất UC tạo ModelWrapper (Admin only)
- **Chỉnh sửa ModelWrapper** → đề xuất UC chỉnh sửa ModelWrapper (Admin only)
- **Xóa ModelWrapper** → đề xuất UC xóa ModelWrapper (Admin only)

**Quan hệ giữa các Use Case:**
Đăng nhập là bắt buộc để truy cập hệ thống và sử dụng các chức năng quản lý ModelConfig và Wrapper. Do đó:
- UC "Đăng nhập" được User và Admin truy cập
- UC "Đăng nhập" include UC "Xem danh sách ModelConfig"
- UC "Đăng nhập" include UC "Xem danh sách ModelWrapper"
- UC "Xem danh sách ModelConfig" có các extension points: Tạo ModelConfig, Chỉnh sửa ModelConfig, Xóa ModelConfig, Preload ModelConfig
- UC "Preload ModelConfig" extend từ UC "Xem danh sách ModelConfig" khi User chưa có ModelConfig nào
- UC "Xem danh sách ModelWrapper" có các extension points: Tạo ModelWrapper, Chỉnh sửa ModelWrapper, Xóa ModelWrapper
- UC "Xóa ModelWrapper" extend từ UC "Xem danh sách ModelWrapper" và được thực hiện bởi Admin

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Đăng nhập** | UC này cho phép User xác thực danh tính để truy cập hệ thống. |
| **Xem danh sách ModelConfig** | UC này cho phép User xem tất cả các ModelConfig của mình được tổ chức theo task type. |
| **Tạo ModelConfig** | UC này cho phép User tạo một ModelConfig mới. User cần chọn Wrapper từ danh sách hiển thị trước khi tạo. |
| **Chỉnh sửa ModelConfig** | UC này cho phép User cập nhật thông tin của một ModelConfig hiện có. Trong quá trình chỉnh sửa, User có thể chọn Wrapper từ danh sách hiển thị. |
| **Xóa ModelConfig** | UC này cho phép User xóa một ModelConfig khỏi hệ thống. |
| **Preload ModelConfig** | UC này cho phép User tải các ModelConfig mẫu (GPT-3.5, GPT-4, Claude, v.v.) khi chưa có cấu hình nào trong hệ thống. |
| **Xem danh sách ModelWrapper** | UC này cho phép User xem tất cả các ModelWrapper có sẵn trong hệ thống. |
| **Tạo ModelWrapper** | UC này cho phép Admin tạo một ModelWrapper mới cho hệ thống. |
| **Chỉnh sửa ModelWrapper** | UC này cho phép Admin cập nhật thông tin của một ModelWrapper hiện có. |
| **Xóa ModelWrapper** | UC này cho phép Admin xóa một ModelWrapper khỏi hệ thống. |
