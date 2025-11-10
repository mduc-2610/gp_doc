# Sequence Scenario - Quản lý các Session

## Kịch bản 1: Xem danh sách Session

1. Sau khi đăng nhập, User click nút "Tài liệu" tại sidebar
2. DocAgentSessionPage.js khởi tạo và gọi `DocAgentService.getSessions()` để lấy danh sách Session
3. DocAgentService gửi request GET tới endpoint `/session/by-user/{userId}`
4. Backend route `@router.get("/by-user/{user_id}")` trong session_routes.py nhận request
5. SessionService được gọi hàm `get_sessions_by_user()`
6. SessionService truy vấn cơ sở dữ liệu để lấy các Session mà User là MANAGER
7. SessionService convert Session objects thành SessionResponse objects
8. session_routes.py trả về danh sách SessionResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Session[]>
10. DocAgentSessionPage.js cập nhật state `sessions` với dữ liệu nhận được
11. Component re-render và hiển thị danh sách Session cho User

---

## Kịch bản 2: Tạo Session mới

1. Tại giao diện danh sách Session, User click nút "Tạo Session mới"
2. DocAgentSessionPage.js gọi tới hàm `handleCreateSession()`
3. Hàm `handleCreateSession()` mở dialog SessionCreateDialog.js
4. SessionCreateDialog.js hiển thị giao diện cho User
5. User nhập tên Session, mô tả Session, và click nút "Tạo"
6. SessionCreateDialog.js gọi tới lớp DocAgentService
7. Lớp DocAgentService gọi tới hàm `createSession()`
8. Hàm `createSession()` gửi yêu cầu với phương thức POST đến endpoint `/session`
9. Backend route trong session_routes.py nhận request
10. SessionService được gọi `create_session()`
11. SessionService tạo Session model mới với các field: name, description, created_by, auto_grant_invite, is_active
12. SessionService thêm session vào database (db.add, db.commit)
13. Database trigger `after_session_insert` được kích hoạt tự động tạo SessionAccessLink và SessionAccessGrant
14. SessionService convert Session thành SessionResponse
15. session_routes.py trả về SessionResponse JSON
16. DocAgentService nhận response và đóng gói với ServiceResult<Session>
17. SessionCreateDialog gọi callback `onSessionCreated()`
18. DocAgentSessionPage.js điều hướng User tới trang chi tiết Session vừa tạo
19. Toast success notification được hiển thị

---

## Kịch bản 3: Chỉnh sửa Session

1. Tại giao diện danh sách Session, User click nút "Chỉnh sửa" trên một Session
2. DocAgentSessionPage.js gọi tới dialog SessionUpdateDialog.js
3. SessionUpdateDialog.js hiển thị giao diện với thông tin Session hiện tại
4. User nhập thông tin và click nút "Lưu"
5. SessionUpdateDialog.js gọi tới lớp DocAgentService
6. Lớp DocAgentService gọi tới hàm `updateSession()`
7. Hàm `updateSession()` gửi yêu cầu với phương thức PUT đến endpoint `/session/{sessionId}`
8. Backend route trong session_routes.py nhận request
9. SessionService được gọi `update_session()`
10. SessionService lấy session hiện tại từ database, cập nhật các field và commit transaction
11. SessionService convert thành SessionResponse
12. session_routes.py trả về SessionResponse JSON
13. DocAgentService nhận response và đóng gói với ServiceResult<Session>
14. SessionUpdateDialog đóng
15. DocAgentSessionPage.js cập nhật item trong danh sách Session
16. Component re-render
17. Toast success notification được hiển thị: "Cập nhật Session thành công"

---

## Kịch bản 4: Xóa Session

1. Tại giao diện danh sách Session, User click nút "Xóa" trên một Session
2. DocAgentSessionPage.js gọi tới dialog SessionDeleteDialog.js
3. SessionDeleteDialog.js hiển thị giao diện xác nhận xóa
4. User xem thông tin Session sắp xóa
5. User click nút "Xóa"
6. SessionDeleteDialog.js gọi tới lớp DocAgentService
7. Lớp DocAgentService gọi tới hàm `deleteSession()`
8. Hàm `deleteSession()` gửi yêu cầu với phương thức DELETE đến endpoint `/session/{sessionId}`
9. Backend route trong session_routes.py nhận request
10. SessionService được gọi `delete_session()`
11. SessionService lấy session từ database, kiểm tra session có tồn tại
12. SessionService xóa session khỏi database (db.delete, db.commit())
13. SessionService trả MessageResponse
14. session_routes.py trả về MessageResponse JSON
15. DocAgentService nhận response
16. SessionDeleteDialog đóng
17. DocAgentSessionPage.js loại bỏ item khỏi danh sách Session
18. Component re-render, danh sách Session được cập nhật
19. Toast success notification được hiển thị: "Xóa Session thành công"

---
