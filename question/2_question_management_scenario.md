# Scenario - Quản lý Question và Flashcard

## Use Case: Xem danh sách Question

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Question |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Danh sách các Question trong Session được hiển thị |
| **Kịch bản chính** | 1. User click tab "Chỉnh sửa"<br>2. User click tab con "Câu hỏi"<br>3. Hệ thống gửi yêu cầu lấy danh sách Question đến backend<br>4. Backend trả về danh sách Question<br>5. Hệ thống hiển thị danh sách Question |
| **Ngoại lệ** | - Không có Question: hiển thị thông báo "Chưa có câu hỏi"<br>- Lỗi load: hiển thị thông báo "Không thể tải câu hỏi" |

---

## Use Case: Tạo Question mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo Question mới |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Question" đã hoàn thành |
| **Hậu điều kiện** | - Question mới được tạo trong hệ thống<br>- Danh sách Question được cập nhật |
| **Kịch bản chính** | 1. User click "Thêm câu hỏi"<br>2. Hệ thống thêm item mới ở chế độ chỉnh sửa<br>3. User nhập thông tin và click "Lưu"<br>4. Hệ thống gửi yêu cầu tạo Question đến backend<br>5. Backend tạo Question và trả về Question mới<br>6. Hệ thống cập nhật item trong danh sách |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo: hiển thị thông báo "Không thể tạo câu hỏi" |

---

## Use Case: Chỉnh sửa Question

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa Question |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Question" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin Question được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. User click icon chỉnh sửa trên Question<br>2. Item Question chuyển sang chế độ chỉnh sửa<br>3. User cập nhật thông tin và click "Lưu"<br>4. Hệ thống gửi yêu cầu cập nhật đến backend<br>5. Backend cập nhật Question và trả về Question mới<br>6. Hệ thống cập nhật item trong danh sách và hiển thị thông báo thành công |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Question không tồn tại: hiển thị thông báo "Không tìm thấy câu hỏi"<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật câu hỏi" |

---

## Use Case: Xóa Question

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Question |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Question" đã hoàn thành |
| **Hậu điều kiện** | - Question được xóa khỏi hệ thống<br>- Danh sách Question được cập nhật |
| **Kịch bản chính** | 1. User click icon xóa trên Question<br>2. Hệ thống yêu cầu xác nhận xóa<br>3. User xác nhận muốn xóa<br>4. Hệ thống gửi yêu cầu xóa đến backend<br>5. Backend xóa Question và trả về trạng thái thành công<br>6. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo thành công |
| **Ngoại lệ** | - Question không tồn tại: hiển thị thông báo "Không tìm thấy câu hỏi"<br>- Lỗi xóa: hiển thị thông báo "Không thể xóa câu hỏi" |

---

## Use Case: Luyện tập với Question

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Luyện tập với Question |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành<br>- Session có ít nhất một Question |
| **Hậu điều kiện** | - User hoàn thành bài luyện tập<br>- Kết quả luyện tập được hiển thị |
| **Kịch bản chính** | 1. User click tab "Câu hỏi"<br>2. Hệ thống load và hiển thị Question đầu tiên<br>3. User trả lời Question và điều hướng bằng phím mũi tên hoặc click "Enter"<br>4. Hệ thống chuyển đến Question tiếp theo<br>5. User hoàn thành luyện tập và click "Hoàn thành" hoặc phím "Enter"<br>6. Hệ thống tính điểm và hiển thị kết quả |
| **Ngoại lệ** | - Không có Question: hiển thị thông báo "Chưa có câu hỏi"<br>- Lỗi load: hiển thị thông báo "Đang tải câu hỏi..." |

---

## Use Case: Xem danh sách Flashcard

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Flashcard |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành |
| **Hậu điều kiện** | - Danh sách các Flashcard trong Session được hiển thị |
| **Kịch bản chính** | 1. User click tab "Chỉnh sửa"<br>2. User click tab con "Thẻ ghi nhớ"<br>3. Hệ thống gửi yêu cầu lấy danh sách Flashcard đến backend<br>4. Backend trả về danh sách Flashcard<br>5. Hệ thống hiển thị danh sách Flashcard |
| **Ngoại lệ** | - Không có Flashcard: hiển thị thông báo "Chưa có thẻ ghi nhớ"<br>- Lỗi load: hiển thị thông báo "Không thể tải thẻ ghi nhớ" |

---

## Use Case: Tạo Flashcard mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo Flashcard mới |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Flashcard" đã hoàn thành |
| **Hậu điều kiện** | - Flashcard mới được tạo trong hệ thống<br>- Danh sách Flashcard được cập nhật |
| **Kịch bản chính** | 1. User click "Thêm thẻ"<br>2. Hệ thống thêm item mới ở chế độ chỉnh sửa<br>3. User nhập thông tin và click "Lưu"<br>4. Hệ thống gửi yêu cầu tạo Flashcard đến backend<br>5. Backend tạo Flashcard và trả về Flashcard mới<br>6. Hệ thống cập nhật item trong danh sách và hiển thị thông báo thành công |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo: hiển thị thông báo lỗi |

---

## Use Case: Chỉnh sửa Flashcard

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa Flashcard |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Flashcard" đã hoàn thành |
| **Hậu điều kiện** | - Thông tin Flashcard được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. User click icon chỉnh sửa trên Flashcard<br>2. Item Flashcard chuyển sang chế độ chỉnh sửa<br>3. User cập nhật thông tin và click "Lưu"<br>4. Hệ thống gửi yêu cầu cập nhật đến backend<br>5. Backend cập nhật Flashcard và trả về Flashcard mới<br>6. Hệ thống cập nhật item trong danh sách và hiển thị thông báo thành công |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Flashcard không tồn tại: hiển thị thông báo lỗi<br>- Lỗi cập nhật: hiển thị thông báo lỗi |

---

## Use Case: Xóa Flashcard

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Flashcard |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem danh sách Flashcard" đã hoàn thành |
| **Hậu điều kiện** | - Flashcard được xóa khỏi hệ thống<br>- Danh sách Flashcard được cập nhật |
| **Kịch bản chính** | 1. User click icon xóa trên Flashcard<br>2. Hệ thống yêu cầu xác nhận xóa<br>3. User xác nhận muốn xóa<br>4. Hệ thống gửi yêu cầu xóa đến backend<br>5. Backend xóa Flashcard và trả về trạng thái thành công<br>6. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo thành công |
| **Ngoại lệ** | - Flashcard không tồn tại: hiển thị thông báo lỗi<br>- Lỗi xóa: hiển thị thông báo lỗi |

---

## Use Case: Luyện tập với Flashcard

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Luyện tập với Flashcard |
| **Actor** | User |
| **Tiền điều kiện** | - UC "Xem chi tiết Session" đã hoàn thành<br>- Session có ít nhất một Flashcard |
| **Hậu điều kiện** | - User hoàn thành luyện tập với Flashcard |
| **Kịch bản chính** | 1. User click tab "Thẻ ghi nhớ"<br>2. Hệ thống load và hiển thị Flashcard đầu tiên<br>3. User xem câu hỏi và click Flashcard để xem đáp án<br>4. Hệ thống hiển thị đáp án<br>5. User click icon mũi tên hoặc dùng phím mũi tên để điều hướng<br>6. Hệ thống chuyển đến Flashcard trước/sau |
| **Ngoại lệ** | - Không có Flashcard: hiển thị thông báo "Chưa có thẻ ghi nhớ"<br>- Lỗi load: hiển thị thông báo "Đang tải thẻ ghi nhớ..." |
