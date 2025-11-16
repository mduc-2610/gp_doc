# Use Case Description - Generation với Batch

Chức năng Generation với Batch (Generation & Batch Management) có các giao diện tương tác với User:
- **Tạo yêu cầu generation** → đề xuất UC tạo yêu cầu generation
- **Theo dõi tiến trình generation** → đề xuất UC theo dõi tiến trình generation
- **Hủy tiến trình generation** → đề xuất UC hủy tiến trình generation
- **Xem kết quả batch** → đề xuất UC xem kết quả batch
- **Duyệt kết quả batch** → đề xuất UC duyệt kết quả batch
- **Từ chối kết quả batch** → đề xuất UC từ chối kết quả batch
- **Tạo yêu cầu regenerate** → đề xuất UC tạo yêu cầu regenerate
- **Kết nối lại với tiến trình đang chạy** → đề xuất UC kết nối lại với tiến trình đang chạy

**Quan hệ giữa các Use Case:**
Truy cập chi tiết Session là bắt buộc để sử dụng các chức năng generation. Do đó:
- UC "Tạo yêu cầu generation", "Theo dõi tiến trình generation", "Hủy tiến trình generation" include UC "Truy cập chi tiết Session"
- UC "Xem kết quả batch", "Duyệt kết quả batch", "Từ chối kết quả batch", "Tạo yêu cầu regenerate" include UC "Truy cập chi tiết Session"
- UC "Kết nối lại với tiến trình đang chạy" include UC "Truy cập chi tiết Session"

**Mô tả chi tiết các Use Case:**

| **Use Case** | Mô tả |
|----------|------|
| **Tạo yêu cầu generation** | UC này cho phép User tạo yêu cầu generation Question và Flashcard từ các Document trong Session. |
| **Theo dõi tiến trình generation** | UC này cho phép User theo dõi tiến trình generation. |
| **Hủy tiến trình generation** | UC này cho phép User hủy bỏ tiến trình generation đang chạy. |
| **Xem kết quả batch** | UC này cho phép User xem kết quả batch đã được generate. |
| **Duyệt kết quả batch** | UC này cho phép User phê duyệt các item trong batch. |
| **Từ chối kết quả batch** | UC này cho phép User từ chối các item trong batch. |
| **Tạo yêu cầu regenerate** | UC này cho phép User yêu cầu generation lại các item đã bị reject. |
| **Kết nối lại với tiến trình đang chạy** | UC này cho phép User kết nối lại với tiến trình generation đang chạy khi reload trang hoặc quay lại Session. |
