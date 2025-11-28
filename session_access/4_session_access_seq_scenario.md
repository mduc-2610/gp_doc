# Sequence Scenario - Quản lý Session Access

## 1. Chia sẻ Session với User cụ thể

1. User click "Chia sẻ" tại Header
2. Header.tsx gọi SessionShareDialog.tsx
3. SessionShareDialog.tsx hiển thị dialog cho User
4. User click tab "Chia sẻ"
5. SessionShareDialog.tsx gọi hàm loadShareData()
6. SessionShareDialog.tsx gọi AuthApi trong loop để lấy thông tin User
7. AuthApi gọi hàm getUserById()
8. AuthApi trả về thông tin User cho SessionShareDialog.tsx
9. SessionShareDialog.tsx gọi DocAgentService
10. DocAgentService gọi hàm getSessionGrantsBySession()
11. DocAgentService gửi yêu cầu GET "/session-access/grant/by-session/{session_id}" đến session_access_routes.py
12. session_access_routes.py gọi SessionAccessService.get_session_access_grants_by_session()
13. SessionAccessService xử lý yêu cầu và gọi chính nó
14. SessionAccessService gọi SessionAccessGrant
15. SessionAccessGrant khởi tạo đối tượng SessionAccessGrant()
16. SessionAccessGrant trả về dữ liệu cho SessionAccessService
17. SessionAccessService trả về danh sách grant cho session_access_routes.py
18. session_access_routes.py trả về kết quả cho DocAgentService
19. DocAgentService gọi hàm getSessionLink()
20. DocAgentService gửi yêu cầu GET "/session-access/link/by-session/{session_id}" đến session_access_routes.py
21. session_access_routes.py gọi SessionAccessService.get_session_access_link_by_session()
22. SessionAccessService xử lý yêu cầu và gọi chính nó
23. SessionAccessService gọi SessionAccessLink
24. SessionAccessLink khởi tạo đối tượng SessionAccessLink()
25. SessionAccessLink trả về dữ liệu cho SessionAccessService
26. SessionAccessService trả về link cho session_access_routes.py
27. session_access_routes.py trả về kết quả cho DocAgentService
28. DocAgentService hiển thị dữ liệu cho User
29. User nhập keyword tìm kiếm User
30. SessionShareDialog.tsx gọi SearchDialog.tsx
31. SearchDialog.tsx gọi AuthApi
32. AuthApi gọi hàm searchUsersByName()
33. AuthApi trả về danh sách User cho SearchDialog.tsx
34. SearchDialog.tsx hiển thị kết quả tìm kiếm cho User
35. User chọn User và click "Thêm"
36. SessionShareDialog.tsx gọi hàm validate()
37. SessionShareDialog.tsx gọi hàm handleAddUser()
38. SessionShareDialog.tsx gọi DocAgentService
39. DocAgentService gọi hàm createInvite()
40. DocAgentService gửi yêu cầu POST "/session-access/invite" đến session_access_routes.py
41. session_access_routes.py gọi SessionAccessService.create_session_access_invite()
42. SessionAccessService xử lý yêu cầu và gọi chính nó
43. SessionAccessService gọi SessionAccessGrant
44. SessionAccessGrant khởi tạo đối tượng SessionAccessGrant()
45. SessionAccessGrant trả về dữ liệu cho SessionAccessService
46. SessionAccessService trả về invite cho session_access_routes.py
47. session_access_routes.py trả về kết quả cho DocAgentService
48. DocAgentService trả về kết quả cho SessionShareDialog.tsx
49. SessionShareDialog.tsx hiển thị thông báo cho User

---

## 2. Cập nhật quyền truy cập

1. User chọn quyền từ dropdown của User
2. SessionShareDialog.tsx gọi hàm handleUpdateSessionGrant()
3. SessionShareDialog.tsx gọi DocAgentService
4. DocAgentService gọi hàm updateSessionGrant()
5. DocAgentService gửi yêu cầu PUT "/session-access/grant/{grant_id}" đến session_access_routes.py
6. session_access_routes.py gọi SessionAccessService.update_session_access_grant()
7. SessionAccessService xử lý yêu cầu và gọi chính nó
8. SessionAccessService gọi SessionAccessGrant
9. SessionAccessGrant khởi tạo đối tượng SessionAccessGrant()
10. SessionAccessGrant trả về dữ liệu cho SessionAccessService
11. SessionAccessService trả về grant đã cập nhật cho session_access_routes.py
12. session_access_routes.py trả về kết quả cho DocAgentService
13. DocAgentService trả về kết quả cho SessionShareDialog.tsx
14. SessionShareDialog.tsx hiển thị thông báo cho User

---

## 3. Thu hồi quyền truy cập

1. User click "Xóa" từ dropdown của User
2. SessionShareDialog.tsx gọi hàm handleRemoveUser()
3. SessionShareDialog.tsx gọi DocAgentService
4. DocAgentService gọi hàm revokeSessionGrant()
5. DocAgentService gửi yêu cầu DELETE "/session-access/grant/{grant_id}" đến session_access_routes.py
6. session_access_routes.py gọi SessionAccessService.revoke_session_access_grant()
7. SessionAccessService xử lý yêu cầu và gọi chính nó
8. SessionAccessService gọi MessageResponse
9. MessageResponse khởi tạo đối tượng MessageResponse()
10. MessageResponse trả về thông báo cho SessionAccessService
11. SessionAccessService trả về kết quả cho session_access_routes.py
12. session_access_routes.py trả về thông báo thành công cho DocAgentService
13. DocAgentService trả về kết quả cho SessionShareDialog.tsx
14. SessionShareDialog.tsx hiển thị thông báo cho User

---

## 4. Quản lý liên kết chia sẻ

1. User chọn "Bật kỳ ai có liên kết" trong dropdown
2. SessionShareDialog.tsx gọi hàm handleLinkUpdate()
3. SessionShareDialog.tsx gọi DocAgentService
4. DocAgentService gọi hàm updateSessionLink()
5. DocAgentService gửi yêu cầu PUT "/session-access/link/{link_id}" đến session_access_routes.py
6. session_access_routes.py gọi SessionAccessService.update_session_access_link()
7. SessionAccessService xử lý yêu cầu và gọi chính nó
8. SessionAccessService gọi SessionAccessLink
9. SessionAccessLink khởi tạo đối tượng SessionAccessLink()
10. SessionAccessLink trả về dữ liệu cho SessionAccessService
11. SessionAccessService trả về link đã cập nhật cho session_access_routes.py
12. session_access_routes.py trả về kết quả cho DocAgentService
13. DocAgentService hiển thị thông báo cho User
14. User chọn quyền liên kết dropdown
15. SessionShareDialog.tsx gọi hàm handleLinkUpdate()
16. SessionShareDialog.tsx gọi DocAgentService
17. DocAgentService gọi hàm updateSessionLink()
18. DocAgentService gửi yêu cầu PUT "/session-access/link/{link_id}" đến session_access_routes.py
19. session_access_routes.py gọi SessionAccessService.update_session_access_link()
20. SessionAccessService xử lý yêu cầu và gọi chính nó
21. SessionAccessService gọi SessionAccessLink
22. SessionAccessLink khởi tạo đối tượng SessionAccessLink()
23. SessionAccessLink trả về dữ liệu cho SessionAccessService
24. SessionAccessService trả về link đã cập nhật cho session_access_routes.py
25. session_access_routes.py trả về kết quả cho DocAgentService
26. DocAgentService trả về kết quả cho SessionShareDialog.tsx
27. SessionShareDialog.tsx hiển thị thông báo cho User

---

## 5. Chấp nhận yêu cầu truy cập

1. User chọn quyền từ dropdown và click "Cấp quyền"
2. SessionShareDialog.tsx gọi hàm handleAcceptRequest()
3. SessionShareDialog.tsx gọi DocAgentService
4. DocAgentService gọi hàm acceptSessionRequest()
5. DocAgentService gửi yêu cầu POST "/session-access/request/accept/{request_id}" đến session_access_routes.py
6. session_access_routes.py gọi SessionAccessService.accept_session_access_request()
7. SessionAccessService xử lý yêu cầu và gọi chính nó
8. SessionAccessService gọi SessionAccessRequest
9. SessionAccessRequest khởi tạo đối tượng SessionAccessRequest()
10. SessionAccessRequest trả về dữ liệu cho SessionAccessService
11. SessionAccessService trả về request đã accept cho session_access_routes.py
12. session_access_routes.py trả về kết quả cho DocAgentService
13. DocAgentService trả về kết quả cho SessionShareDialog.tsx
14. SessionShareDialog.tsx hiển thị thông báo cho User

---

## 6. Từ chối yêu cầu truy cập

1. User click "Từ chối" request to User
2. SessionShareDialog.tsx gọi hàm handleRejectRequest()
3. SessionShareDialog.tsx gọi DocAgentService
4. DocAgentService gọi hàm rejectSessionRequest()
5. DocAgentService gửi yêu cầu DELETE "/session-access/request/reject/{request_id}" đến session_access_routes.py
6. session_access_routes.py gọi SessionAccessService.reject_session_access_request()
7. SessionAccessService xử lý yêu cầu và gọi chính nó
8. SessionAccessService gọi SessionAccessRequest
9. SessionAccessRequest khởi tạo đối tượng SessionAccessRequest()
10. SessionAccessRequest trả về dữ liệu cho SessionAccessService
11. SessionAccessService trả về kết quả cho session_access_routes.py
12. session_access_routes.py trả về thông báo cho DocAgentService
13. DocAgentService trả về kết quả cho SessionShareDialog.tsx
14. SessionShareDialog.tsx hiển thị thông báo cho User

---

## 7. Yêu cầu truy cập Session

1. User xem chi tiết Session
2. DocAgentPage.tsx gọi RequestAccessState.tsx
3. RequestAccessState.tsx gọi hàm loadUserSessionRequest()
4. RequestAccessState.tsx gọi DocAgentService
5. DocAgentService gọi hàm getUserSessionRequest()
6. DocAgentService gửi yêu cầu GET "/session-access/request/by-user-session/{user_id}/{session_id}" đến session_access_routes.py
7. session_access_routes.py gọi SessionAccessService.get_user_session_access_request()
8. SessionAccessService xử lý yêu cầu và gọi chính nó
9. SessionAccessService gọi SessionAccessRequest
10. SessionAccessRequest khởi tạo đối tượng SessionAccessRequest()
11. SessionAccessRequest trả về dữ liệu cho SessionAccessService
12. SessionAccessService trả về request cho session_access_routes.py
13. session_access_routes.py trả về kết quả cho DocAgentService
14. DocAgentService hiển thị dữ liệu cho User
15. Nếu danh sách session access không null, User gọi RequestAccessState.tsx
16. RequestAccessState.tsx trả về cho User
17. User click "Tạo" hoặc "Yêu cầu"
18. RequestAccessState.tsx gọi hàm handleCreateRequest()
19. RequestAccessState.tsx gọi DocAgentService
20. DocAgentService gọi hàm createSessionRequest()
21. DocAgentService gửi yêu cầu POST "/session-access/request" đến session_access_routes.py
22. session_access_routes.py gọi SessionAccessService.create_session_access_request()
23. SessionAccessService xử lý yêu cầu và gọi chính nó
24. SessionAccessService gọi SessionAccessRequest
25. SessionAccessRequest khởi tạo đối tượng SessionAccessRequest()
26. SessionAccessRequest trả về dữ liệu cho SessionAccessService
27. SessionAccessService trả về request mới cho session_access_routes.py
28. session_access_routes.py trả về kết quả cho DocAgentService
29. DocAgentService trả về kết quả cho DocAgentPage.tsx
30. DocAgentPage.tsx hiển thị thông báo cho User

---

## 8. Hủy yêu cầu truy cập

1. User xem chi tiết Session
2. DocAgentPage.tsx gọi RequestAccessState.tsx
3. RequestAccessState.tsx gọi hàm loadUserSessionRequest()
4. RequestAccessState.tsx gọi DocAgentService
5. DocAgentService gọi hàm getUserSessionRequest()
6. DocAgentService gửi yêu cầu GET "/session-access/request/by-user-session/{user_id}/{session_id}" đến session_access_routes.py
7. session_access_routes.py gọi SessionAccessService.get_user_session_access_request()
8. SessionAccessService xử lý yêu cầu và gọi chính nó
9. SessionAccessService gọi SessionAccessRequest
10. SessionAccessRequest khởi tạo đối tượng SessionAccessRequest()
11. SessionAccessRequest trả về dữ liệu cho SessionAccessService
12. SessionAccessService trả về request cho session_access_routes.py
13. session_access_routes.py trả về kết quả cho DocAgentService
14. DocAgentService hiển thị dữ liệu cho User
15. Nếu danh sách session access không null, User gọi RequestAccessState.tsx
16. RequestAccessState.tsx trả về cho User
17. User click "Từ chối"
18. RequestAccessState.tsx gọi hàm handleDeleteRequest()
19. RequestAccessState.tsx gọi DocAgentService
20. DocAgentService gọi hàm deleteRequest()
21. DocAgentService gửi yêu cầu DELETE "/session-access/request/{request_id}" đến session_access_routes.py
22. session_access_routes.py gọi SessionAccessService.delete_session_access_request()
23. SessionAccessService xử lý yêu cầu và gọi chính nó
24. SessionAccessService gọi SessionAccessRequest
25. SessionAccessRequest khởi tạo đối tượng SessionAccessRequest()
26. SessionAccessRequest trả về dữ liệu cho SessionAccessService
27. SessionAccessService trả về kết quả cho session_access_routes.py
28. session_access_routes.py trả về thông báo cho DocAgentService
29. DocAgentService trả về kết quả cho DocAgentPage.tsx
30. DocAgentPage.tsx hiển thị thông báo cho User

---

## 9. Xem danh sách yêu cầu truy cập

1. User click "Chia sẻ" tại Header
2. Header.tsx gọi SessionShareDialog.tsx
3. SessionShareDialog.tsx hiển thị dialog cho User
4. User click tab "Yêu cầu"
5. SessionShareDialog.tsx gọi hàm loadSessionRequests()
6. SessionShareDialog.tsx gọi DocAgentService
7. DocAgentService gọi hàm getSessionRequestsBySession()
8. DocAgentService gửi yêu cầu GET "/session-access/grant/by-session/{session_id}" đến session_access_routes.py
9. session_access_routes.py gọi SessionAccessService.get_session_access_requests_by_session()
10. SessionAccessService xử lý yêu cầu và gọi chính nó
11. SessionAccessService gọi SessionAccessRequest
12. SessionAccessRequest khởi tạo đối tượng SessionAccessRequest()
13. SessionAccessRequest trả về dữ liệu cho SessionAccessService
14. SessionAccessService trả về danh sách request cho session_access_routes.py
15. session_access_routes.py trả về kết quả cho DocAgentService
16. DocAgentService gọi AuthApi trong loop để lấy thông tin User
17. AuthApi gọi hàm getUserById()
18. AuthApi trả về thông tin User cho DocAgentService
19. DocAgentService hiển thị danh sách yêu cầu cho User
