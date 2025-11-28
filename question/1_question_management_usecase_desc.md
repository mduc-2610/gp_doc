# Use Case Description - Quản lý Question và Flashcard

Chức năng quản lý Question và flashcard (Question & Flashcard Management) có các giao diện tương tác với User:
- **Đăng nhập** → đề xuất UC đăng nhập
- **Xem danh sách Session** → đề xuất UC xem danh sách Session
- **Xem chi tiết Session** → đề xuất UC xem chi tiết Session
- **Xem danh sách Question** → đề xuất UC xem danh sách Question
- **Xem danh sách Flashcard** → đề xuất UC xem danh sách Flashcard
- **Thêm Question** → đề xuất UC thêm Question
- **Chỉnh sửa Question** → đề xuất UC chỉnh sửa Question
- **Xóa Question** → đề xuất UC xóa Question
- **Thêm Flashcard** → đề xuất UC thêm Flashcard
- **Chỉnh sửa Flashcard** → đề xuất UC chỉnh sửa flashcard
- **Xóa Flashcard** → đề xuất UC xóa flashcard
- **Luyện tập với Question** → đề xuất UC luyện tập với Question
- **Luyện tập với Flashcard** → đề xuất UC luyện tập với Flashcard
- **Trích xuất Question** → đề xuất UC trích xuất Question
- **Trích xuất Flashcard** → đề xuất UC trích xuất Flashcard

**Quan hệ giữa các Use Case:**
Đăng nhập và xem danh sách Session là bắt buộc để có thể xem chi tiết Session và sử dụng các tab Question và flashcard. Do đó:
- UC "Xem danh sách Session" include UC "Đăng nhập"
- UC "Xem danh sách Session" có extension point: Xem chi tiết Session
- UC "Xem chi tiết Session" có các extension points: Xem danh sách Question, Xem danh sách Flashcard, Luyện tập với Question, Luyện tập với Flashcard, Trích xuất Question, Trích xuất Flashcard
- UC "Xem danh sách Question" có các extension points: Thêm Question, Chỉnh sửa Question, Xóa Question
- UC "Xem danh sách Flashcard" có các extension points: Thêm Flashcard, Chỉnh sửa Flashcard, Xóa Flashcard

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Đăng nhập** | UC này cho phép User xác thực danh tính để truy cập hệ thống. |
| **Xem danh sách Session** | UC này cho phép User xem danh sách tất cả các Session đã tạo. |
| **Xem chi tiết Session** | UC này cho phép User xem thông tin chi tiết của một Session và truy cập các chức năng quản lý Question và Flashcard. |
| **Xem danh sách Question** | UC này cho phép User xem danh sách tất cả các Question trong một Session. |
| **Xem danh sách Flashcard** | UC này cho phép User xem danh sách tất cả các Flashcard trong một Session. |
| **Thêm Question** | UC này cho phép User tạo một Question mới cho Session. |
| **Chỉnh sửa Question** | UC này cho phép User cập nhật thông tin của một Question hiện có. |
| **Xóa Question** | UC này cho phép User xóa một Question khỏi Session. |
| **Thêm Flashcard** | UC này cho phép User tạo một Flashcard mới cho Session. |
| **Chỉnh sửa Flashcard** | UC này cho phép User cập nhật thông tin của một Flashcard hiện có. |
| **Xóa Flashcard** | UC này cho phép User xóa một Flashcard khỏi Session. |
| **Luyện tập với Question** | UC này cho phép User luyện tập với các Question của một Session. |
| **Luyện tập với Flashcard** | UC này cho phép User luyện tập với các Flashcard của một Session. |
| **Trích xuất Question** | UC này cho phép User xuất danh sách Question ra file Excel theo category đã chọn. |
| **Trích xuất Flashcard** | UC này cho phép User xuất danh sách Flashcard ra file Excel. |

