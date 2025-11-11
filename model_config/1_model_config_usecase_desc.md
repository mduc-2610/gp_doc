# Use Case Description - Cấu hình Model

Chức năng cấu hình Model (Model Configuration) có các giao diện tương tác với User:
- **Xem danh sách Model** → đề xuất UC xem danh sách Model
- **Tạo Model mới** → đề xuất UC tạo Model mới
- **Chỉnh sửa Model** → đề xuất UC chỉnh sửa Model
- **Xóa Model** → đề xuất UC xóa Model
- **Xem danh sách Model Wrapper** → đề xuất UC xem danh sách Model Wrapper (User và Admin)
- **Tạo Model Wrapper mới** → đề xuất UC tạo Model Wrapper mới (Admin)
- **Chỉnh sửa Model Wrapper** → đề xuất UC chỉnh sửa Model Wrapper (Admin)
- **Xóa Model Wrapper** → đề xuất UC xóa Model Wrapper (Admin)

**Quan hệ giữa các Use Case:**
- UC "Xem danh sách Model" là chức năng cơ bản để User truy cập
- UC "Tạo Model mới", "Chỉnh sửa Model", "Xóa Model" đều phụ thuộc vào UC "Xem danh sách Model"
- UC "Xem danh sách Model Wrapper" là chức năng mà cả User và Admin đều có thể truy cập
- UC "Tạo Model Wrapper mới", "Chỉnh sửa Model Wrapper", "Xóa Model Wrapper" đều phụ thuộc vào UC "Xem danh sách Model Wrapper" và chỉ dành cho Admin (User có role ADMIN)

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Xem danh sách Model** | UC này cho phép User xem danh sách tất cả các Model đã cấu hình cho mình, được phân loại theo loại tác vụ (Generation, Embedding, Parse Doc, Parse Audio, Parse Image). |
| **Tạo Model mới** | UC này cho phép User tạo một Model mới bằng cách chọn Model Wrapper. |
| **Chỉnh sửa Model** | UC này cho phép User cập nhật thông tin của một Model hiện có như tên, extra config. |
| **Xóa Model** | UC này cho phép User xóa một Model đã cấu hình. |
| **Xem danh sách Model Wrapper** | UC này cho phép User và Admin xem danh sách tất cả các Model Wrapper có sẵn trong hệ thống. |
| **Tạo Model Wrapper mới** | UC này cho phép Admin tạo một Model Wrapper mới định nghĩa cách tương tác với một mô hình AI cụ thể. |
| **Chỉnh sửa Model Wrapper** | UC này cho phép Admin cập nhật thông tin của một Model Wrapper như tên, task type, instruction. |
| **Xóa Model Wrapper** | UC này cho phép Admin xóa một Model Wrapper khỏi hệ thống. |
