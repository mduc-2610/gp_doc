```markdown
# Scenario - Quản lý Session Access

## Use Case: Chia sẻ Session với User cụ thể

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chia sẻ Session với User cụ thể |
| **Actor** | Manager |
| **Tiền điều kiện** | - Manager đã đăng nhập thành công<br>- Manager có quyền Manager trên Session<br>- Manager đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - User được chỉ định nhận quyền truy cập vào Session<br>- Grant mới được tạo trong cơ sở dữ liệu<br>- Thông báo realtime được gửi đến User được chia sẻ |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, Manager click nút "Share"<br>2. Hệ thống hiển thị SessionShareModal với tab "Share" được chọn mặc định<br>3. Hệ thống gửi yêu cầu lấy danh sách User đã được chia sẻ (GET /session-access/grant/by-session/{sessionId})<br>4. Backend trả về danh sách SessionAccessGrant<br>5. Hệ thống hiển thị danh sách User đã được chia sẻ<br>6. Manager click nút "Add people"<br>7. Hệ thống hiển thị User search dialog<br>8. Manager tìm kiếm và chọn User cần chia sẻ<br>9. Manager chọn mức quyền (Reader/Contributor/Manager)<br>10. Manager click "Add"<br>11. Hệ thống gửi yêu cầu tạo invite (POST /session-access/invite)<br>12. Backend tạo SessionAccessInvite và tự động tạo SessionAccessGrant<br>13. Backend broadcast WebSocket event PERMISSION_GRANTED<br>14. Hệ thống nhận response và cập nhật danh sách<br>15. Toast success notification hiển thị "Chia sẻ thành công"<br>16. User được chia sẻ nhận thông báo realtime và reload permission |
| **Ngoại lệ** | - User không tồn tại: hiển thị thông báo lỗi<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- User đã được chia sẻ trước đó: hiển thị thông báo<br>- Lỗi tạo grant: hiển thị thông báo lỗi từ server |

---

## Use Case: Cập nhật quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Cập nhật quyền truy cập |
| **Actor** | Manager |
| **Tiền điều kiện** | - Manager đã đăng nhập thành công<br>- Manager có quyền Manager trên Session<br>- User đã được chia sẻ trước đó |
| **Hậu điều kiện** | - Quyền truy cập của User được cập nhật<br>- Grant được cập nhật trong cơ sở dữ liệu<br>- Thông báo realtime được gửi đến User |
| **Kịch bản chính** | 1. Tại SessionShareModal, Manager xem danh sách User đã được chia sẻ<br>2. Manager click vào dropdown quyền của User cần cập nhật<br>3. Manager chọn mức quyền mới (Reader/Contributor/Manager)<br>4. Hệ thống gửi yêu cầu cập nhật grant (PUT /session-access/grant/{grantId})<br>5. Backend cập nhật access_level trong SessionAccessGrant<br>6. Backend broadcast WebSocket event PERMISSION_CHANGED<br>7. Hệ thống nhận response và cập nhật UI<br>8. Toast success notification hiển thị "Cập nhật quyền thành công"<br>9. User nhận thông báo realtime và reload permission |
| **Ngoại lệ** | - Grant không tồn tại: hiển thị thông báo lỗi<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server<br>- WebSocket disconnected: hiển thị cảnh báo kết nối |

---

## Use Case: Thu hồi quyền truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Thu hồi quyền truy cập |
| **Actor** | Manager |
| **Tiền điều kiện** | - Manager đã đăng nhập thành công<br>- Manager có quyền Manager trên Session<br>- User đã được chia sẻ trước đó |
| **Hậu điều kiện** | - Quyền truy cập của User bị xóa<br>- Grant bị revoke trong cơ sở dữ liệu<br>- Thông báo realtime được gửi đến User<br>- User bị đẩy ra khỏi Session nếu đang truy cập |
| **Kịch bản chính** | 1. Tại SessionShareModal, Manager xem danh sách User đã được chia sẻ<br>2. Manager click icon "X" để xóa quyền của User<br>3. Hệ thống gửi yêu cầu revoke grant (DELETE /session-access/grant/revoke/{grantId})<br>4. Backend set is_active=false trong SessionAccessGrant<br>5. Backend broadcast WebSocket event PERMISSION_REVOKED<br>6. Hệ thống nhận response và xóa User khỏi danh sách<br>7. Toast success notification hiển thị "Thu hồi quyền thành công"<br>8. User nhận thông báo realtime, permission state thay đổi<br>9. Nếu User đang trong Session, component chuyển sang RequestAccessState |
| **Ngoại lệ** | - Grant không tồn tại: hiển thị thông báo lỗi<br>- Lỗi revoke: hiển thị thông báo lỗi từ server<br>- WebSocket disconnected: hiển thị cảnh báo kết nối |

---

## Use Case: Yêu cầu truy cập Session

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Yêu cầu truy cập Session |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User chưa có quyền truy cập Session hoặc quyền bị hạn chế<br>- User truy cập vào Session URL |
| **Hậu điều kiện** | - Yêu cầu truy cập được tạo trong hệ thống<br>- Manager nhận thông báo có yêu cầu mới<br>- User thấy trạng thái "Pending" |
| **Kịch bản chính** | 1. User truy cập Session URL mà không có quyền<br>2. Hệ thống kiểm tra permission và phát hiện User không có quyền<br>3. Component RequestAccessState được hiển thị<br>4. Hệ thống gửi yêu cầu kiểm tra request hiện tại (GET /session-access/request/by-user-session/{sessionId}/{userId})<br>5. Backend trả về request hiện tại nếu có, hoặc null<br>6. User click nút "Request Access"<br>7. Hệ thống gửi yêu cầu tạo request (POST /session-access/request)<br>8. Backend tạo SessionAccessRequest với status=PENDING<br>9. Backend broadcast WebSocket event REQUEST_CREATED đến Manager<br>10. Hệ thống nhận response và cập nhật UI<br>11. Toast success notification hiển thị "Yêu cầu đã được gửi"<br>12. UI hiển thị trạng thái "Pending" với nút "Cancel Request" |
| **Ngoại lệ** | - User đã có request pending: hiển thị trạng thái pending ngay<br>- Session không tồn tại: hiển thị thông báo lỗi<br>- Lỗi tạo request: hiển thị thông báo lỗi từ server |

---

## Use Case: Chấp nhận/Từ chối yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xử lý yêu cầu truy cập |
| **Actor** | Manager |
| **Tiền điều kiện** | - Manager đã đăng nhập thành công<br>- Manager có quyền Manager trên Session<br>- Có ít nhất một yêu cầu truy cập pending |
| **Hậu điều kiện (Approve)** | - Request status = APPROVED<br>- Grant mới được tạo cho User<br>- User nhận quyền truy cập<br>- Thông báo realtime được gửi |
| **Hậu điều kiện (Deny)** | - Request status = REJECTED<br>- User không nhận quyền<br>- Thông báo realtime được gửi |
| **Kịch bản chính (Approve)** | 1. Manager mở SessionShareModal<br>2. Manager click tab "Request"<br>3. Hệ thống gửi yêu cầu lấy danh sách request (GET /session-access/request/by-session/{sessionId})<br>4. Backend trả về danh sách SessionAccessRequest với status=PENDING<br>5. Hệ thống hiển thị danh sách request<br>6. Manager chọn mức quyền cho User (Reader/Contributor/Manager)<br>7. Manager click "Approve"<br>8. Hệ thống gửi yêu cầu approve (POST /session-access/request/accept/{requestId})<br>9. Backend cập nhật request status=APPROVED<br>10. Backend tạo SessionAccessGrant cho User<br>11. Backend broadcast WebSocket event PERMISSION_GRANTED<br>12. Hệ thống nhận response và xóa request khỏi danh sách<br>13. Toast success notification hiển thị "Đã chấp nhận yêu cầu"<br>14. User nhận thông báo realtime, permission state thay đổi, reload Session |
| **Kịch bản chính (Deny)** | 1-5. Giống bước 1-5 ở trên<br>6. Manager click "Deny"<br>7. Hệ thống gửi yêu cầu reject (POST /session-access/request/reject/{requestId})<br>8. Backend cập nhật request status=REJECTED<br>9. Backend broadcast WebSocket event REQUEST_DELETED<br>10. Hệ thống nhận response và xóa request khỏi danh sách<br>11. Toast success notification hiển thị "Đã từ chối yêu cầu"<br>12. User nhận thông báo, request bị xóa |
| **Ngoại lệ** | - Request không tồn tại: hiển thị thông báo lỗi<br>- Lỗi xử lý request: hiển thị thông báo lỗi từ server |

---

## Use Case: Hủy yêu cầu truy cập

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Hủy yêu cầu truy cập |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đã gửi yêu cầu truy cập<br>- Request đang ở trạng thái PENDING |
| **Hậu điều kiện** | - Request bị xóa khỏi hệ thống<br>- Manager không còn thấy request này<br>- UI quay về trạng thái ban đầu |
| **Kịch bản chính** | 1. User xem RequestAccessState với trạng thái "Pending"<br>2. User click nút "Cancel Request"<br>3. Hệ thống gửi yêu cầu xóa request (DELETE /session-access/request/{requestId})<br>4. Backend xóa SessionAccessRequest<br>5. Backend broadcast WebSocket event REQUEST_DELETED<br>6. Hệ thống nhận response và cập nhật UI<br>7. Toast success notification hiển thị "Đã hủy yêu cầu"<br>8. UI hiển thị lại nút "Request Access" |
| **Ngoại lệ** | - Request không tồn tại: hiển thị thông báo lỗi<br>- Lỗi xóa request: hiển thị thông báo lỗi từ server |

---

## Use Case: Quản lý liên kết chia sẻ công khai

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Quản lý liên kết chia sẻ công khai (General Access Link) |
| **Actor** | Manager |
| **Tiền điều kiện** | - Manager đã đăng nhập thành công<br>- Manager có quyền Manager trên Session |
| **Hậu điều kiện** | - SessionAccessLink được cập nhật<br>- Cấu hình chia sẻ công khai thay đổi<br>- Thông báo realtime được gửi đến các User |
| **Kịch bản chính** | 1. Manager mở SessionShareModal<br>2. Hệ thống gửi yêu cầu lấy link config (GET /session-access/link/by-session/{sessionId})<br>3. Backend trả về SessionAccessLink<br>4. Hệ thống hiển thị toggle switch và dropdown quyền mặc định<br>5a. **Bật/tắt link:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- Manager toggle switch on/off<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống gửi yêu cầu cập nhật (PUT /session-access/link/{linkId})<br>&nbsp;&nbsp;&nbsp;&nbsp;- Backend cập nhật is_active<br>&nbsp;&nbsp;&nbsp;&nbsp;- Backend broadcast WebSocket event LINK_UPDATED<br>&nbsp;&nbsp;&nbsp;&nbsp;- Toast hiển thị "Link đã được bật/tắt"<br>5b. **Thay đổi quyền mặc định:**<br>&nbsp;&nbsp;&nbsp;&nbsp;- Manager chọn quyền mới (Reader/Contributor/Manager)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Hệ thống gửi yêu cầu cập nhật (PUT /session-access/link/{linkId})<br>&nbsp;&nbsp;&nbsp;&nbsp;- Backend cập nhật access_level<br>&nbsp;&nbsp;&nbsp;&nbsp;- Backend broadcast WebSocket event LINK_UPDATED<br>&nbsp;&nbsp;&nbsp;&nbsp;- Toast hiển thị "Quyền mặc định đã được cập nhật"<br>6. Manager click "Copy link"<br>7. Hệ thống copy URL Session vào clipboard<br>8. Toast hiển thị "Đã copy link" |
| **Ngoại lệ** | - Link không tồn tại: tạo link mới<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server |

---

## Use Case: Nhận cập nhật quyền realtime

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Nhận cập nhật quyền realtime |
| **Actor** | User, Manager |
| **Tiền điều kiện** | - User/Manager đã đăng nhập<br>- User/Manager đang truy cập Session<br>- WebSocket connection đã được thiết lập |
| **Hậu điều kiện** | - UI được cập nhật theo thay đổi quyền<br>- User thấy quyền mới hoặc bị đẩy ra nếu quyền bị thu hồi |
| **Kịch bản chính** | 1. User/Manager truy cập Session<br>2. Component DocAgentPage khởi tạo<br>3. Hệ thống tạo WebSocket connection (connectSessionSocket)<br>4. Hệ thống gửi message "subscribe" qua WebSocket<br>5. Hệ thống bắt đầu keep-alive ping mỗi 30 giây<br>6. **Khi có sự kiện từ server:**<br>&nbsp;&nbsp;&nbsp;&nbsp;a. **PERMISSION_GRANTED:** User nhận quyền mới<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Toast hiển thị "Bạn đã được cấp quyền truy cập"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- setPermissionLoading(true) để reload permission<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Component reload Session data<br>&nbsp;&nbsp;&nbsp;&nbsp;b. **PERMISSION_CHANGED:** Quyền của User thay đổi<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Toast hiển thị "Quyền của bạn đã được cập nhật"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- setPermissionLoading(true) để reload permission<br>&nbsp;&nbsp;&nbsp;&nbsp;c. **PERMISSION_REVOKED:** Quyền bị thu hồi<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Toast hiển thị "Quyền truy cập của bạn đã bị thu hồi"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- setPermissionLoading(true) để reload permission<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Component chuyển sang RequestAccessState<br>&nbsp;&nbsp;&nbsp;&nbsp;d. **REQUEST_CREATED:** Manager nhận thông báo có request mới<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- setRequestTabReloadKey tăng lên để reload request list<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Toast hiển thị "Có yêu cầu truy cập mới"<br>&nbsp;&nbsp;&nbsp;&nbsp;e. **REQUEST_DELETED:** Request bị xóa hoặc từ chối<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- setRequestTabReloadKey tăng lên để reload request list<br>&nbsp;&nbsp;&nbsp;&nbsp;f. **LINK_UPDATED:** Link config thay đổi<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Reload link config trong SessionShareModal<br>7. Khi User rời Session hoặc unmount component<br>8. Hệ thống đóng WebSocket connection |
| **Ngoại lệ** | - WebSocket connection lost: hiển thị cảnh báo và tự động reconnect<br>- Parse message error: log error và bỏ qua message |

```