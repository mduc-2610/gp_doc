# Use Case Description - Generation với Batch

Chức năng Generation với Batch (Generation & Batch Management) có các giao diện tương tác với User:
- **Đăng nhập** → đề xuất UC đăng nhập
- **Xem danh sách Session** → đề xuất UC xem danh sách Session
- **Xem chi tiết Session** → đề xuất UC xem chi tiết Session
- **Tạo yêu cầu Generation** → đề xuất UC tạo yêu cầu Generation
- **Theo dõi tiến trình Generation** → đề xuất UC theo dõi tiến trình Generation
- **Hủy tiến trình Generation** → đề xuất UC hủy tiến trình Generation
- **Xem kết quả Batch** → đề xuất UC xem kết quả Batch
- **Duyệt kết quả Batch** → đề xuất UC duyệt kết quả Batch
- **Từ chối kết quả Batch** → đề xuất UC từ chối kết quả Batch
- **Tạo yêu cầu regenerate** → đề xuất UC tạo yêu cầu regenerate

**Quan hệ giữa các Use Case:**
Đăng nhập và xem danh sách Session là bắt buộc để có thể xem chi tiết Session và sử dụng các chức năng liên quan Generation. Do đó:
- UC "Xem danh sách Session" include UC "Đăng nhập"
- UC "Xem danh sách Session" có extension point: Xem chi tiết Session
- UC "Xem chi tiết Session" có các extension points: Tạo yêu cầu Generation, Xem kết quả batch
- UC "Tạo yêu cầu Generation" include "Theo dõi tiến trình Generation" và có extension point: Hủy tiến trình Generation
- UC "Xem kết quả batch" có các extension points: Duyệt kết quả Batch, Từ chối kết quả Batch, Tạo yêu cầu regenerate

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Đăng nhập** | UC này cho phép User đăng nhập vào hệ thống. |
| **Xem danh sách Session** | UC này cho phép User xem danh sách các Session. |
| **Xem chi tiết Session** | UC này cho phép User xem chi tiết một Session. |
| **Tạo yêu cầu Generation** | UC này cho phép User tạo yêu cầu generation Question và Flashcard từ các Document trong Session. |
| **Theo dõi tiến trình Generation** | UC này cho phép User theo dõi tiến trình Generation. |
| **Hủy tiến trình Generation** | UC này cho phép User hủy bỏ tiến trình Generation đang chạy. |
| **Xem kết quả Batch** | UC này cho phép User xem kết quả Batch đã được generate. |
| **Duyệt kết quả Batch** | UC này cho phép User phê duyệt các item trong Batch. |
| **Từ chối kết quả Batch** | UC này cho phép User từ chối các item trong Batch. |
| **Tạo yêu cầu regenerate** | UC này cho phép User tạo lại yêu cầu Generation với yêu cầu khác. |
