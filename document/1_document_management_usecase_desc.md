# Use Case Description - Quản lý Document

Chức năng quản lý Document (Document Management) có các giao diện tương tác với User:
- **Upload Document** → đề xuất UC upload Document
- **Xem danh sách Document** → đề xuất UC xem danh sách Document
- **Xem thông tin Document** → đề xuất UC xem thông tin Document
- **Cập nhật Document** → đề xuất UC cập nhật Document
- **Xóa Document** → đề xuất UC xóa Document

**Quan hệ giữa các Use Case:**
Xem chi tiết Session là bắt buộc để sử dụng các chức năng quản lý Document. Do đó:
- UC "Upload Document", "Xem danh sách Document", "Xem thông tin Document", "Cập nhật Document", "Xóa Document" include UC "Xem chi tiết Session"

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Upload Document** | UC này cho phép User tải lên Document mới vào Session. User có thể upload file hoặc cung cấp URL. |
| **Xem danh sách Document** | UC này cho phép User xem danh sách tất cả các Document trong Session. |
| **Xem thông tin Document** | UC này cho phép User xem chi tiết thông tin của một Document cụ thể. |
| **Cập nhật Document** | UC này cho phép User cập nhật thông tin của một Document hiện có (cập nhật tên hiển thị). |
| **Xóa Document** | UC này cho phép User xóa một Document khỏi Session. |
