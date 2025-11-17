# Scenario - Generation với Batch

## Use Case: Tạo yêu cầu generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo yêu cầu generation |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết<br>- Session có ít nhất một Document |
| **Hậu điều kiện** | - Yêu cầu generation được tạo trong hệ thống<br>- Tiến trình generation bắt đầu thực thi |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click icon sparkles tại sidebar<br>2. Hệ thống hiển thị dialog tạo generation<br>3. Hệ thống kiểm tra xem có tiến trình generation đang chạy không<br>4. Nếu không có tiến trình đang chạy, hiển thị form generation<br>5. User chọn các tham số generation:<br>&nbsp;&nbsp;&nbsp;&nbsp;- Document nguồn (có thể chọn nhiều)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Số lượng Question/Flashcard<br>&nbsp;&nbsp;&nbsp;&nbsp;- Topic (tùy chọn)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Cognitive levels (cho Question)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Question types<br>&nbsp;&nbsp;&nbsp;&nbsp;- Flashcard types<br>6. User click nút "Tạo"<br>7. Hệ thống gửi yêu cầu batch generate đến backend<br>8. Backend xử lý yêu cầu và trả về thông tin process<br>9. Hệ thống nhận response và tự động kết nối với tiến trình<br>10. Dialog đóng lại và hệ thống bắt đầu hiển thị progress bar trong tab tương ứng |
| **Ngoại lệ** | - Không có Document: hiển thị thông báo "Hãy chọn ít nhất một tài liệu."<br>- Có tiến trình đang chạy: hiển thị cảnh báo và không cho phép tạo mới<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo tiến trình: hiển thị thông báo "Tạo nội dung thất bại. Vui lòng thử lại." |

---

## Use Case: Theo dõi tiến trình generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Theo dõi tiến trình generation |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đã tạo yêu cầu generation hoặc có tiến trình đang chạy |
| **Hậu điều kiện** | - User nhìn thấy tiến trình generation real-time<br>- Kết quả được hiển thị khi hoàn thành |
| **Kịch bản chính** | 1. Sau khi tạo yêu cầu generation, hệ thống tự động thiết lập kết nối WebSocket với backend<br>2. Hệ thống gửi lệnh bắt đầu/tiếp tục generation<br>3. Backend khởi động pipeline generation và gửi snapshot ban đầu về danh sách các bước<br>4. Hệ thống nhận snapshot và hiển thị progress bar với danh sách bước (retrieve_context, generate_content, validate_results, save_batch)<br>5. Backend thực hiện từng bước trong pipeline và gửi cập nhật trạng thái real-time<br>6. Hệ thống nhận cập nhật và hiển thị trạng thái từng bước (PENDING, RUNNING, COMPLETED, FAILED)<br>7. User có thể expand/collapse bước để xem thông tin chi tiết<br>8. Khi pipeline hoàn thành, backend gửi thông báo finished<br>9. Hệ thống nhận thông báo, đóng WebSocket và kiểm tra batch pending<br>10. Nếu có batch pending, hiển thị tab approval để User xem kết quả |
| **Ngoại lệ** | - WebSocket connection error: hiển thị thông báo lỗi và thử kết nối lại<br>- Bước bị failed: hiển thị trạng thái FAILED với thông tin lỗi chi tiết<br>- Network timeout: hiển thị thông báo và cho phép User kết nối lại<br>- Không có model config: hiển thị lỗi và dừng tiến trình<br>- API key không hợp lệ: hiển thị lỗi cấu hình model |

---

## Use Case: Hủy tiến trình generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Hủy tiến trình generation |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang có tiến trình generation đang chạy |
| **Hậu điều kiện** | - Tiến trình generation bị hủy<br>- Trạng thái cập nhật thành cancelled |
| **Kịch bản chính** | 1. User đang xem progress bar của tiến trình generation<br>2. User click nút "Hủy tiến trình"<br>3. Hệ thống hiển thị dialog xác nhận hủy<br>4. User xác nhận muốn hủy<br>5. Hệ thống gửi yêu cầu cancel đến backend<br>6. Backend cập nhật trạng thái thành cancelled<br>7. Backend dừng generation<br>8. Hệ thống nhận response và cập nhật UI<br>9. Hệ thống hiển thị thông báo "Đã hủy tiến trình thành công" |
| **Ngoại lệ** | - Tiến trình đã hoàn thành: hiển thị thông báo không thể hủy<br>- Lỗi cancel: hiển thị thông báo lỗi từ server |

---

## Use Case: Xem kết quả batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem kết quả batch |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User có batch pending cần approval |
| **Hậu điều kiện** | - Danh sách item trong batch được hiển thị |
| **Kịch bản chính** | 1. Sau khi generation hoàn thành, hệ thống check pending batch<br>2. Hệ thống gọi API để lấy batch pending<br>3. Backend trả về thông tin batch<br>4. Hệ thống gọi API để lấy danh sách items<br>5. Backend trả về danh sách items<br>6. Hệ thống hiển thị tab duyệt kết quả<br>7. User có thể chuyển đổi giữa layout "single" và "list"<br>8. Trong single layout, User xem từng item với navigation buttons<br>9. Trong list layout, User xem toàn bộ items với checkbox để select<br>10. User có thể edit item trực tiếp trong approval tab |
| **Ngoại lệ** | - Không có batch pending: không hiển thị approval tab<br>- Lỗi load batch: hiển thị thông báo lỗi |

---

## Use Case: Duyệt kết quả batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Duyệt kết quả batch |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang xem kết quả batch trong approval tab |
| **Hậu điều kiện** | - Các item được approve lưu vào Session<br>- Item có thể được sử dụng trong Session |
| **Kịch bản chính** | 1. User đang xem batch items trong approval tab<br>2. User chọn các item muốn approve bằng checkbox<br>3. User click nút "Phê duyệt đã chọn" hoặc "Phê duyệt tất cả"<br>4. Hệ thống gửi yêu cầu approve đến backend<br>5. Backend cập nhật status của items thành "approved"<br>6. Backend lưu items vào Session<br>7. Hệ thống nhận response và cập nhật UI<br>8. Hệ thống reload tab data để hiển thị items mới<br>9. Hệ thống hiển thị thông báo approval thành công<br>10. Nếu còn items chưa xử lý, tiếp tục hiển thị approval tab, nếu không thì ẩn |
| **Ngoại lệ** | - Không có item nào được chọn: hiển thị thông báo "Vui lòng chọn ít nhất một mục để phê duyệt"<br>- Lỗi approve: hiển thị thông báo "Không thể phê duyệt mục" |

---

## Use Case: Từ chối kết quả batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Từ chối kết quả batch |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang xem kết quả batch trong approval tab |
| **Hậu điều kiện** | - Các item bị reject với feedback<br>- Item có thể được regenerate |
| **Kịch bản chính** | 1. User đang xem batch items trong approval tab<br>2. User chọn các item muốn reject bằng checkbox<br>3. User nhập feedback text (tùy chọn, max 500 ký tự)<br>4. User click nút "Hủy" hoặc "Tạo lại"<br>5. Hệ thống gửi yêu cầu reject đến backend<br>6. Backend cập nhật status của items thành "rejected"<br>7. Backend lưu feedback<br>8. Hệ thống nhận response và cập nhật UI<br>9. Hệ thống hiển thị thông báo rejection thành công<br>10. Nếu còn items chưa xử lý, tiếp tục hiển thị approval tab<br>11. Nếu có rejected items, hiển thị nút "Tạo lại" |
| **Ngoại lệ** | - Không có item nào được chọn: hiển thị thông báo "Vui lòng chọn ít nhất một mục"<br>- Feedback quá dài: hiển thị lỗi validation<br>- Lỗi reject: hiển thị thông báo "Không thể hủy mục" hoặc "Không thể tạo lại mục" |

---

## Use Case: Tạo yêu cầu regenerate

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo yêu cầu regenerate |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User có batch với rejected items và feedback |
| **Hậu điều kiện** | - Yêu cầu regenerate được tạo trong hệ thống<br>- Tiến trình generation mới bắt đầu thực thi |
| **Kịch bản chính** | 1. User đang xem batch với rejected items<br>2. User click nút "Regenerate"<br>3. Hệ thống gửi yêu cầu regenerate đến backend<br>4. Backend tạo yêu cầu regenerate mới dựa trên feedback<br>5. Backend trả về thông tin process mới<br>6. Hệ thống nhận response và kết nối với tiến trình mới<br>7. Hệ thống hiển thị progress bar cho tiến trình regenerate<br>8. Sau khi regenerate hoàn thành, hiển thị approval tab mới |
| **Ngoại lệ** | - Không có rejected items: không cho phép regenerate<br>- Lỗi regenerate: hiển thị thông báo lỗi từ server |

```

---

## Use Case: Kết nối lại với tiến trình đang chạy

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Kết nối lại với tiến trình đang chạy |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User có tiến trình generation đang chạy<br>- User reload trang hoặc quay lại Session |
| **Hậu điều kiện** | - WebSocket kết nối lại với tiến trình<br>- User tiếp tục theo dõi tiến trình |
| **Kịch bản chính** | 1. User truy cập lại Session detail page<br>2. Hệ thống load session info và xác nhận session<br>3. Hệ thống kiểm tra tiến trình đang chạy<br>4. Hệ thống gọi API để lấy thông tin tiến trình đang active<br>5. Nếu có tiến trình đang chạy, hệ thống kết nối lại với tiến trình<br>6. Hệ thống gửi message resume<br>7. Hệ thống nhận snapshot hiện tại<br>8. Hệ thống hiển thị progress bar với trạng thái hiện tại<br>9. Tiếp tục nhận updates từ tiến trình |
| **Ngoại lệ** | - Không có tiến trình đang chạy: không làm gì<br>- WebSocket connection error: hiển thị thông báo và retry<br>- Tiến trình đã hoàn thành: hiển thị approval tab nếu có batch pending |
