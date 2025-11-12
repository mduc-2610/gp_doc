# Use Case Description - Quản lý Model Config và Model Wrapper

Chức năng quản lý Model Config và Model Wrapper (Model Config & Model Wrapper Management) có các giao diện tương tác với User:
- **Xem danh sách Model Config** → đề xuất UC xem danh sách Model Config
- **Xem danh sách Model Wrapper** → đề xuất UC xem danh sách Model Wrapper
- **Chỉnh sửa Model Config** → đề xuất UC chỉnh sửa Model Config
- **Tạo Model Config mới** → đề xuất UC tạo Model Config mới
- **Xóa Model Config** → đề xuất UC xóa Model Config
- **Toggle trạng thái sử dụng Model Config** → đề xuất UC toggle trạng thái Model Config
- **Chỉnh sửa Model Wrapper** → đề xuất UC chỉnh sửa Model Wrapper (Admin only)
- **Tạo Model Wrapper mới** → đề xuất UC tạo Model Wrapper mới (Admin only)
- **Xóa Model Wrapper** → đề xuất UC xóa Model Wrapper (Admin only)
- **Preload Model Config** → đề xuất UC preload Model Config

**Quan hệ giữa các Use Case:**
Truy cập trang cấu hình Model là bắt buộc để sử dụng các chức năng quản lý Model Config và Wrapper. Do đó:
- UC "Xem danh sách Model Config", "Xem danh sách Model Wrapper" include UC "Truy cập trang cấu hình Model"
- UC "Chỉnh sửa Model Config", "Tạo Model Config mới", "Xóa Model Config", "Toggle trạng thái Model Config", "Preload Model Config" include UC "Xem danh sách Model Config"
- UC "Chỉnh sửa Model Wrapper", "Tạo Model Wrapper mới", "Xóa Model Wrapper" include UC "Xem danh sách Model Wrapper" và require role Admin

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Xem danh sách Model Config** | UC này cho phép User xem tất cả các Model Config của mình được tổ chức theo task type. |
| **Xem danh sách Model Wrapper** | UC này cho phép User xem tất cả các Model Wrapper có sẵn trong hệ thống. |
| **Chỉnh sửa Model Config** | UC này cho phép User cập nhật thông tin của một Model Config hiện có. Trong quá trình chỉnh sửa, User có thể chọn Wrapper từ danh sách hiển thị. |
| **Tạo Model Config mới** | UC này cho phép User tạo một Model Config mới. User cần chọn Wrapper từ danh sách hiển thị trước khi tạo. |
| **Xóa Model Config** | UC này cho phép User xóa một Model Config khỏi hệ thống. |
| **Toggle trạng thái sử dụng Model Config** | UC này cho phép User bật/tắt trạng thái sử dụng của một Model Config cho task type cụ thể. |
| **Chỉnh sửa Model Wrapper** | UC này cho phép Admin cập nhật thông tin của một Model Wrapper hiện có. |
| **Tạo Model Wrapper mới** | UC này cho phép Admin tạo một Model Wrapper mới cho hệ thống. |
| **Xóa Model Wrapper** | UC này cho phép Admin xóa một Model Wrapper khỏi hệ thống. |
| **Preload Model Config** | UC này cho phép User tải trước các Model Config mặc định vào hệ thống. |
