# Use Case Description - Quản lý Document

Chức năng quản lý Document có các giao diện tương tác với User:
- **Đăng nhập** → đề xuất UC đăng nhập
- **Xem danh sách Session** → đề xuất UC xem danh sách Session
- **Xem chi tiết Session** → đề xuất UC xem chi tiết Session
- **Upload Document** → đề xuất UC upload Document
- **Xem danh sách Document** → đề xuất UC xem danh sách Document
- **Cập nhật Document** → đề xuất UC cập nhật Document
- **Xóa Document** → đề xuất UC xóa Document

**Quan hệ giữa các Use Case:**
Đăng nhập, xem danh sách Session là bắt buộc mới có thể thực hiện được các thao tác khác (xem chi tiết Session, upload, xem danh sách, cập nhật, xóa Document). Do đó:
- UC "Xem danh sách Session" include UC "Đăng nhập"
- UC "Xem danh sách Session" có extension point: Xem chi tiết Session
- UC "Xem chi tiết Session" có các extension points: Upload Document, Xem danh sách Document
- UC "Xem danh sách Document" có các extension points: Cập nhật Document, Xóa Document

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Đăng nhập** | UC này cho phép User xác thực danh tính để truy cập hệ thống. |
| **Xem danh sách Session** | UC này cho phép User xem danh sách tất cả các Session đã tạo. |
| **Xem chi tiết Session** | UC này cho phép User xem thông tin chi tiết của một Session và truy cập các chức năng quản lý Document. |
| **Upload Document** | UC này cho phép User tải lên các Document mới vào Session. |
| **Xem danh sách Document** | UC này cho phép User xem danh sách tất cả các Document trong một Session. |
| **Cập nhật Document** | UC này cho phép User cập nhật thông tin của một Document hiện có. |
| **Xóa Document** | UC này cho phép User xóa một Document khỏi Session. |
