# Scenario - Generation với Batch

## Use Case: Đăng nhập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Đăng nhập |
| **Actor** | User |
| **Tiền điều kiện** | - User chưa đăng nhập |
| **Hậu điều kiện** | - User đăng nhập thành công và có quyền truy cập hệ thống |
| **Kịch bản chính** | 1. User truy cập trang đăng nhập<br>2. Hệ thống hiển thị form đăng nhập<br>3. User nhập thông tin đăng nhập<br>4. Hệ thống xác thực và chuyển đến trang chính |

---

## Use Case: Xem danh sách Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Đăng nhập" đã hoàn thành |
| **Hậu điều kiện** | - Danh sách Session được hiển thị |
| **Kịch bản chính** | 1. User truy cập trang danh sách Session<br>2. Hệ thống hiển thị danh sách các Session của User |

---

## Use Case: Xem chi tiết Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem chi tiết Session |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Session" đã hoàn thành |
| **Hậu điều kiện** | - Chi tiết Session được hiển thị |
| **Kịch bản chính** | 1. User chọn một Session từ danh sách<br>2. Hệ thống hiển thị chi tiết Session |

---

## Use Case: Tạo yêu cầu Generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo yêu cầu Generation |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành<br>- Session có ít nhất một Document |
| **Hậu điều kiện** | - Yêu cầu generation được tạo<br>- Tiến trình generation bắt đầu |
| **Kịch bản chính** | 1. User click icon sparkles tại sidebar<br>2. Hệ thống hiển thị dialog tạo generation<br>3. User chọn tham số generation và click "Tạo"<br>4. Hệ thống gửi yêu cầu và hiển thị progress bar |

---

## Use Case: Theo dõi tiến trình Generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Theo dõi tiến trình Generation |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Tạo yêu cầu Generation" đã hoàn thành |
| **Hậu điều kiện** | - User nhìn thấy tiến trình generation real-time |
| **Kịch bản chính** | 1. Hệ thống kết nối WebSocket<br>2. Hệ thống hiển thị LiveProgress với các step<br>3. User theo dõi tiến trình qua progress bar |

---

## Use Case: Hủy tiến trình Generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Hủy tiến trình Generation |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Tạo yêu cầu Generation" đã hoàn thành<br>- Tiến trình generation đang chạy |
| **Hậu điều kiện** | - Tiến trình generation bị hủy |
| **Kịch bản chính** | 1. User click "Hủy tiến trình"<br>2. Hệ thống gửi yêu cầu cancel<br>3. Backend hủy tiến trình và cập nhật trạng thái |

---

## Use Case: Xem kết quả Batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem kết quả Batch |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành<br>- Có batch đang pending |
| **Hậu điều kiện** | - Kết quả batch được hiển thị |
| **Kịch bản chính** | 1. User click tab "Câu hỏi" hoặc "Flashcard"<br>2. Hệ thống load và hiển thị batch items |

---

## Use Case: Duyệt kết quả Batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Duyệt kết quả Batch |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem kết quả Batch" đã hoàn thành |
| **Hậu điều kiện** | - Batch items được approve và chuyển thành Question/Flashcard |
| **Kịch bản chính** | 1. User chọn items và click "Phê duyệt"<br>2. Hệ thống gửi yêu cầu approve<br>3. Backend chuyển batch items thành Question/Flashcard |

---

## Use Case: Từ chối kết quả Batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Từ chối kết quả Batch |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem kết quả Batch" đã hoàn thành |
| **Hậu điều kiện** | - Batch items bị reject với feedback |
| **Kịch bản chính** | 1. User chọn items và click "Từ chối"<br>2. User nhập feedback và xác nhận<br>3. Backend lưu feedback và cập nhật trạng thái |

---

## Use Case: Tạo yêu cầu regenerate

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo yêu cầu regenerate |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem kết quả Batch" đã hoàn thành<br>- Có items đã bị reject |
| **Hậu điều kiện** | - Yêu cầu regenerate được tạo |
| **Kịch bản chính** | 1. User click "Tạo lại"<br>2. Hệ thống gửi yêu cầu regenerate với feedback<br>3. Backend tạo generation process mới |
