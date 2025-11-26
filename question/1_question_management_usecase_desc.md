# Use Case Description - Quản lý Question và Flashcard

Chức năng quản lý Question và flashcard (Question & Flashcard Management) có các giao diện tương tác với User:
- **Chỉnh sửa Question** → đề xuất UC chỉnh sửa Question
- **Tạo Question mới** → đề xuất UC tạo Question mới
- **Xóa Question** → đề xuất UC xóa Question
- **Chỉnh sửa Flashcard** → đề xuất UC chỉnh sửa flashcard
- **Tạo Flashcard mới** → đề xuất UC tạo flashcard mới
- **Xóa Flashcard** → đề xuất UC xóa flashcard
- **Luyện tập với Question** → đề xuất UC luyện tập với Question
- **Luyện tập với Flashcard** → đề xuất UC luyện tập với Flashcard

**Quan hệ giữa các Use Case:**
Đăng nhập và xem danh sách Session là bắt buộc để có thể xem chi tiết Session và sử dụng các tab Question và flashcard. Do đó:
- UC "Xem danh sách Session" include UC "Đăng nhập"
- UC "Xem danh sách Session" có extension point: Xem chi tiết Session
- UC "Xem chi tiết Session" có các extension points: Xem danh sách Question, Xem danh sách Flashcard, Luyện tập với Question, Luyện tập với Flashcard
- UC "Xem danh sách Question" có các extension points: Thêm Question, Chỉnh sửa Question, Xóa Question
- UC "Xem danh sách Flashcard" có các extension points: Thêm Flashcard, Chỉnh sửa Flashcard, Xóa Flashcard

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Chỉnh sửa Question** | UC này cho phép User cập nhật thông tin của một Question hiện có. |
| **Tạo Question mới** | UC này cho phép User tạo một Question mới cho Session. |
| **Xóa Question** | UC này cho phép User xóa một Question khỏi Session. |
| **Chỉnh sửa Flashcard** | UC này cho phép User cập nhật thông tin của một flashcard hiện có. |
| **Tạo Flashcard mới** | UC này cho phép User tạo một flashcard mới cho Session. |
| **Xóa Flashcard** | UC này cho phép User xóa một flashcard khỏi Session. |
| **Luyện tập với Question** | UC này cho phép User luyện tập với các Question của một Session. |
| **Luyện tập với Flashcard** | UC này cho phép User luyện tập với các flashcard của một Session. |

