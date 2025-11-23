# Sequence Scenario - Quản lý các Session

## Kịch bản 1: Xem danh sách Session

1. Sau khi đăng nhập, User click "Tài liệu" tại sidebar
2. DocAgentSessionPage.js khởi tạo và gọi `DocAgentService.getSessions()` để lấy danh sách Session
3. DocAgentService gửi request GET tới endpoint `/session/by-user/{userId}`
4. Backend route trong session_routes.py nhận request
5. SessionService được gọi hàm `get_sessions_by_user()`
6. SessionService truy vấn cơ sở dữ liệu để lấy các Session mà User là MANAGER
7. SessionService convert Session objects thành SessionResponse objects
8. session_routes.py trả về danh sách SessionResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Session[]>
10. DocAgentSessionPage.js cập nhật state `sessions` với dữ liệu nhận được
11. Component re-render và hiển thị danh sách Session cho User

---

## Kịch bản 2: Tạo Session mới

1. Sau khi đăng nhập, User click "Tài liệu" tại sidebar
2. DocAgentSessionPage.js khởi tạo và gọi `DocAgentService.getSessions()` để lấy danh sách Session
3. DocAgentService gửi request GET tới endpoint `/session/by-user/{userId}`
4. Backend route trong session_routes.py nhận request
5. SessionService được gọi hàm `get_sessions_by_user()`
6. SessionService truy vấn cơ sở dữ liệu để lấy các Session mà User là MANAGER
7. SessionService convert Session objects thành SessionResponse objects
8. session_routes.py trả về danh sách SessionResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Session[]>
10. DocAgentSessionPage.js cập nhật state `sessions` với dữ liệu nhận được
11. Component re-render và hiển thị danh sách Session cho User
12. User click "Tạo Session mới"
13. DocAgentSessionPage.js gọi tới hàm `handleCreateSession()`
14. Hàm `handleCreateSession()` mở dialog SessionCreateDialog.js
15. SessionCreateDialog.js hiển thị giao diện cho User
16. User nhập tên Session, mô tả Session, và click "Tạo"
17. SessionCreateDialog.js gọi tới lớp DocAgentService
18. Lớp DocAgentService gọi tới hàm `createSession()`
19. Hàm `createSession()` gửi yêu cầu với phương thức POST đến endpoint `/session`
20. Backend route trong session_routes.py nhận request
21. SessionService được gọi `create_session()`
22. SessionService tạo Session model mới với các field: name, description, created_by, auto_grant_invite, is_active
23. SessionService thêm session vào database (db.add, db.commit)
24. Database trigger `after_session_insert` được kích hoạt tự động tạo SessionAccessLink và SessionAccessGrant
25. SessionService convert Session thành SessionResponse
26. session_routes.py trả về SessionResponse JSON
27. DocAgentService nhận response và đóng gói với ServiceResult<Session>
28. SessionCreateDialog gọi callback `onSessionCreated()`
29. DocAgentSessionPage.js điều hướng User tới trang chi tiết Session vừa tạo
30. Toast success notification được hiển thị

---

## Kịch bản 3: Chỉnh sửa Session

1. Sau khi đăng nhập, User click "Tài liệu" tại sidebar
2. DocAgentSessionPage.js khởi tạo và gọi `DocAgentService.getSessions()` để lấy danh sách Session
3. DocAgentService gửi request GET tới endpoint `/session/by-user/{userId}`
4. Backend route trong session_routes.py nhận request
5. SessionService được gọi hàm `get_sessions_by_user()`
6. SessionService truy vấn cơ sở dữ liệu để lấy các Session mà User là MANAGER
7. SessionService convert Session objects thành SessionResponse objects
8. session_routes.py trả về danh sách SessionResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Session[]>
10. DocAgentSessionPage.js cập nhật state `sessions` với dữ liệu nhận được
11. Component re-render và hiển thị danh sách Session cho User
12. User click "Chỉnh sửa" trên một Session
13. DocAgentSessionPage.js gọi tới dialog SessionUpdateDialog.js
14. SessionUpdateDialog.js hiển thị giao diện với thông tin Session hiện tại
15. User nhập thông tin và click "Lưu"
16. SessionUpdateDialog.js gọi tới lớp DocAgentService
17. Lớp DocAgentService gọi tới hàm `updateSession()`
18. Hàm `updateSession()` gửi yêu cầu với phương thức PUT đến endpoint `/session/{sessionId}`
19. Backend route trong session_routes.py nhận request
20. SessionService được gọi `update_session()`
21. SessionService lấy session hiện tại từ database, cập nhật các field và commit transaction
22. SessionService convert thành SessionResponse
23. session_routes.py trả về SessionResponse JSON
24. DocAgentService nhận response và đóng gói với ServiceResult<Session>
25. SessionUpdateDialog đóng
26. DocAgentSessionPage.js cập nhật item trong danh sách Session
27. Component re-render
28. Toast success notification được hiển thị: "Cập nhật Session thành công"

---

## Kịch bản 4: Xóa Session

1. Sau khi đăng nhập, User click "Tài liệu" tại sidebar
2. DocAgentSessionPage.js khởi tạo và gọi `DocAgentService.getSessions()` để lấy danh sách Session
3. DocAgentService gửi request GET tới endpoint `/session/by-user/{userId}`
4. Backend route trong session_routes.py nhận request
5. SessionService được gọi hàm `get_sessions_by_user()`
6. SessionService truy vấn cơ sở dữ liệu để lấy các Session mà User là MANAGER
7. SessionService convert Session objects thành SessionResponse objects
8. session_routes.py trả về danh sách SessionResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Session[]>
10. DocAgentSessionPage.js cập nhật state `sessions` với dữ liệu nhận được
11. Component re-render và hiển thị danh sách Session cho User
12. User click "Xóa" trên một Session
13. DocAgentSessionPage.js gọi tới dialog SessionDeleteDialog.js
14. SessionDeleteDialog.js hiển thị giao diện xác nhận xóa
15. User xem thông tin Session sắp xóa
16. User click "Xóa"
17. SessionDeleteDialog.js gọi tới lớp DocAgentService
18. Lớp DocAgentService gọi tới hàm `deleteSession()`
19. Hàm `deleteSession()` gửi yêu cầu với phương thức DELETE đến endpoint `/session/{sessionId}`
20. Backend route trong session_routes.py nhận request
21. SessionService được gọi `delete_session()`
22. SessionService lấy session từ database, kiểm tra session có tồn tại
23. SessionService xóa session khỏi database (db.delete, db.commit())
24. SessionService trả MessageResponse
25. session_routes.py trả về MessageResponse JSON
26. DocAgentService nhận response
27. SessionDeleteDialog đóng
28. DocAgentSessionPage.js loại bỏ item khỏi danh sách Session
29. Component re-render, danh sách Session được cập nhật
30. Toast success notification được hiển thị: "Xóa Session thành công"

---
