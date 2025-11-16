```markdown
# Use Case Description - Quản lý Session Access

Chức năng quản lý Session Access (Session Access Management) có các giao diện tương tác với User:
- **Chia sẻ Session với User cụ thể** → đề xuất UC chia sẻ Session
- **Cập nhật quyền của User được chia sẻ** → đề xuất UC cập nhật quyền truy cập
- **Thu hồi quyền truy cập** → đề xuất UC thu hồi quyền
- **Yêu cầu quyền truy cập Session** → đề xuất UC yêu cầu truy cập
- **Chấp nhận/Từ chối yêu cầu truy cập** → đề xuất UC xử lý yêu cầu
- **Hủy yêu cầu truy cập** → đề xuất UC hủy yêu cầu
- **Bật/tắt liên kết chia sẻ công khai** → đề xuất UC quản lý liên kết chia sẻ
- **Nhận thông báo thời gian thực về thay đổi quyền** → đề xuất UC nhận cập nhật quyền

**Quan hệ giữa các Use Case:**
Truy cập chi tiết Session là bắt buộc để sử dụng các chức năng quản lý truy cập. Do đó:
- UC "Chia sẻ Session", "Cập nhật quyền", "Thu hồi quyền", "Xử lý yêu cầu", "Quản lý liên kết chia sẻ" include UC "Truy cập chi tiết Session" với quyền **Manager**
- UC "Yêu cầu truy cập", "Hủy yêu cầu" có thể thực hiện khi **chưa có quyền truy cập** hoặc quyền bị hạn chế
- UC "Nhận cập nhật quyền" hoạt động thông qua WebSocket connection tự động khi User vào Session

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Chia sẻ Session** | UC này cho phép Manager thêm User cụ thể vào Session với mức quyền Reader, Contributor hoặc Manager. |
| **Cập nhật quyền truy cập** | UC này cho phép Manager thay đổi mức quyền của User đã được chia sẻ. |
| **Thu hồi quyền truy cập** | UC này cho phép Manager xóa quyền truy cập của User khỏi Session. |
| **Yêu cầu truy cập** | UC này cho phép User gửi yêu cầu để được cấp quyền truy cập vào Session. |
| **Xử lý yêu cầu truy cập** | UC này cho phép Manager chấp nhận hoặc từ chối yêu cầu truy cập từ User. |
| **Hủy yêu cầu truy cập** | UC này cho phép User hủy yêu cầu truy cập đang chờ xử lý. |
| **Quản lý liên kết chia sẻ** | UC này cho phép Manager bật/tắt liên kết chia sẻ công khai và cấu hình mức quyền mặc định. |
| **Nhận cập nhật quyền** | UC này tự động thông báo cho User khi quyền truy cập của họ thay đổi qua WebSocket. |


```