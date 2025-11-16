```markdown
# Sequence Scenario - Quản lý Session Access

## Kịch bản 1: Chia sẻ Session với User cụ thể

1. Tại giao diện Session chi tiết, Manager click nút "Share" trên Header
2. Header.tsx mở SessionShareModal với prop `open=true`
3. SessionShareModal.tsx khởi tạo và hiển thị modal
4. Component gọi `DocAgentService.getSessionGrants(sessionId)` để lấy danh sách User đã được chia sẻ
5. DocAgentService gửi request GET tới endpoint `/session-access/grant/by-session/{sessionId}`
6. Backend route nhận request và SessionAccessService được gọi `get_session_access_grants_by_session()`
7. SessionAccessService truy vấn cơ sở dữ liệu và trả danh sách SessionAccessGrant
8. session_access_routes.py trả về danh sách SessionAccessGrantResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<SessionAccessGrant[]>
10. SessionShareModal nhận ServiceResult và hiển thị danh sách User đã được chia sẻ
11. Manager click nút "Add people"
12. Component hiển thị UserSearchDialog
13. Manager tìm kiếm User, chọn User cần chia sẻ
14. Manager chọn mức quyền (Reader/Contributor/Manager) từ dropdown
15. Manager click "Add" để xác nhận
16. SessionShareModal gọi `DocAgentService.createInvite()` với thông tin invited_to và invited_level
17. DocAgentService gửi POST request tới `/session-access/invite`
18. Backend route nhận request và SessionAccessService được gọi `create_session_access_invite(auto_grant=true)`
19. SessionAccessService kiểm tra grant/invite hiện tại
20. SessionAccessService tạo SessionAccessInvite mới
21. SessionAccessService tự động tạo SessionAccessGrant cho User (vì auto_grant=true)
22. SessionAccessService cập nhật invite.granted_access_id
23. SessionAccessService commit transaction
24. SessionAccessService gọi WSSessionAccessManager để broadcast event PERMISSION_GRANTED
25. WSSessionAccessManager gửi WebSocket message đến tất cả connections của Session
26. SessionAccessService trả SessionAccessInvite
27. session_access_routes.py trả về SessionAccessInviteResponse JSON
28. DocAgentService nhận response và đóng gói với ServiceResult<SessionAccessInvite>
29. SessionShareModal nhận ServiceResult và reload danh sách grant
30. Component re-render và hiển thị danh sách được cập nhật
31. Toast success notification hiển thị "Chia sẻ thành công"
32. User được chia sẻ nhận WebSocket event PERMISSION_GRANTED
33. DocAgentPage của User xử lý event trong handler `onPermissionGranted`
34. Toast hiển thị "Bạn đã được cấp quyền truy cập"
35. Component set permissionLoading=true để reload permission
36. Component reload Session data và hiển thị với quyền mới

---

## Kịch bản 2: Cập nhật quyền truy cập

1. Manager mở SessionShareModal (đã load danh sách User)
2. Manager click vào dropdown quyền của User cần cập nhật
3. Manager chọn mức quyền mới (Reader/Contributor/Manager)
4. SessionShareModal gọi `DocAgentService.updateGrant(grantId, {access_level})`
5. DocAgentService gửi PUT request tới `/session-access/grant/{grantId}`
6. Backend route nhận request và SessionAccessService được gọi `update_session_access_grant()`
7. SessionAccessService lấy grant hiện tại từ cơ sở dữ liệu
8. SessionAccessService cập nhật access_level
9. SessionAccessService commit transaction
10. SessionAccessService gọi WSSessionAccessManager để broadcast event PERMISSION_CHANGED
11. WSSessionAccessManager gửi WebSocket message đến tất cả connections
12. SessionAccessService trả SessionAccessGrant đã cập nhật
13. session_access_routes.py trả về SessionAccessGrantResponse JSON
14. DocAgentService nhận response và đóng gói với ServiceResult<SessionAccessGrant>
15. SessionShareModal cập nhật grant trong danh sách
16. Component re-render và hiển thị quyền mới
17. Toast success notification hiển thị "Cập nhật quyền thành công"
18. User bị thay đổi quyền nhận WebSocket event PERMISSION_CHANGED
19. DocAgentPage xử lý event trong handler `onPermissionChanged`
20. Toast hiển thị "Quyền của bạn đã được cập nhật"
21. Component set permissionLoading=true để reload permission
22. Component reload và hiển thị với quyền mới

---

## Kịch bản 3: Thu hồi quyền truy cập

1. Manager mở SessionShareModal và xem danh sách User
2. Manager click icon "X" bên cạnh User cần thu hồi quyền
3. SessionShareModal gọi `DocAgentService.revokeGrant(grantId)`
4. DocAgentService gửi DELETE request tới `/session-access/grant/revoke/{grantId}`
5. Backend route nhận request và SessionAccessService được gọi `revoke_session_access_grant()`
6. SessionAccessService lấy grant từ cơ sở dữ liệu
7. SessionAccessService set is_active = false
8. SessionAccessService commit transaction
9. SessionAccessService gọi WSSessionAccessManager để broadcast event PERMISSION_REVOKED
10. WSSessionAccessManager gửi WebSocket message đến tất cả connections
11. SessionAccessService trả SessionAccessGrant (với is_active=false)
12. session_access_routes.py trả về MessageResponse JSON
13. DocAgentService nhận response và đóng gói với ServiceResult<MessageResponse>
14. SessionShareModal xóa grant khỏi danh sách hiển thị
15. Component re-render
16. Toast success notification hiển thị "Thu hồi quyền thành công"
17. User bị thu hồi quyền nhận WebSocket event PERMISSION_REVOKED
18. DocAgentPage xử lý event trong handler `onPermissionRevoked`
19. Toast hiển thị "Quyền truy cập của bạn đã bị thu hồi"
20. Component set permissionLoading=true
21. Permission hook phát hiện User không còn quyền
22. Component chuyển sang hiển thị RequestAccessState
23. User thấy UI yêu cầu truy cập hoặc bị đẩy ra khỏi Session

---

## Kịch bản 4: Yêu cầu truy cập Session

1. User truy cập Session URL mà không có quyền truy cập
2. DocAgentPage component khởi tạo
3. Component check permission qua hook `useSessionPermission`
4. Hook phát hiện User không có quyền (permission === null)
5. Component hiển thị RequestAccessState thay vì nội dung Session
6. RequestAccessState khởi tạo và gọi `checkExistingRequest()`
7. Component gọi `DocAgentService.getUserSessionRequest(sessionId, userId)`
8. DocAgentService gửi GET request tới `/session-access/request/by-user-session/{sessionId}/{userId}`
9. Backend route nhận request và SessionAccessService được gọi `get_session_access_request_by_user_session()`
10. SessionAccessService truy vấn request với status=PENDING
11. SessionAccessService trả SessionAccessRequest nếu có, hoặc null
12. session_access_routes.py trả về SessionAccessRequestResponse hoặc null
13. DocAgentService nhận response và đóng gói với ServiceResult
14. RequestAccessState nhận ServiceResult và set state existingRequest
15. Component hiển thị nút "Request Access" (nếu chưa có request)
16. User click "Request Access"
17. RequestAccessState gọi `DocAgentService.createRequest()` với session_id và requested_by
18. DocAgentService gửi POST request tới `/session-access/request`
19. Backend route nhận request và SessionAccessService được gọi `create_session_access_request()`
20. SessionAccessService kiểm tra request/grant hiện tại
21. SessionAccessService tạo SessionAccessRequest mới với status=PENDING
22. SessionAccessService commit transaction
23. SessionAccessService gọi WSSessionAccessManager để broadcast event REQUEST_CREATED
24. WSSessionAccessManager gửi WebSocket message đến Manager
25. SessionAccessService trả SessionAccessRequest
26. session_access_routes.py trả về SessionAccessRequestResponse JSON
27. DocAgentService nhận response và đóng gói với ServiceResult<SessionAccessRequest>
28. RequestAccessState nhận ServiceResult và update existingRequest
29. Component re-render và hiển thị trạng thái "Pending"
30. Toast success notification hiển thị "Yêu cầu đã được gửi"
31. UI hiển thị nút "Cancel Request"
32. Manager (có WebSocket connection) nhận event REQUEST_CREATED
33. DocAgentPage của Manager xử lý event trong handler `onRequestCreated`
34. Toast hiển thị "Có yêu cầu truy cập mới"
35. Component increment requestTabReloadKey
36. SessionShareModal (nếu đang mở) tự động reload request list

---

## Kịch bản 5: Chấp nhận yêu cầu truy cập

1. Manager mở SessionShareModal
2. Manager click tab "Request"
3. useEffect hook trigger khi activeTab thay đổi
4. Component gọi `DocAgentService.getSessionRequests(sessionId)`
5. DocAgentService gửi GET request tới `/session-access/request/by-session/{sessionId}`
6. Backend route nhận request và SessionAccessService được gọi `get_session_access_requests_by_session()`
7. SessionAccessService truy vấn requests với status=PENDING
8. SessionAccessService trả danh sách SessionAccessRequest
9. session_access_routes.py trả về danh sách SessionAccessRequestResponse JSON
10. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult
11. SessionShareModal nhận ServiceResult và hiển thị danh sách request
12. Manager xem thông tin User yêu cầu
13. Manager chọn mức quyền sẽ cấp (Reader/Contributor/Manager)
14. Manager click "Approve"
15. SessionShareModal gọi `DocAgentService.acceptRequest(requestId, {requested_level})`
16. DocAgentService gửi POST request tới `/session-access/request/accept/{requestId}`
17. Backend route nhận request và SessionAccessService được gọi `accept_session_access_request()`
18. SessionAccessService lấy request từ cơ sở dữ liệu
19. SessionAccessService cập nhật status = APPROVED
20. SessionAccessService tạo SessionAccessGrant mới cho User
21. SessionAccessService cập nhật request.granted_access_id
22. SessionAccessService commit transaction
23. SessionAccessService gọi WSSessionAccessManager để broadcast event PERMISSION_GRANTED
24. WSSessionAccessManager gửi WebSocket message đến User
25. SessionAccessService trả SessionAccessRequest đã cập nhật
26. session_access_routes.py trả về SessionAccessRequestResponse JSON
27. DocAgentService nhận response và đóng gói với ServiceResult
28. SessionShareModal xóa request khỏi danh sách
29. Component re-render
30. Toast success notification hiển thị "Đã chấp nhận yêu cầu"
31. User nhận WebSocket event PERMISSION_GRANTED
32. DocAgentPage xử lý event trong handler `onPermissionGranted`
33. Toast hiển thị "Bạn đã được cấp quyền truy cập"
34. Component set permissionLoading=true
35. Permission hook reload và phát hiện User đã có quyền
36. Component reload Session data
37. RequestAccessState bị unmount
38. Session content được hiển thị với quyền mới

---

## Kịch bản 6: Từ chối yêu cầu truy cập

1. Manager mở SessionShareModal và xem tab "Request"
2. Manager xem danh sách request (đã load như kịch bản 5, bước 1-11)
3. Manager click "Deny" trên một request
4. SessionShareModal gọi `DocAgentService.rejectRequest(requestId)`
5. DocAgentService gửi POST request tới `/session-access/request/reject/{requestId}`
6. Backend route nhận request và SessionAccessService được gọi `reject_session_access_request()`
7. SessionAccessService lấy request từ cơ sở dữ liệu
8. SessionAccessService cập nhật status = REJECTED
9. SessionAccessService commit transaction
10. SessionAccessService gọi WSSessionAccessManager để broadcast event REQUEST_DELETED
11. WSSessionAccessManager gửi WebSocket message
12. SessionAccessService trả MessageResponse
13. session_access_routes.py trả về MessageResponse JSON
14. DocAgentService nhận response và đóng gói với ServiceResult
15. SessionShareModal xóa request khỏi danh sách
16. Component re-render
17. Toast success notification hiển thị "Đã từ chối yêu cầu"
18. User nhận WebSocket event REQUEST_DELETED
19. DocAgentPage không xử lý gì (User không nhận thông báo cụ thể)
20. RequestAccessState của User vẫn hiển thị trạng thái pending
21. User vẫn có thể gửi request mới

---

## Kịch bản 7: Hủy yêu cầu truy cập

1. User đang xem RequestAccessState với trạng thái "Pending"
2. User click nút "Cancel Request"
3. RequestAccessState gọi `DocAgentService.deleteRequest(requestId, sessionId)`
4. DocAgentService gửi DELETE request tới `/session-access/request/{requestId}`
5. Backend route nhận request và SessionAccessService được gọi `delete_session_access_request()`
6. SessionAccessService lấy request từ cơ sở dữ liệu
7. SessionAccessService xóa request
8. SessionAccessService commit transaction
9. SessionAccessService gọi WSSessionAccessManager để broadcast event REQUEST_DELETED
10. WSSessionAccessManager gửi WebSocket message đến Manager
11. SessionAccessService trả MessageResponse
12. session_access_routes.py trả về MessageResponse JSON
13. DocAgentService nhận response và đóng gói với ServiceResult
14. RequestAccessState set existingRequest = null
15. Component re-render và hiển thị nút "Request Access"
16. Toast success notification hiển thị "Đã hủy yêu cầu"
17. Manager (có WebSocket connection) nhận event REQUEST_DELETED
18. DocAgentPage của Manager xử lý event trong handler `onRequestDeleted`
19. Component increment requestTabReloadKey
20. SessionShareModal (nếu đang mở) tự động reload request list
21. Request bị xóa khỏi danh sách của Manager

---

## Kịch bản 8: Bật/tắt liên kết chia sẻ công khai

1. Manager mở SessionShareModal
2. useEffect hook trigger khi modal open
3. Component gọi `DocAgentService.getSessionLink(sessionId)`
4. DocAgentService gửi GET request tới `/session-access/link/by-session/{sessionId}`
5. Backend route nhận request và SessionAccessService được gọi `get_session_access_link_by_session()`
6. SessionAccessService truy vấn SessionAccessLink
7. Nếu không tồn tại, tạo link mới với is_active=false
8. SessionAccessService trả SessionAccessLink
9. session_access_routes.py trả về SessionAccessLinkResponse JSON
10. DocAgentService nhận response và đóng gói với ServiceResult
11. SessionShareModal set state generalAccess
12. Component hiển thị toggle switch và dropdown quyền
13. Manager toggle switch để bật/tắt link
14. SessionShareModal gọi `DocAgentService.updateSessionLink(linkId, {is_active})`
15. DocAgentService gửi PUT request tới `/session-access/link/{linkId}`
16. Backend route nhận request và SessionAccessService được gọi `update_session_access_link()`
17. SessionAccessService lấy link từ cơ sở dữ liệu
18. SessionAccessService cập nhật is_active
19. SessionAccessService commit transaction
20. SessionAccessService gọi WSSessionAccessManager để broadcast event LINK_UPDATED
21. WSSessionAccessManager gửi WebSocket message
22. SessionAccessService trả SessionAccessLink đã cập nhật
23. session_access_routes.py trả về SessionAccessLinkResponse JSON
24. DocAgentService nhận response và đóng gói với ServiceResult
25. SessionShareModal cập nhật generalAccess state
26. Component re-render với toggle switch cập nhật
27. Toast success notification hiển thị "Link đã được bật" hoặc "Link đã được tắt"
28. Các User có WebSocket connection nhận event LINK_UPDATED
29. Component có thể reload link config nếu cần

---

## Kịch bản 9: Thay đổi quyền mặc định của liên kết chia sẻ

1. Manager mở SessionShareModal (đã load link config)
2. Manager click dropdown quyền của General Access
3. Manager chọn quyền mới (Reader/Contributor/Manager)
4. SessionShareModal gọi `DocAgentService.updateSessionLink(linkId, {access_level})`
5. DocAgentService gửi PUT request tới `/session-access/link/{linkId}`
6. Backend route nhận request và SessionAccessService được gọi `update_session_access_link()`
7. SessionAccessService lấy link từ cơ sở dữ liệu
8. SessionAccessService cập nhật access_level
9. SessionAccessService commit transaction
10. SessionAccessService gọi WSSessionAccessManager để broadcast event LINK_UPDATED
11. WSSessionAccessManager gửi WebSocket message
12. SessionAccessService trả SessionAccessLink đã cập nhật
13. session_access_routes.py trả về SessionAccessLinkResponse JSON
14. DocAgentService nhận response và đóng gói với ServiceResult
15. SessionShareModal cập nhật generalAccess state
16. Component re-render với dropdown hiển thị quyền mới
17. Toast success notification hiển thị "Quyền mặc định đã được cập nhật"

---

## Kịch bản 10: Copy liên kết chia sẻ

1. Manager mở SessionShareModal
2. Manager click nút "Copy link"
3. SessionShareModal gọi `handleCopyLink()`
4. Component lấy URL hiện tại của Session
5. Component sử dụng navigator.clipboard.writeText() để copy URL
6. Toast success notification hiển thị "Đã copy link"
7. Manager có thể paste link và chia sẻ cho User khác

---

## Kịch bản 11: Nhận cập nhật quyền realtime qua WebSocket

1. User/Manager truy cập Session
2. DocAgentPage component mount
3. useEffect hook trigger khi sessionConfirmed thay đổi
4. Component gọi `DocSocketService.connectSessionSocket(sessionId, userId, handlers)`
5. DocSocketService tạo WebSocket connection tới ws://...session/ws/{sessionId}?user_id={userId}
6. WebSocket connection established
7. Server (session_routes.py) nhận connection
8. Server gọi `ws_session_access_manager.connect(session_id, websocket, user_id)`
9. WSSessionAccessManager lưu connection vào active_connections dict
10. DocSocketService gửi message {"action": "subscribe"} qua WebSocket
11. Server nhận message và log
12. DocSocketService trả WebSocket instance cho component
13. Component lưu WebSocket vào state sessionSocket
14. Component gọi `DocSocketService.startSessionKeepAlive(ws, 30000)`
15. Interval bắt đầu gửi ping message mỗi 30 giây
16. User/Manager đang xem Session
17. **Khi có sự kiện từ Manager khác:**
    - Manager thực hiện thao tác (share, update, revoke, etc.)
    - SessionAccessService gọi WSSessionAccessManager.broadcast_xxx()
    - WSSessionAccessManager lấy danh sách connections của session
    - WSSessionAccessManager loop qua các connections
    - WSSessionAccessManager skip connection của actor (exclude_user_id)
    - WSSessionAccessManager gửi WsSessionAccessEnvelope qua mỗi WebSocket
    - User nhận message trong DocAgentPage
18. **Component xử lý message theo event type:**
    - **PERMISSION_GRANTED:**
      - Handler `onPermissionGranted` được gọi
      - Toast hiển thị "Bạn đã được cấp quyền truy cập"
      - set permissionLoading(true) để trigger reload
      - Permission hook reload grant từ API
      - Component reload Session data
    - **PERMISSION_CHANGED:**
      - Handler `onPermissionChanged` được gọi
      - Toast hiển thị "Quyền của bạn đã được cập nhật"
      - set permissionLoading(true)
      - Permission hook reload grant
    - **PERMISSION_REVOKED:**
      - Handler `onPermissionRevoked` được gọi
      - Toast hiển thị "Quyền truy cập của bạn đã bị thu hồi"
      - set permissionLoading(true)
      - Permission hook phát hiện không còn grant
      - Component chuyển sang RequestAccessState
    - **REQUEST_CREATED:** (chỉ Manager nhận)
      - Handler `onRequestCreated` được gọi
      - Toast hiển thị "Có yêu cầu truy cập mới"
      - increment requestTabReloadKey
      - SessionShareModal reload request list
    - **REQUEST_DELETED:**
      - increment requestTabReloadKey
      - SessionShareModal reload request list
    - **LINK_UPDATED:**
      - Handler `onLinkUpdated` được gọi
      - SessionShareModal reload link config (nếu đang mở)
19. User/Manager rời khỏi Session (navigate away)
20. Component unmount, useEffect cleanup trigger
21. Component gọi `DocSocketService.closeSessionSocket(ws)`
22. DocSocketService gọi ws.close()
23. WebSocket connection closed
24. Server nhận close event
25. Server gọi `ws_session_access_manager.disconnect(websocket)`
26. WSSessionAccessManager xóa connection khỏi active_connections
27. Interval keep-alive bị clear

```