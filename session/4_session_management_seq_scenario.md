# Sequence Scenario - Quản lý Document

## Kịch bản 1: Xem danh sách Session

1. User click "Tài liệu" từ sidebar
2. DocAgentSessionPage.tsx gọi tới hàm loadSessions()
3. Hàm loadSessions() gọi tới lớp DocAgentService
4. Lớp DocAgentService gọi tới hàm getSessions()
5. Hàm getSessions() gửi yêu cầu với phương thức GET đến endpoint "/session" tới session_routes.py phía backend thông qua RESTFul API
6. Lớp session_routes.py gọi hàm get_sessions()
7. Hàm get_sessions() gọi đến lớp SessionService để truy vấn trong cơ sở dữ liệu
8. Lớp SessionService sẽ gọi đến hàm get_sessions()
9. Hàm get_sessions() gọi tới lớp Session để đóng gói thông tin thực thể
10. Lớp Session đóng gói thông tin thực thể
11. Lớp Session trả kết quả về cho hàm get_sessions()
12. Hàm get_sessions() trả kết quả về cho lớp session_routes.py
13. Lớp session_routes.py trả về dữ liệu cho hàm getSessions() phía Frontend
14. Hàm getSessions() trả kết quả cho DocAgentSessionPage.tsx
15. DocAgentSessionPage.tsx hiển thị danh sách Session cho User

---

## Kịch bản 2: Tạo Session mới

1. User click "Tạo phiên mới"
2. DocAgentSessionPage.tsx gọi tới SessionCreateDialog.tsx
3. SessionCreateDialog.tsx hiển thị dialog cho User
4. User nhập thông tin tên Session, mô tả Session và click "Tạo phiên"
5. SessionCreateDialog.tsx gọi tới hàm validate()
6. Hàm validate() kiểm tra dữ liệu hợp lệ
7. SessionCreateDialog.tsx gọi tới hàm handleCreateSession()
8. Hàm handleCreateSession() gọi tới lớp DocAgentService
9. Lớp DocAgentService gọi tới hàm createSession()
10. Hàm createSession() gửi yêu cầu với phương thức POST đến endpoint "/session" tới session_routes.py phía backend thông qua RESTFul API
11. Lớp session_routes.py gọi hàm create_session()
12. Hàm create_session() gọi đến lớp SessionService để xử lý logic nghiệp vụ
13. Lớp SessionService sẽ gọi đến hàm create_session()
14. Hàm create_session() gọi tới lớp Session để tạo thực thể mới
15. Lớp Session đóng gói thông tin thực thể
16. Lớp Session trả kết quả về cho hàm create_session()
17. Hàm create_session() trả kết quả về cho lớp session_routes.py
18. Lớp session_routes.py trả về dữ liệu cho hàm createSession() phía Frontend
19. Hàm createSession() trả kết quả cho DocAgentSessionPage.tsx
20. DocAgentSessionPage.tsx hiển thị thông báo thành công cho User

---

## Kịch bản 3: Xóa Session

1. User click icon delete trên Session
2. DocAgentSessionPage.tsx gọi tới SessionDeleteDialog.tsx
3. SessionDeleteDialog.tsx hiển thị dialog xác nhận cho User
4. User click "Xác nhận"
5. SessionDeleteDialog.tsx gọi tới hàm handleDeleteSession()
6. Hàm handleDeleteSession() gọi tới lớp DocAgentService
7. Lớp DocAgentService gọi tới hàm deleteSession()
8. Hàm deleteSession() gửi yêu cầu với phương thức DELETE đến endpoint "/session/{session_id}" tới session_routes.py phía backend thông qua RESTFul API
9. Lớp session_routes.py gọi hàm delete_session()
10. Hàm delete_session() gọi đến lớp SessionService để xử lý logic nghiệp vụ
11. Lớp SessionService sẽ gọi đến hàm delete_session()
12. Hàm delete_session() gọi tới lớp MessageResponse để tạo thông báo phản hồi
13. Lớp MessageResponse đóng gói thông tin phản hồi
14. Lớp MessageResponse trả kết quả về cho hàm delete_session()
15. Hàm delete_session() trả kết quả về cho lớp session_routes.py
16. Lớp session_routes.py trả về dữ liệu cho hàm deleteSession() phía Frontend
17. Hàm deleteSession() trả kết quả cho DocAgentSessionPage.tsx
18. DocAgentSessionPage.tsx hiển thị thông báo xóa thành công cho User

---

## Kịch bản 4: Xem chi tiết Session

1. User click vào 1 Session trong danh sách
2. DocAgentSessionPage.tsx gọi tới DocAgentPage.tsx
3. DocAgentPage.tsx gọi tới Header.tsx
4. Header.tsx gọi tới hàm loadSession()
5. Hàm loadSession() gọi tới lớp DocAgentService
6. Lớp DocAgentService gọi tới hàm getSession()
7. Hàm getSession() gửi yêu cầu với phương thức GET đến endpoint "/session/{session_id}" tới session_routes.py phía backend thông qua RESTFul API
8. Lớp session_routes.py gọi hàm get_session()
9. Hàm get_session() gọi đến lớp SessionService để truy vấn trong cơ sở dữ liệu
10. Lớp SessionService sẽ gọi đến hàm get_session()
11. Hàm get_session() gọi tới lớp Session để đóng gói thông tin thực thể
12. Lớp Session đóng gói thông tin thực thể
13. Lớp Session trả kết quả về cho hàm get_session()
14. Hàm get_session() trả kết quả về cho lớp session_routes.py
15. Lớp session_routes.py trả về dữ liệu cho hàm getSession() phía Frontend
16. Hàm getSession() trả kết quả cho DocAgentPage.tsx
17. DocAgentPage.tsx hiển thị chi tiết Session cho User

---

## Kịch bản 5: Cập nhật Session

1. User click icon update
2. Header.tsx gọi tới SessionUpdateDialog.tsx
3. SessionUpdateDialog.tsx hiển thị dialog với thông tin hiện tại cho User
4. User nhập thông tin cập nhật và click "Lưu phiên"
5. SessionUpdateDialog.tsx gọi tới hàm validate()
6. Hàm validate() kiểm tra dữ liệu hợp lệ
7. SessionUpdateDialog.tsx gọi tới hàm handleUpdateSession()
8. Hàm handleUpdateSession() gọi tới lớp DocAgentService
9. Lớp DocAgentService gọi tới hàm updateSession()
10. Hàm updateSession() gửi yêu cầu với phương thức PUT đến endpoint "/session/{session_id}" tới session_routes.py phía backend thông qua RESTFul API
11. Lớp session_routes.py gọi hàm update_session()
12. Hàm update_session() gọi đến lớp SessionService để xử lý logic nghiệp vụ
13. Lớp SessionService sẽ gọi đến hàm update_session()
14. Hàm update_session() gọi tới lớp Session để cập nhật thực thể
15. Lớp Session đóng gói thông tin thực thể đã cập nhật
16. Lớp Session trả kết quả về cho hàm update_session()
17. Hàm update_session() trả kết quả về cho lớp session_routes.py
18. Lớp session_routes.py trả về dữ liệu cho hàm updateSession() phía Frontend
19. Hàm updateSession() trả kết quả cho Header.tsx
20. Header.tsx hiển thị thông báo cập nhật thành công cho User
