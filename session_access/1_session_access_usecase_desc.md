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
Đăng nhập và xem danh sách Session là bắt buộc để có thể xem chi tiết Session và sử dụng các chức năng quản lý truy cập. Do đó:
- UC "Xem danh sách Session" include UC "Đăng nhập"
- UC "Xem danh sách Session" có extension point: Xem chi tiết Session
- UC "Xem chi tiết Session" có các extension points chính:
  - Cập nhật quyền realtime (tự động thông qua WebSocket)
  - Xem danh sách yêu cầu truy cập
  - Xem các thông tin về quyền truy cập
- UC "Xem danh sách yêu cầu truy cập" có các extension points: Chấp nhận yêu cầu truy cập, Từ chối yêu cầu truy cập
- UC "Xem các thông tin về quyền truy cập" có các extension points: Chia sẻ Session với User, Cập nhật quyền truy cập, Thu hồi quyền truy cập, Quản lý liên kết chia sẻ
- UC "Yêu cầu truy cập", "Hủy yêu cầu" có thể thực hiện khi **chưa có quyền truy cập** hoặc quyền bị hạn chế
- UC "Nhận cập nhật quyền" hoạt động thông qua WebSocket connection tự động khi User vào Session

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Xem danh sách yêu cầu truy cập** | UC này cho phép User xem tất cả các yêu cầu truy cập đang chờ xử lý cho Session. |
| **Chấp nhận yêu cầu truy cập** | UC này cho phép User chấp nhận yêu cầu truy cập từ User khác. |
| **Từ chối yêu cầu truy cập** | UC này cho phép User từ chối yêu cầu truy cập từ User khác. |
| **Xem các thông tin về quyền truy cập** | UC này cho phép User xem chi tiết về các quyền truy cập hiện tại của Session. |
| **Chia sẻ Session** | UC này cho phép User thêm User cụ thể vào Session với mức quyền Reader, Contributor hoặc Manager. |
| **Cập nhật quyền truy cập** | UC này cho phép User thay đổi mức quyền của User đã được chia sẻ. |
| **Thu hồi quyền truy cập** | UC này cho phép User xóa quyền truy cập của User khỏi Session. |
| **Quản lý liên kết chia sẻ** | UC này cho phép User bật/tắt liên kết chia sẻ công khai và cấu hình mức quyền mặc định. |
| **Yêu cầu truy cập** | UC này cho phép User gửi yêu cầu để được cấp quyền truy cập vào Session. |
| **Hủy yêu cầu truy cập** | UC này cho phép User hủy yêu cầu truy cập đang chờ xử lý. |
| **Nhận cập nhật quyền** | UC này tự động thông báo cho User khi quyền truy cập của họ thay đổi qua WebSocket. |
