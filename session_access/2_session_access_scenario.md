# Scenario - Quản lý Session Access

## Use Case: Chia sẻ Session với User cụ thể

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chia sẻ Session với User cụ thể |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - User được chỉ định nhận quyền truy cập vào Session<br>- Thông báo được gửi đến User được chia sẻ |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click chia sẻ<br>2. Hệ thống hiển thị dialog chia sẻ với danh sách User đã được chia sẻ<br>3. User click thêm người dùng<br>4. Hệ thống hiển thị dialog tìm kiếm User<br>5. User nhập từ khóa tìm kiếm<br>6. Hệ thống hiển thị kết quả tìm kiếm (loại bỏ User đã được chia sẻ)<br>7. User chọn User cần chia sẻ, chọn mức quyền (Reader/Contributor/Manager) và click "Thêm"<br>8. Hệ thống gửi yêu cầu chia sẻ đến backend<br>9. Backend tạo quyền truy cập cho User<br>10. Hệ thống cập nhật danh sách, hiển thị thông báo "Đã gửi lời mời thành công" và User được chia sẻ nhận thông báo realtime "Bạn đã được cấp quyền {{level}}" |
| **Ngoại lệ** | - User không tồn tại: hiển thị thông báo "Không tìm thấy người dùng"<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- User đã được chia sẻ trước đó: hiển thị thông báo "Lời mời đã tồn tại"<br>- Lỗi chia sẻ: hiển thị thông báo "Không thể tạo lời mời" |

---

## Use Case: Cập nhật quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Cập nhật quyền truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đã được chia sẻ trước đó |
| **Hậu điều kiện** | - Quyền truy cập của User được cập nhật<br>- Thông báo được gửi đến User |
| **Kịch bản chính** | 1. Tại dialog chia sẻ, User xem danh sách User đã được chia sẻ<br>2. User chọn User cần cập nhật quyền và chọn mức quyền mới (Reader/Contributor/Manager)<br>3. Hệ thống gửi yêu cầu cập nhật quyền đến backend<br>4. Backend cập nhật quyền truy cập<br>5. Hệ thống cập nhật giao diện, hiển thị thông báo thành công và User nhận thông báo realtime về thay đổi quyền |
| **Ngoại lệ** | - Quyền không tồn tại: hiển thị thông báo lỗi<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server |

---

## Use Case: Thu hồi quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Thu hồi quyền truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đã được chia sẻ trước đó |
| **Hậu điều kiện** | - Quyền truy cập của User bị xóa<br>- Thông báo được gửi đến User<br>- User bị đẩy ra khỏi Session nếu đang truy cập |
| **Kịch bản chính** | 1. Tại dialog chia sẻ, User xem danh sách User đã được chia sẻ<br>2. User chọn xóa quyền của User<br>3. Hệ thống yêu cầu xác nhận thu hồi quyền<br>4. User xác nhận muốn thu hồi<br>5. Hệ thống gửi yêu cầu thu hồi quyền đến backend<br>6. Backend xóa quyền truy cập<br>7. Hệ thống cập nhật danh sách, hiển thị thông báo thành công và User nhận thông báo realtime về việc quyền bị thu hồi |
| **Ngoại lệ** | - Quyền không tồn tại: hiển thị thông báo lỗi<br>- Lỗi thu hồi: hiển thị thông báo lỗi từ server |

---

## Use Case: Yêu cầu truy cập Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Yêu cầu truy cập Session |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User chưa có quyền truy cập Session |
| **Hậu điều kiện** | - Yêu cầu truy cập được tạo trong hệ thống<br>- Chủ sở hữu Session nhận thông báo có yêu cầu mới |
| **Kịch bản chính** | 1. User truy cập Session mà không có quyền<br>2. Hệ thống kiểm tra quyền, phát hiện User không có quyền truy cập và hiển thị giao diện yêu cầu truy cập<br>3. User click gửi yêu cầu truy cập<br>4. Hệ thống gửi yêu cầu tạo request đến backend<br>5. Backend tạo yêu cầu truy cập với trạng thái chờ duyệt<br>6. Hệ thống hiển thị thông báo "Yêu cầu đã được gửi" và Chủ sở hữu Session nhận thông báo về yêu cầu mới |
| **Ngoại lệ** | - User đã có yêu cầu đang chờ: hiển thị trạng thái yêu cầu hiện tại<br>- Session không tồn tại: hiển thị thông báo lỗi<br>- Lỗi tạo yêu cầu: hiển thị thông báo lỗi từ server |

---

## Use Case: Chấp nhận/Từ chối yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xử lý yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- Có ít nhất một yêu cầu truy cập đang chờ duyệt |
| **Hậu điều kiện (Chấp nhận)** | - Yêu cầu được chấp nhận<br>- User nhận quyền truy cập vào Session<br>- Thông báo được gửi đến User |
| **Hậu điều kiện (Từ chối)** | - Yêu cầu bị từ chối<br>- User không nhận quyền<br>- Thông báo được gửi đến User |
| **Kịch bản chính (Chấp nhận)** | 1. User mở dialog chia sẻ và chọn tab yêu cầu truy cập<br>2. Hệ thống hiển thị danh sách yêu cầu đang chờ duyệt<br>3. User chọn mức quyền cho User (Reader/Contributor/Manager) và click chấp nhận yêu cầu<br>4. Hệ thống gửi yêu cầu chấp nhận đến backend<br>5. Backend cập nhật trạng thái yêu cầu và tạo quyền truy cập<br>6. Hệ thống cập nhật danh sách, hiển thị thông báo thành công và User nhận thông báo realtime về quyền mới |
| **Kịch bản chính (Từ chối)** | 1. User mở dialog chia sẻ và chọn tab yêu cầu truy cập<br>2. Hệ thống hiển thị danh sách yêu cầu đang chờ duyệt<br>3. User click từ chối yêu cầu<br>4. Hệ thống gửi yêu cầu từ chối đến backend<br>5. Backend cập nhật trạng thái yêu cầu<br>6. Hệ thống cập nhật danh sách, hiển thị thông báo và User nhận thông báo về việc yêu cầu bị từ chối |
| **Ngoại lệ** | - Yêu cầu không tồn tại: hiển thị thông báo lỗi<br>- Lỗi xử lý yêu cầu: hiển thị thông báo lỗi từ server |

---

## Use Case: Hủy yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Hủy yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đã gửi yêu cầu truy cập đang chờ duyệt |
| **Hậu điều kiện** | - Yêu cầu bị hủy khỏi hệ thống<br>- Chủ sở hữu Session không còn thấy yêu cầu này |
| **Kịch bản chính** | 1. User xem giao diện yêu cầu truy cập với trạng thái đang chờ duyệt<br>2. User click hủy yêu cầu<br>3. Hệ thống gửi yêu cầu hủy đến backend<br>4. Backend xóa yêu cầu truy cập<br>5. Hệ thống cập nhật giao diện, hiển thị thông báo thành công và quay về trạng thái ban đầu |
| **Ngoại lệ** | - Yêu cầu không tồn tại: hiển thị thông báo lỗi<br>- Lỗi hủy yêu cầu: hiển thị thông báo lỗi từ server |

---

## Use Case: Quản lý liên kết chia sẻ công khai

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Quản lý liên kết chia sẻ công khai |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công |
| **Hậu điều kiện** | - Cấu hình chia sẻ công khai được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. User mở dialog chia sẻ<br>2. Hệ thống hiển thị cấu hình liên kết chia sẻ công khai<br>3. User có thể thực hiện các thao tác:<br>&nbsp;&nbsp;&nbsp;&nbsp;**a. Bật/tắt liên kết:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- User bật hoặc tắt chế độ chia sẻ công khai<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống gửi yêu cầu cập nhật đến backend<br>&nbsp;&nbsp;&nbsp;&nbsp;- Backend cập nhật trạng thái liên kết<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống hiển thị thông báo thành công<br>&nbsp;&nbsp;&nbsp;&nbsp;**b. Thay đổi quyền mặc định:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- User chọn quyền mặc định mới (Reader/Contributor/Manager)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống gửi yêu cầu cập nhật đến backend<br>&nbsp;&nbsp;&nbsp;&nbsp;- Backend cập nhật quyền mặc định<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống hiển thị thông báo thành công<br>&nbsp;&nbsp;&nbsp;&nbsp;**c. Sao chép liên kết:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- User click sao chép liên kết<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống sao chép URL Session vào clipboard và hiển thị thông báo "Đã sao chép liên kết" |
| **Ngoại lệ** | - Liên kết không tồn tại: tạo liên kết mới<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server |

---

## Use Case: Nhận cập nhật quyền realtime

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Nhận cập nhật quyền realtime |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang truy cập Session |
| **Hậu điều kiện** | - Giao diện được cập nhật theo thay đổi quyền |
| **Kịch bản chính** | 1. User truy cập Session<br>2. Hệ thống thiết lập kết nối realtime và lắng nghe các sự kiện thay đổi quyền<br>3. Khi có sự kiện từ server:<br>&nbsp;&nbsp;&nbsp;&nbsp;**a. Nhận quyền mới:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống hiển thị thông báo "Bạn đã được cấp quyền truy cập", tải lại thông tin quyền và cập nhật hiển thị Session<br>&nbsp;&nbsp;&nbsp;&nbsp;**b. Quyền thay đổi:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống hiển thị thông báo "Quyền của bạn đã được cập nhật" và tải lại thông tin quyền<br>&nbsp;&nbsp;&nbsp;&nbsp;**c. Quyền bị thu hồi:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống hiển thị thông báo "Quyền truy cập của bạn đã bị thu hồi" và chuyển sang giao diện yêu cầu truy cập<br>&nbsp;&nbsp;&nbsp;&nbsp;**d. Có yêu cầu truy cập mới:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống cập nhật danh sách yêu cầu và hiển thị thông báo "Có yêu cầu truy cập mới"<br>&nbsp;&nbsp;&nbsp;&nbsp;**e. Yêu cầu bị xóa:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống cập nhật danh sách yêu cầu<br>&nbsp;&nbsp;&nbsp;&nbsp;**f. Cấu hình liên kết thay đổi:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống tải lại cấu hình liên kết chia sẻ<br>4. Khi User rời Session, hệ thống đóng kết nối realtime |
| **Ngoại lệ** | - Mất kết nối realtime: hiển thị cảnh báo và tự động kết nối lại<br>- Lỗi nhận thông báo: ghi log lỗi |