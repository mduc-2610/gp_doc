# Use Case Description - Tổng quan hệ thống DLab

Hệ thống DLab (Digital Learning Assistant) cung cấp các chức năng quản lý và tạo nội dung học tập thông minh. Các chức năng chính bao gồm:

- **Đăng nhập** → đề xuất UC đăng nhập
- **Quản lý Session** → đề xuất UC quản lý Session
- **Quản lý Document** → đề xuất UC quản lý Document
- **Quản lý Question và Flashcard** → đề xuất UC quản lý Question và Flashcard
- **Generation** → đề xuất UC generation nội dung
- **Quản lý Model Config** → đề xuất UC quản lý Model Config
- **Quản lý Session Access** → đề xuất UC quản lý Session Access

**Quan hệ giữa các Use Case:**
Đăng nhập là điều kiện tiên quyết để sử dụng tất cả các chức năng khác. Do đó:
- UC "Quản lý Session", "Quản lý Document", "Quản lý Question và Flashcard", "Generation", "Quản lý Model Config", "Quản lý Session Access" đều include UC "Đăng nhập"

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Đăng nhập** | UC này cho phép User xác thực danh tính để truy cập hệ thống DLab. |
| **Quản lý Session** | UC này cho phép User tạo, xem, chỉnh sửa và xóa các Session học tập. |
| **Quản lý Document** | UC này cho phép User upload, xem và quản lý các tài liệu trong Session. |
| **Quản lý Question và Flashcard** | UC này cho phép User tạo và quản lý câu hỏi và flashcard trong Session. |
| **Generation** | UC này cho phép User tạo nội dung học tập tự động từ Document. |
| **Quản lý Model Config** | UC này cho phép User cấu hình các model AI sử dụng trong hệ thống. |
| **Quản lý Session Access** | UC này cho phép User chia sẻ và quản lý quyền truy cập Session. |