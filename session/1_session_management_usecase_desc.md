# Use Case Description - Quản lý Session

Chức năng quản lý Session có các giao diện tương tác với User:
- **Đăng nhập** → đề xuất UC đăng nhập
- **Xem danh sách Session** → đề xuất UC xem danh sách Session
- **Tạo Session** → đề xuất UC tạo Session
- **Xóa Session** → đề xuất UC xóa Session
- **Xem chi tiết Session** → đề xuất UC xem chi tiết Session
- **Chỉnh sửa Session** → đề xuất UC chỉnh sửa Session

**Quan hệ giữa các Use Case:**
Đăng nhập là bắt buộc mới có thể thực hiện được các thao tác khác (xem danh sách Session, tạo Session, xóa Session, xem chi tiết Session, chỉnh sửa Session). Do đó:
- UC "Xem danh sách Session" include UC "Đăng nhập"
- UC "Xem danh sách Session" có các extension points: Xóa Session, Tạo Session, Xem chi tiết Session
- UC "Xem chi tiết Session" có extension point: Chỉnh sửa Session

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Đăng nhập** | UC này cho phép User xác thực danh tính để truy cập hệ thống. |
| **Xem danh sách Session** | UC này cho phép User xem danh sách tất cả các Session đã tạo. |
| **Tạo Session** | UC này cho phép User tạo một Session mới với tên và mô tả. |
| **Xóa Session** | UC này cho phép User xóa một Session khỏi hệ thống. |
| **Xem chi tiết Session** | UC này cho phép User xem thông tin chi tiết của một Session cụ thể. |
| **Chỉnh sửa Session** | UC này cho phép User cập nhật thông tin của một Session hiện có (tên, mô tả). |
