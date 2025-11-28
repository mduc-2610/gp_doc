# Scenario - Generation với Batch

## Use Case: Tạo yêu cầu Generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo yêu cầu Generation |
| **Actor** | User |
| **Tiền điều kiện** | - User đang xem chi tiết Session<br>- Session có ít nhất một Document |
| **Hậu điều kiện** | - Yêu cầu generation được tạo<br>- Tiến trình generation bắt đầu |
| **Kịch bản chính** | 1. User click icon sparkles tại sidebar<br>2. Hệ thống hiển thị dialog tạo generation<br>3. User chọn tham số generation và click "Tạo"<br>4. Hệ thống gửi yêu cầu và hiển thị progress bar |
| **Ngoại lệ** | - Số lượng không hợp lệ: hiển thị lỗi validation "Số lượng phải từ 1 đến 30"<br>- Topic quá dài: hiển thị lỗi validation "Chủ đề không được quá 100 ký tự"<br>- Chưa chọn document: hiển thị thông báo "Vui lòng chọn ít nhất một tài liệu"<br>- Process đã tồn tại: hiển thị thông báo "Đã có tiến trình generation đang chạy"<br>- Model config không tìm thấy: hiển thị thông báo "Không tìm thấy cấu hình model"<br>- Lỗi tạo process: hiển thị thông báo "Không thể tạo yêu cầu generation" |

---

## Use Case: Theo dõi tiến trình Generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Theo dõi tiến trình Generation |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Tạo yêu cầu Generation" đã hoàn thành |
| **Hậu điều kiện** | - User nhìn thấy tiến trình generation real-time |
| **Kịch bản chính** | 1. Hệ thống kết nối WebSocket<br>2. Hệ thống hiển thị LiveProgress với các step<br>3. User theo dõi tiến trình qua progress bar |
| **Ngoại lệ** | - Mất kết nối WebSocket: tự động reconnect hoặc hiển thị thông báo "Mất kết nối, đang thử kết nối lại"<br>- Process không tồn tại: hiển thị thông báo "Không tìm thấy tiến trình generation"<br>- Generation bị lỗi: hiển thị thông báo lỗi từ backend với chi tiết bước thất bại<br>- Rate limit exceeded: hiển thị thông báo "Vượt quá giới hạn request, vui lòng thử lại sau"<br>- Context không tìm thấy: hiển thị thông báo "Không tìm thấy nội dung liên quan"<br>- Generation trả về rỗng: hiển thị thông báo "Model không thể tạo nội dung, vui lòng thử lại" |

---

## Use Case: Hủy tiến trình Generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Hủy tiến trình Generation |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Tạo yêu cầu Generation" đã hoàn thành<br>- Tiến trình generation đang chạy |
| **Hậu điều kiện** | - Tiến trình generation bị hủy |
| **Kịch bản chính** | 1. User click "Hủy tiến trình"<br>2. Hệ thống gửi yêu cầu cancel<br>3. Backend hủy tiến trình và cập nhật trạng thái |
| **Ngoại lệ** | - Process không tồn tại: hiển thị thông báo "Không tìm thấy tiến trình generation"<br>- Process đã hoàn thành: hiển thị thông báo "Tiến trình đã hoàn thành, không thể hủy"<br>- Process đã bị hủy: hiển thị thông báo "Tiến trình đã bị hủy trước đó"<br>- Lỗi hủy process: hiển thị thông báo "Không thể hủy tiến trình generation" |

---

## Use Case: Xem kết quả Batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem kết quả Batch |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành<br>- Có batch đang pending |
| **Hậu điều kiện** | - Kết quả batch được hiển thị |
| **Kịch bản chính** | 1. User click tab "Câu hỏi" hoặc "Flashcard"<br>2. Hệ thống load và hiển thị batch items |
| **Ngoại lệ** | - Không có batch pending: hiển thị thông báo "Chưa có kết quả nào cần duyệt"<br>- Batch không tồn tại: hiển thị thông báo "Không tìm thấy batch"<br>- Lỗi load batch items: hiển thị thông báo "Không thể tải kết quả batch"<br>- Batch items rỗng: hiển thị thông báo "Batch không có item nào" |

---

## Use Case: Duyệt kết quả Batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Duyệt kết quả Batch |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem kết quả Batch" đã hoàn thành |
| **Hậu điều kiện** | - Batch items được approve và chuyển thành Question/Flashcard |
| **Kịch bản chính** | 1. User chọn items và click "Phê duyệt"<br>2. Hệ thống gửi yêu cầu approve<br>3. Backend chuyển batch items thành Question/Flashcard |
| **Ngoại lệ** | - Chưa chọn item: hiển thị thông báo "Vui lòng chọn ít nhất một item để phê duyệt"<br>- Batch không tồn tại: hiển thị thông báo "Không tìm thấy batch"<br>- Item không hợp lệ: hiển thị lỗi validation với chi tiết item bị lỗi<br>- Lỗi approve: hiển thị thông báo "Không thể phê duyệt batch items" |

---

## Use Case: Từ chối kết quả Batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Từ chối kết quả Batch |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem kết quả Batch" đã hoàn thành |
| **Hậu điều kiện** | - Batch items bị reject với feedback |
| **Kịch bản chính** | 1. User chọn items và click "Từ chối"<br>2. User nhập feedback và xác nhận<br>3. Backend lưu feedback và cập nhật trạng thái |
| **Ngoại lệ** | - Chưa chọn item: hiển thị thông báo "Vui lòng chọn ít nhất một item để từ chối"<br>- Feedback quá dài: hiển thị lỗi validation "Feedback không được quá 500 ký tự"<br>- Batch không tồn tại: hiển thị thông báo "Không tìm thấy batch"<br>- Item không hợp lệ: hiển thị lỗi validation với chi tiết item bị lỗi<br>- Lỗi reject: hiển thị thông báo "Không thể từ chối batch items" |

---

## Use Case: Tạo yêu cầu regenerate

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo yêu cầu regenerate |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem kết quả Batch" đã hoàn thành<br>- Có items đã bị reject |
| **Hậu điều kiện** | - Yêu cầu regenerate được tạo |
| **Kịch bản chính** | 1. User click "Tạo lại"<br>2. Hệ thống gửi yêu cầu regenerate với feedback<br>3. Backend tạo generation process mới |
| **Ngoại lệ** | - Batch không tồn tại: hiển thị thông báo "Không tìm thấy batch"<br>- Batch chưa có feedback: hiển thị thông báo "Vui lòng cung cấp feedback trước khi tạo lại"<br>- Process đã tồn tại: hiển thị thông báo "Đã có tiến trình regeneration đang chạy"<br>- Model config không tìm thấy: hiển thị thông báo "Không tìm thấy cấu hình model"<br>- Lỗi regenerate: hiển thị thông báo "Không thể tạo yêu cầu regenerate" |
