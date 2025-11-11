# Use Case Description - Quản lý các Session

Chức năng quản lý các Session (Session Management) có các giao diện tương tác với User:
- **Đăng nhập** → đề xuất UC đăng nhập
- **Xem danh sách Session** → đề xuất UC xem danh sách Session
- **Tạo Session mới** → đề xuất UC tạo Session mới
- **Xem chi tiết Session** → đề xuất UC xem chi tiết Session
- **Chỉnh sửa Session** → đề xuất UC chỉnh sửa Session
- **Xóa Session** → đề xuất UC xóa Session

**Quan hệ giữa các Use Case:**
Đăng nhập, xem danh sách Session là bắt buộc mới có thể thực hiện được các thao tác khác (tạo, xem chi tiết, chỉnh sửa, xóa Session). Do đó:
- UC "Xem danh sách Session" include UC "Đăng nhập"
- UC "Tạo Session mới", "Xem chi tiết Session", "Chỉnh sửa Session", "Xóa Session" đều include UC "Xem danh sách Session"
- UC "Chỉnh sửa Session" include UC "Xem chi tiết Session"

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Đăng nhập** | UC này cho phép User xác thực danh tính để truy cập hệ thống. |
| **Xem danh sách Session** | UC này cho phép User xem danh sách tất cả các Session đã tạo. |
| **Tạo Session mới** | UC này cho phép User tạo một Session mới. |
| **Xem chi tiết Session** | UC này cho phép User xem thông tin chi tiết của một Session. |
| **Chỉnh sửa Session** | UC này cho phép User cập nhật thông tin của một Session hiện có. |
| **Xóa Session** | UC này cho phép User xóa một Session. |
