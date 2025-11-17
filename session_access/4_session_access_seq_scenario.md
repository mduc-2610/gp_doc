# Sequence Scenario - Quản lý Session Access

## Kịch bản 1: Chia sẻ Session với User cụ thể

1. Tại giao diện Session chi tiết, User click nút "Share" trên Header
2. Header.tsx mở SessionShareModal với prop `open=true`
3. SessionShareModal.tsx khởi tạo, hiển thị modal và gọi `DocAgentService.getSessionGrants()` để lấy danh sách User đã được chia sẻ
4. DocAgentService gửi request GET tới endpoint `/session-access/grant/by-session/{sessionId}`
5. Backend route nhận request, SessionAccessService được gọi `get_session_access_grants_by_session()`, truy vấn cơ sở dữ liệu và trả danh sách SessionAccessGrant
6. session_access_routes.py trả về danh sách SessionAccessGrantResponse JSON
7. DocAgentService nhận danh sách từ response, đóng gói với ServiceResult<SessionAccessGrant[]> và SessionShareModal hiển thị danh sách User đã được chia sẻ
8. User click nút "Add people"
9. Component hiển thị SearchDialog
10. User tìm kiếm, chọn User cần chia sẻ, chọn mức quyền (Reader/Contributor/Manager) và click "Add" để xác nhận
11. SessionShareModal gọi `DocAgentService.createInvite()` với thông tin invited_to và invited_level
12. DocAgentService gửi POST request tới `/session-access/invite`
13. Backend route nhận request, SessionAccessService được gọi `create_session_access_invite(auto_grant=true)`, kiểm tra grant/invite hiện tại, tạo SessionAccessInvite mới, tự động tạo SessionAccessGrant cho User, cập nhật invite.granted_access_id và commit transaction
14. SessionAccessService gọi WSSessionAccessManager để broadcast event PERMISSION_GRANTED
15. WSSessionAccessManager gửi WebSocket message đến tất cả connections của Session
16. SessionAccessService trả SessionAccessInvite
17. session_access_routes.py trả về SessionAccessInviteResponse JSON
18. DocAgentService nhận response, đóng gói với ServiceResult<SessionAccessInvite> và SessionShareModal reload danh sách grant
19. Component re-render, hiển thị danh sách được cập nhật và Toast success notification hiển thị "Chia sẻ thành công"
20. User được chia sẻ nhận WebSocket event PERMISSION_GRANTED, DocAgentPage xử lý event trong handler `onPermissionGranted`, hiển thị Toast "Bạn đã được cấp quyền truy cập", set permissionLoading=true để reload permission và hiển thị Session với quyền mới

---

## Kịch bản 2: Cập nhật quyền truy cập

1. User mở SessionShareModal (đã load danh sách User)
2. User click vào dropdown quyền của User cần cập nhật và chọn mức quyền mới (Reader/Contributor/Manager)
3. SessionShareModal gọi `DocAgentService.updateGrant()`
4. DocAgentService gửi PUT request tới `/session-access/grant/{grantId}`
5. Backend route nhận request, SessionAccessService được gọi `update_session_access_grant()`, lấy grant hiện tại từ cơ sở dữ liệu, cập nhật access_level và commit transaction
6. SessionAccessService gọi WSSessionAccessManager để broadcast event PERMISSION_CHANGED
7. WSSessionAccessManager gửi WebSocket message đến tất cả connections
8. SessionAccessService trả SessionAccessGrant đã cập nhật
9. session_access_routes.py trả về SessionAccessGrantResponse JSON
10. DocAgentService nhận response, đóng gói với ServiceResult<SessionAccessGrant> và SessionShareModal cập nhật grant trong danh sách
11. Component re-render, hiển thị quyền mới và Toast success notification hiển thị "Cập nhật quyền thành công"
12. User bị thay đổi quyền nhận WebSocket event PERMISSION_CHANGED, DocAgentPage xử lý event trong handler `onPermissionChanged`, hiển thị Toast "Quyền của bạn đã được cập nhật", set permissionLoading=true để reload permission và hiển thị với quyền mới

---

## Kịch bản 3: Thu hồi quyền truy cập

1. User mở SessionShareModal và xem danh sách User
2. User click icon "X" bên cạnh User cần thu hồi quyền
3. SessionShareModal gọi `DocAgentService.revokeGrant()`
4. DocAgentService gửi DELETE request tới `/session-access/grant/revoke/{grantId}`
5. Backend route nhận request, SessionAccessService được gọi `revoke_session_access_grant()`, lấy grant từ cơ sở dữ liệu, set is_active = false và commit transaction
6. SessionAccessService gọi WSSessionAccessManager để broadcast event PERMISSION_REVOKED
7. WSSessionAccessManager gửi WebSocket message đến tất cả connections
8. SessionAccessService trả SessionAccessGrant (với is_active=false)
9. session_access_routes.py trả về MessageResponse JSON
10. DocAgentService nhận response, đóng gói với ServiceResult<MessageResponse> và SessionShareModal xóa grant khỏi danh sách hiển thị
11. Component re-render và Toast success notification hiển thị "Thu hồi quyền thành công"
12. User bị thu hồi quyền nhận WebSocket event PERMISSION_REVOKED, DocAgentPage xử lý event trong handler `onPermissionRevoked`, hiển thị Toast "Quyền truy cập của bạn đã bị thu hồi", set permissionLoading=true, Permission hook phát hiện User không còn quyền, Component chuyển sang hiển thị RequestAccessState

---

## Kịch bản 4: Yêu cầu truy cập Session

1. User truy cập Session URL mà không có quyền truy cập
2. DocAgentPage component khởi tạo, check permission qua hook `useSessionPermission`, hook phát hiện User không có quyền (permission === null) và hiển thị RequestAccessState thay vì nội dung Session
3. RequestAccessState khởi tạo, gọi `checkExistingRequest()` và gọi `DocAgentService.getUserSessionRequest()`
4. DocAgentService gửi GET request tới `/session-access/request/by-user-session/{sessionId}/{userId}`
5. Backend route nhận request, SessionAccessService được gọi `get_session_access_request_by_user_session()`, truy vấn request với status=PENDING và trả SessionAccessRequest nếu có, hoặc null
6. session_access_routes.py trả về SessionAccessRequestResponse hoặc null
7. DocAgentService nhận response, đóng gói với ServiceResult và RequestAccessState set state existingRequest, hiển thị nút "Yêu cầu truy cập" (nếu chưa có request)
8. User click "Yêu cầu truy cập"
9. RequestAccessState gọi `DocAgentService.createRequest()` với session_id và requested_by
10. DocAgentService gửi POST request tới `/session-access/request`
11. Backend route nhận request, SessionAccessService được gọi `create_session_access_request()`, kiểm tra request/grant hiện tại, tạo SessionAccessRequest mới với status=PENDING và commit transaction
12. SessionAccessService gọi WSSessionAccessManager để broadcast event REQUEST_CREATED
13. WSSessionAccessManager gửi WebSocket message đến User có quyền Manager
14. SessionAccessService trả SessionAccessRequest
15. session_access_routes.py trả về SessionAccessRequestResponse JSON
16. DocAgentService nhận response, đóng gói với ServiceResult<SessionAccessRequest> và RequestAccessState update existingRequest
17. Component re-render, hiển thị trạng thái "Đang chờ", Toast success notification hiển thị "Yêu cầu đã được gửi" và UI hiển thị nút "Hủy yêu cầu"
18. User có quyền Manager (có WebSocket connection) nhận event REQUEST_CREATED, DocAgentPage xử lý event trong handler `onRequestCreated`, hiển thị Toast "Có yêu cầu truy cập mới", increment requestTabReloadKey và SessionShareModal (nếu đang mở) tự động reload request list

---

## Kịch bản 5: Chấp nhận yêu cầu truy cập

1. User mở SessionShareModal
2. User click tab "Yêu cầu"
3. useEffect hook trigger khi activeTab thay đổi và Component gọi `DocAgentService.getSessionRequests()`
4. DocAgentService gửi GET request tới `/session-access/request/by-session/{sessionId}`
5. Backend route nhận request, SessionAccessService được gọi `get_session_access_requests_by_session()`, truy vấn requests với status=PENDING và trả danh sách SessionAccessRequest
6. session_access_routes.py trả về danh sách SessionAccessRequestResponse JSON
7. DocAgentService nhận danh sách từ response, đóng gói với ServiceResult và SessionShareModal hiển thị danh sách request
8. User xem thông tin User yêu cầu, chọn mức quyền sẽ cấp (Reader/Contributor/Manager) và click "Approve"
9. SessionShareModal gọi `DocAgentService.acceptRequest()`
10. DocAgentService gửi POST request tới `/session-access/request/accept/{requestId}`
11. Backend route nhận request, SessionAccessService được gọi `accept_session_access_request()`, lấy request từ cơ sở dữ liệu, cập nhật status = APPROVED, tạo SessionAccessGrant mới cho User, cập nhật request.granted_access_id và commit transaction
12. SessionAccessService gọi WSSessionAccessManager để broadcast event PERMISSION_GRANTED
13. WSSessionAccessManager gửi WebSocket message đến User
14. SessionAccessService trả SessionAccessRequest đã cập nhật
15. session_access_routes.py trả về SessionAccessRequestResponse JSON
16. DocAgentService nhận response, đóng gói với ServiceResult và SessionShareModal xóa request khỏi danh sách
17. Component re-render và Toast success notification hiển thị "Đã chấp nhận yêu cầu"
18. User nhận WebSocket event PERMISSION_GRANTED, DocAgentPage xử lý event trong handler `onPermissionGranted`, hiển thị Toast "Bạn đã được cấp quyền truy cập", set permissionLoading=true, Permission hook reload và phát hiện User đã có quyền, Component reload Session data, RequestAccessState bị unmount và Session content được hiển thị với quyền mới

---

## Kịch bản 6: Từ chối yêu cầu truy cập

1. User mở SessionShareModal và xem tab "Request"
2. User xem danh sách request (đã load như kịch bản 5, bước 1-7)
3. User click "Deny" trên một request
4. SessionShareModal gọi `DocAgentService.rejectRequest()`
5. DocAgentService gửi POST request tới `/session-access/request/reject/{requestId}`
6. Backend route nhận request, SessionAccessService được gọi `reject_session_access_request()`, lấy request từ cơ sở dữ liệu, cập nhật status = REJECTED và commit transaction
7. SessionAccessService gọi WSSessionAccessManager để broadcast event REQUEST_DELETED
8. WSSessionAccessManager gửi WebSocket message
9. SessionAccessService trả MessageResponse
10. session_access_routes.py trả về MessageResponse JSON
11. DocAgentService nhận response, đóng gói với ServiceResult và SessionShareModal xóa request khỏi danh sách
12. Component re-render và Toast success notification hiển thị "Đã từ chối yêu cầu"

---

## Kịch bản 7: Hủy yêu cầu truy cập

1. User đang xem RequestAccessState với trạng thái "Đang chờ"
2. User click nút "Hủy yêu cầu"
3. RequestAccessState gọi `DocAgentService.deleteRequest()`
4. DocAgentService gửi DELETE request tới `/session-access/request/{requestId}`
5. Backend route nhận request, SessionAccessService được gọi `delete_session_access_request()`, lấy request từ cơ sở dữ liệu, xóa request và commit transaction
6. SessionAccessService gọi WSSessionAccessManager để broadcast event REQUEST_DELETED
7. WSSessionAccessManager gửi WebSocket message đến User có quyền Manager
8. SessionAccessService trả MessageResponse
9. session_access_routes.py trả về MessageResponse JSON
10. DocAgentService nhận response, đóng gói với ServiceResult và RequestAccessState set existingRequest = null
11. Component re-render, hiển thị nút "Yêu cầu truy cập" và Toast success notification hiển thị "Đã hủy yêu cầu"
12. User có quyền Manager (có WebSocket connection) nhận event REQUEST_DELETED, DocAgentPage xử lý event trong handler `onRequestDeleted`, increment requestTabReloadKey và SessionShareModal (nếu đang mở) tự động reload request list, xóa request khỏi danh sách

---

## Kịch bản 8: Bật/tắt liên kết chia sẻ công khai

1. User mở SessionShareModal
2. useEffect hook trigger khi modal open và Component gọi `DocAgentService.getSessionLink()`
3. DocAgentService gửi GET request tới `/session-access/link/by-session/{sessionId}`
4. Backend route nhận request, SessionAccessService được gọi `get_session_access_link_by_session()`, truy vấn SessionAccessLink (nếu không tồn tại, tạo link mới với is_active=false) và trả SessionAccessLink
5. session_access_routes.py trả về SessionAccessLinkResponse JSON
6. DocAgentService nhận response, đóng gói với ServiceResult và SessionShareModal set state generalAccess, hiển thị toggle switch và dropdown quyền
7. User toggle switch để bật/tắt link
8. SessionShareModal gọi `DocAgentService.updateSessionLink()`
9. DocAgentService gửi PUT request tới `/session-access/link/{linkId}`
10. Backend route nhận request, SessionAccessService được gọi `update_session_access_link()`, lấy link từ cơ sở dữ liệu, cập nhật is_active và commit transaction
11. SessionAccessService gọi WSSessionAccessManager để broadcast event LINK_UPDATED
12. WSSessionAccessManager gửi WebSocket message
13. SessionAccessService trả SessionAccessLink đã cập nhật
14. session_access_routes.py trả về SessionAccessLinkResponse JSON
15. DocAgentService nhận response, đóng gói với ServiceResult và SessionShareModal cập nhật state generalAccess
16. Component re-render và Toast success notification hiển thị "Đã cập nhật cấu hình liên kết"
23. session_access_routes.py trả về SessionAccessLinkResponse JSON
24. DocAgentService nhận response và đóng gói với ServiceResult
25. SessionShareModal cập nhật generalAccess state
26. Component re-render với toggle switch cập nhật
27. Toast success notification hiển thị "Link đã được bật" hoặc "Link đã được tắt"
28. Các User có WebSocket connection nhận event LINK_UPDATED
29. Component có thể reload link config nếu cần

---

## Kịch bản 9: Thay đổi quyền mặc định của liên kết chia sẻ

1. User mở SessionShareModal (đã load link config)
2. User click dropdown quyền của General Access
3. User chọn quyền mới (Reader/Contributor/Manager)
4. SessionShareModal gọi `DocAgentService.updateSessionLink()`
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
