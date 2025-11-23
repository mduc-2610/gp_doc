# Scenario - Quản lý Question và Flashcard

## Use Case: Tạo Question mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo Question mới |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - Question mới được tạo trong hệ thống<br>- Question được thêm vào danh sách |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click vào tab "Chỉnh sửa"<br>2. Hệ thống hiển thị giao diện của tab "Chỉnh sửa" <br>3. User click vào tab con "Câu hỏi" <br>4. Hệ thống gửi yêu cầu lấy danh sách Question đến backend<br>5. Backend trả về danh sách Question<br>6. Hệ thống hiển thị danh sách Question hiện có<br>7. User click "Thêm câu hỏi"<br>8. Hệ thống thêm một item mới vào danh sách ở chế độ chỉnh sửa<br>9. User nhập các thông tin và click "Lưu":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Nội dung<br>&nbsp;&nbsp;&nbsp;&nbsp;- Loại<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tùy chọn trả lời<br>&nbsp;&nbsp;&nbsp;&nbsp;- Đáp án đúng<br>&nbsp;&nbsp;&nbsp;&nbsp;- Mức độ nhận thức<br>&nbsp;&nbsp;&nbsp;&nbsp;- Giải thích<br>10. Hệ thống gửi yêu cầu tạo Question đến backend<br>11. Backend tạo Question và trả về Question mới<br>12. Hệ thống cập nhật item trong danh sách |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo Question: hiển thị thông báo "Không thể tạo câu hỏi" |

## Use Case: Chỉnh sửa Question

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa Question |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - Thông tin Question được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click vào tab "Chỉnh sửa"<br>2. Hệ thống hiển thị giao diện của tab "Chỉnh sửa" <br>3. User click vào tab con "Câu hỏi" <br>4. Hệ thống gửi yêu cầu lấy danh sách Question đến backend<br>5. Backend trả về danh sách Question<br>6. Hệ thống hiển thị danh sách Question hiện có<br>7. User click icon chỉnh sửa trên một Question<br>8. Item Question chuyển sang chế độ chỉnh sửa<br>9. User cập nhật các thông tin và click "Lưu":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Nội dung<br>&nbsp;&nbsp;&nbsp;&nbsp;- Loại<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tùy chọn trả lời<br>&nbsp;&nbsp;&nbsp;&nbsp;- Đáp án đúng<br>&nbsp;&nbsp;&nbsp;&nbsp;- Mức độ nhận thức<br>&nbsp;&nbsp;&nbsp;&nbsp;- Giải thích<br>10. Hệ thống gửi yêu cầu cập nhật đến backend<br>11. Backend cập nhật Question trong cơ sở dữ liệu và trả về Question mới<br>12. Hệ thống cập nhật dữ liệu trong danh sách với Question mới và hiển thị thông báo "Cập nhật câu hỏi thành công" |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Question không tồn tại: hiển thị thông báo "Không tìm thấy câu hỏi"<br>- Lỗi cập nhật: hiển thị thông báo "Không thể cập nhật câu hỏi" |

---

## Use Case: Xóa Question

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Question |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - Question được xóa khỏi hệ thống<br>- Danh sách Question được cập nhật |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click vào tab "Chỉnh sửa"<br>2. Hệ thống hiển thị giao diện của tab "Chỉnh sửa" <br>3. User click vào tab con "Câu hỏi" <br>4. Hệ thống gửi yêu cầu lấy danh sách Question đến backend<br>5. Backend trả về danh sách Question<br>6. Hệ thống hiển thị danh sách Question hiện có<br>7. User click icon xóa trên Question<br>8. Hệ thống yêu cầu xác nhận xóa<br>9. User xác nhận muốn xóa<br>10. Hệ thống gửi yêu cầu xóa đến backend<br>11. Backend xóa Question khỏi cơ sở dữ liệu và trả về trạng thái thành công<br>12. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo "Đã xóa câu hỏi thành công" |
| **Ngoại lệ** | - Question không tồn tại: hiển thị thông báo "Không tìm thấy câu hỏi"<br>- Lỗi xóa: hiển thị thông báo "Không thể xóa câu hỏi" |

---

## Use Case: Luyện tập với Question

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Luyện tập với Question |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết<br>- Session có ít nhất một Question |
| **Hậu điều kiện** | - User hoàn thành bài luyện tập<br>- Kết quả luyện tập được hiển thị |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click vào tab "Câu hỏi"<br>2. Hệ thống load và hiển thị Question đầu tiên<br>3. User trả lời Question và click "Tiếp"<br>4. Hệ thống chuyển đến Question tiếp theo<br>5. User tiếp tục trả lời cho đến Question cuối cùng sau đó click "Hoàn thành"<br>6. Hệ thống tính điểm và hiển thị kết quả|
| **Ngoại lệ** | - Không có Question: hiển thị thông báo "Chưa có câu hỏi"<br>- Lỗi load Question: hiển thị thông báo "Đang tải câu hỏi..." |

---

## Use Case: Tạo Flashcard mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo Flashcard mới |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - Flashcard mới được tạo trong hệ thống<br>- Flashcard được thêm vào danh sách |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click vào tab "Edit"<br>2. Hệ thống hiển thị giao diện của tab "Edit" <br>3. User click vào tab con "Flashcard" <br>4. Hệ thống gửi yêu cầu lấy danh sách Flashcard đến backend<br>5. Backend trả về danh sách Flashcard<br>6. Hệ thống hiển thị danh sách Flashcard hiện có<br>7. User click "Thêm Flashcard mới"<br>8. Hệ thống thêm một item mới vào danh sách ở chế độ chỉnh sửa<br>9. User nhập các thông tin và click "Lưu":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Nội dung<br>&nbsp;&nbsp;&nbsp;&nbsp;- Đáp án<br>&nbsp;&nbsp;&nbsp;&nbsp;- Loại<br>&nbsp;&nbsp;&nbsp;&nbsp;- Giải thích <br>10. Hệ thống gửi yêu cầu tạo Flashcard đến backend<br>11. Backend tạo Flashcard và trả về Flashcard mới<br>12. Hệ thống cập nhật item trong danh sách và hiển thị thông báo "Tạo Flashcard thành công" |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo Flashcard: hiển thị thông báo lỗi từ server |

---

## Use Case: Chỉnh sửa Flashcard

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa Flashcard |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - Thông tin Flashcard được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click vào tab "Edit"<br>2. Hệ thống hiển thị giao diện của tab "Edit" <br>3. User click vào tab con "Flashcard" <br>4. Hệ thống gửi yêu cầu lấy danh sách Flashcard đến backend<br>5. Backend trả về danh sách Flashcard<br>6. Hệ thống hiển thị danh sách Flashcard hiện có<br>7. User click icon chỉnh sửa trên Flashcard<br>8. Item Flashcard chuyển sang chế độ chỉnh sửa<br>9. User cập nhật các thông tin và click "Lưu":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Nội dung<br>&nbsp;&nbsp;&nbsp;&nbsp;- Đáp án<br>&nbsp;&nbsp;&nbsp;&nbsp;- Loại<br>&nbsp;&nbsp;&nbsp;&nbsp;- Giải thích<br>10. Hệ thống gửi yêu cầu cập nhật đến backend<br>11. Backend cập nhật Flashcard trong cơ sở dữ liệu và trả về Flashcard mới<br>12. Hệ thống cập nhật dữ liệu trong danh sách với Flashcard mới và hiển thị thông báo "Cập nhật Flashcard thành công" |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Flashcard không tồn tại: hiển thị thông báo lỗi<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server |

---

## Use Case: Xóa Flashcard

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Flashcard |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - Flashcard được xóa khỏi hệ thống<br>- Danh sách Flashcard được cập nhật |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click vào tab "Edit"<br>2. Hệ thống hiển thị giao diện của tab "Edit" <br>3. User click vào tab con "Flashcard" <br>4. Hệ thống gửi yêu cầu lấy danh sách Flashcard đến backend<br>5. Backend trả về danh sách Flashcard<br>6. Hệ thống hiển thị danh sách Flashcard hiện có<br>7. User click icon xóa trên Flashcard<br>8. Hệ thống yêu cầu xác nhận xóa<br>9. User xác nhận muốn xóa<br>10. Hệ thống gửi yêu cầu xóa đến backend<br>11. Backend xóa Flashcard khỏi cơ sở dữ liệu và trả về trạng thái thành công<br>12. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo "Xóa Flashcard thành công" |
| **Ngoại lệ** | - Flashcard không tồn tại: hiển thị thông báo lỗi<br>- Lỗi xóa: hiển thị thông báo lỗi từ server |

---

## Use Case: Luyện tập với Flashcard

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Luyện tập với Flashcard |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết<br>- Session có ít nhất một Flashcard |
| **Hậu điều kiện** | - User hoàn thành luyện tập với Flashcard |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click vào tab "Flashcard"<br>2. Hệ thống load và hiển thị Flashcard đầu tiên<br>3. User xem nội dung câu hỏi trên Flashcard và click vào Flashcard để xem đáp án<br>4. Hệ thống hiển thị đáp án của Flashcard<br>5. User click icon mũi tên trái phải để chuyển sang Flashcard trước và sau<br>6. Hệ thống chuyển đến Flashcard trước và sau<br>|
| **Ngoại lệ** | - Không có Flashcard: hiển thị thông báo trống<br>- Lỗi load Flashcard: hiển thị thông báo lỗi |
