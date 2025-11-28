# Sequence Diagram - Quản lý Session Access

## 1. Chia sẻ Session với User cụ thể

```mermaid
sequenceDiagram
    actor User
    participant Header as Header.tsx
    participant SessionShareDialog as SessionShareDialog.tsx
    participant SearchDialog as SearchDialog.tsx
    participant AuthApi as AuthApi
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes.py
    participant SessionAccessService as SessionAccessService
    participant SessionAccessLink as SessionAccessLink
    participant SessionAccessGrant as SessionAccessGrant

    User->>Header: Click "Chia sẻ"
    activate Header
    
    Header->>SessionShareDialog: gọi
    deactivate Header
    activate SessionShareDialog
    SessionShareDialog-->>User: Hiển thị
    deactivate SessionShareDialog
    
    User->>SessionShareDialog: Click "Chia sẻ" tab
    activate SessionShareDialog
    
    SessionShareDialog->>SessionShareDialog: loadShareData()
    
    alt Loop
        SessionShareDialog->>AuthApi: gọi
        activate AuthApi
        AuthApi->>AuthApi: getUserById()
        AuthApi-->>SessionShareDialog: trả về
        deactivate AuthApi
    end
    
    SessionShareDialog->>DocAgentService: gọi
    deactivate SessionShareDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: getSessionGrantsBySession()
    DocAgentService->>SessionAccessRoutes: GET "/session-access/grant/by-session/{session_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: get_session_access_grants_by_session()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessGrant: gọi
    activate SessionAccessGrant
    
    SessionAccessGrant->>SessionAccessGrant: SessionAccessGrant()
    SessionAccessGrant-->>SessionAccessService: trả về
    deactivate SessionAccessGrant
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService->>DocAgentService: getSessionLink()
    DocAgentService->>SessionAccessRoutes: GET "/session-access/link/by-session/{session_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: get_session_access_link_by_session()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessLink: gọi
    activate SessionAccessLink
    
    SessionAccessLink->>SessionAccessLink: SessionAccessLink()
    SessionAccessLink-->>SessionAccessService: trả về
    deactivate SessionAccessLink
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>User: Hiển thị
    deactivate DocAgentService
    
    User->>SessionShareDialog: Nhập keyword tìm kiếm User
    activate SessionShareDialog
    
    SessionShareDialog->>SearchDialog: gọi
    activate SearchDialog
    
    SearchDialog->>AuthApi: gọi
    activate AuthApi
    
    AuthApi->>AuthApi: searchUsersByName()
    AuthApi-->>SearchDialog: trả về
    deactivate AuthApi
    
    SearchDialog-->>User: Hiển thị
    deactivate SearchDialog
    
    User->>SessionShareDialog: Chọn User và click "Thêm"
    
    SessionShareDialog->>SessionShareDialog: validate()
    SessionShareDialog->>SessionShareDialog: handleAddUser()
    SessionShareDialog->>DocAgentService: gọi
    deactivate SessionShareDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: createInvite()
    DocAgentService->>SessionAccessRoutes: POST "/session-access/invite"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: create_session_access_invite()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessGrant: gọi
    activate SessionAccessGrant
    
    SessionAccessGrant->>SessionAccessGrant: SessionAccessGrant()
    SessionAccessGrant-->>SessionAccessService: trả về
    deactivate SessionAccessGrant
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>SessionShareDialog: trả về
    deactivate DocAgentService
    
    SessionShareDialog-->>User: Hiển thị
```

---

## 2. Cập nhật quyền truy cập

```mermaid
sequenceDiagram
    actor User
    participant SessionShareDialog as SessionShareDialog.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes.py
    participant SessionAccessService as SessionAccessService
    participant SessionAccessGrant as SessionAccessGrant

    User->>SessionShareDialog: Chọn quyền từ dropdown của User
    activate SessionShareDialog
    
    SessionShareDialog->>SessionShareDialog: handleUpdateSessionGrant()
    SessionShareDialog->>DocAgentService: gọi
    deactivate SessionShareDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: updateSessionGrant()
    DocAgentService->>SessionAccessRoutes: PUT "/session-access/grant/{grant_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: update_session_access_grant()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessGrant: gọi
    activate SessionAccessGrant
    
    SessionAccessGrant->>SessionAccessGrant: SessionAccessGrant()
    SessionAccessGrant-->>SessionAccessService: trả về
    deactivate SessionAccessGrant
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>SessionShareDialog: trả về
    deactivate DocAgentService
    
    SessionShareDialog-->>User: Hiển thị
```

---

## 3. Thu hồi quyền truy cập

```mermaid
sequenceDiagram
    actor User
    participant SessionShareDialog as SessionShareDialog.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes.py
    participant SessionAccessService as SessionAccessService
    participant MessageResponse as MessageResponse

    User->>SessionShareDialog: Click "Xóa" từ dropdown của User
    activate SessionShareDialog
    
    SessionShareDialog->>SessionShareDialog: handleRemoveUser()
    SessionShareDialog->>DocAgentService: gọi
    deactivate SessionShareDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: revokeSessionGrant()
    DocAgentService->>SessionAccessRoutes: DELETE "/session-access/grant/{grant_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: revoke_session_access_grant()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>MessageResponse: gọi
    activate MessageResponse
    
    MessageResponse->>MessageResponse: MessageResponse()
    MessageResponse-->>SessionAccessService: trả về
    deactivate MessageResponse
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>SessionShareDialog: trả về
    deactivate DocAgentService
    
    SessionShareDialog-->>User: Hiển thị
```

---

## 4. Quản lý liên kết chia sẻ

```mermaid
sequenceDiagram
    actor User
    participant SessionShareDialog as SessionShareDialog.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes.py
    participant SessionAccessService as SessionAccessService
    participant SessionAccessLink as SessionAccessLink

    User->>SessionShareDialog: Chọn "Bật kỳ ai có liên kết" trong dropdown
    activate SessionShareDialog
    
    SessionShareDialog->>SessionShareDialog: handleLinkUpdate()
    SessionShareDialog->>DocAgentService: gọi
    deactivate SessionShareDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: updateSessionLink()
    DocAgentService->>SessionAccessRoutes: PUT "/session-access/link/{link_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: update_session_access_link()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessLink: gọi
    activate SessionAccessLink
    
    SessionAccessLink->>SessionAccessLink: SessionAccessLink()
    SessionAccessLink-->>SessionAccessService: trả về
    deactivate SessionAccessLink
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>User: Hiển thị
    deactivate DocAgentService
    
    User->>SessionShareDialog: Chọn quyền liên kết dropdown
    activate SessionShareDialog
    
    SessionShareDialog->>SessionShareDialog: handleLinkUpdate()
    SessionShareDialog->>DocAgentService: gọi
    deactivate SessionShareDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: updateSessionLink()
    DocAgentService->>SessionAccessRoutes: PUT "/session-access/link/{link_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: update_session_access_link()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessLink: gọi
    activate SessionAccessLink
    
    SessionAccessLink->>SessionAccessLink: SessionAccessLink()
    SessionAccessLink-->>SessionAccessService: trả về
    deactivate SessionAccessLink
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>SessionShareDialog: trả về
    deactivate DocAgentService
    
    SessionShareDialog-->>User: Hiển thị
```

---

## 5. Chấp nhận yêu cầu truy cập

```mermaid
sequenceDiagram
    actor User
    participant SessionShareDialog as SessionShareDialog.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes.py
    participant SessionAccessService as SessionAccessService
    participant SessionAccessRequest as SessionAccessRequest

    User->>SessionShareDialog: Chọn quyền từ dropdown và click "Cấp quyền"
    activate SessionShareDialog
    
    SessionShareDialog->>SessionShareDialog: handleAcceptRequest()
    SessionShareDialog->>DocAgentService: gọi
    deactivate SessionShareDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: acceptSessionRequest()
    DocAgentService->>SessionAccessRoutes: POST "/session-access/request/accept/{request_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: accept_session_access_request()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessRequest: gọi
    activate SessionAccessRequest
    
    SessionAccessRequest->>SessionAccessRequest: SessionAccessRequest()
    SessionAccessRequest-->>SessionAccessService: trả về
    deactivate SessionAccessRequest
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>SessionShareDialog: trả về
    deactivate DocAgentService
    
    SessionShareDialog-->>User: Hiển thị
```

---

## 6. Từ chối yêu cầu truy cập

```mermaid
sequenceDiagram
    actor User
    participant SessionShareDialog as SessionShareDialog.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes.py
    participant SessionAccessService as SessionAccessService
    participant SessionAccessRequest as SessionAccessRequest

    User->>SessionShareDialog: Click "Từ chối" request to User
    activate SessionShareDialog
    
    SessionShareDialog->>SessionShareDialog: handleRejectRequest()
    SessionShareDialog->>DocAgentService: gọi
    deactivate SessionShareDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: rejectSessionRequest()
    DocAgentService->>SessionAccessRoutes: DELETE "/session-access/request/reject/{request_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: reject_session_access_request()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessRequest: gọi
    activate SessionAccessRequest
    
    SessionAccessRequest->>SessionAccessRequest: SessionAccessRequest()
    SessionAccessRequest-->>SessionAccessService: trả về
    deactivate SessionAccessRequest
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>SessionShareDialog: trả về
    deactivate DocAgentService
    
    SessionShareDialog-->>User: Hiển thị
```

---

## 7. Yêu cầu truy cập Session

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.tsx
    participant RequestAccessState as RequestAccessState.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes.py
    participant SessionAccessService as SessionAccessService
    participant SessionAccessRequest as SessionAccessRequest

    User->>DocAgentPage: Xem chi tiết Session
    activate DocAgentPage
    
    DocAgentPage->>RequestAccessState: gọi
    activate RequestAccessState
    
    RequestAccessState->>RequestAccessState: loadUserSessionRequest()
    RequestAccessState->>DocAgentService: gọi
    deactivate RequestAccessState
    activate DocAgentService
    
    DocAgentService->>DocAgentService: getUserSessionRequest()
    DocAgentService->>SessionAccessRoutes: GET "/session-access/request/by-user-session/{user_id}/{session_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: get_user_session_access_request()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessRequest: gọi
    activate SessionAccessRequest
    
    SessionAccessRequest->>SessionAccessRequest: SessionAccessRequest()
    SessionAccessRequest-->>SessionAccessService: trả về
    deactivate SessionAccessRequest
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>User: Hiển thị
    deactivate DocAgentService
    
    alt Nếu danh sách session access != null
        User->>RequestAccessState: gọi
        activate RequestAccessState
        RequestAccessState-->>User: trả về
        deactivate RequestAccessState
        
        User->>RequestAccessState: Click "Tạo" hoặc "Yêu cầu"
        activate RequestAccessState
        
        RequestAccessState->>RequestAccessState: handleCreateRequest()
        RequestAccessState->>DocAgentService: gọi
        deactivate RequestAccessState
        activate DocAgentService
        
        DocAgentService->>DocAgentService: createSessionRequest()
        DocAgentService->>SessionAccessRoutes: POST "/session-access/request"
        activate SessionAccessRoutes
        
        SessionAccessRoutes->>SessionAccessService: create_session_access_request()
        activate SessionAccessService
        
        SessionAccessService->>SessionAccessService: gọi
        SessionAccessService->>SessionAccessRequest: gọi
        activate SessionAccessRequest
        
        SessionAccessRequest->>SessionAccessRequest: SessionAccessRequest()
        SessionAccessRequest-->>SessionAccessService: trả về
        deactivate SessionAccessRequest
        
        SessionAccessService-->>SessionAccessRoutes: trả về
        deactivate SessionAccessService
        
        SessionAccessRoutes-->>DocAgentService: trả về
        deactivate SessionAccessRoutes
        
        DocAgentService-->>DocAgentPage: trả về
        deactivate DocAgentService
        
        DocAgentPage-->>User: Hiển thị
    end
    
    deactivate DocAgentPage
```

---

## 8. Hủy yêu cầu truy cập

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.tsx
    participant RequestAccessState as RequestAccessState.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes.py
    participant SessionAccessService as SessionAccessService
    participant SessionAccessRequest as SessionAccessRequest

    User->>DocAgentPage: Xem chi tiết Session
    activate DocAgentPage
    
    DocAgentPage->>RequestAccessState: gọi
    activate RequestAccessState
    
    RequestAccessState->>RequestAccessState: loadUserSessionRequest()
    RequestAccessState->>DocAgentService: gọi
    deactivate RequestAccessState
    activate DocAgentService
    
    DocAgentService->>DocAgentService: getUserSessionRequest()
    DocAgentService->>SessionAccessRoutes: GET "/session-access/request/by-user-session/{user_id}/{session_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: get_user_session_access_request()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessRequest: gọi
    activate SessionAccessRequest
    
    SessionAccessRequest->>SessionAccessRequest: SessionAccessRequest()
    SessionAccessRequest-->>SessionAccessService: trả về
    deactivate SessionAccessRequest
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    DocAgentService-->>User: Hiển thị
    deactivate DocAgentService
    
    alt Nếu danh sách session access != null
        User->>RequestAccessState: gọi
        activate RequestAccessState
        RequestAccessState-->>User: trả về
        deactivate RequestAccessState
        
        User->>RequestAccessState: Click "Từ chối"
        activate RequestAccessState
        
        RequestAccessState->>RequestAccessState: handleDeleteRequest()
        RequestAccessState->>DocAgentService: gọi
        deactivate RequestAccessState
        activate DocAgentService
        
        DocAgentService->>DocAgentService: deleteRequest()
        DocAgentService->>SessionAccessRoutes: DELETE "/session-access/request/{request_id}"
        activate SessionAccessRoutes
        
        SessionAccessRoutes->>SessionAccessService: delete_session_access_request()
        activate SessionAccessService
        
        SessionAccessService->>SessionAccessService: gọi
        SessionAccessService->>SessionAccessRequest: gọi
        activate SessionAccessRequest
        
        SessionAccessRequest->>SessionAccessRequest: SessionAccessRequest()
        SessionAccessRequest-->>SessionAccessService: trả về
        deactivate SessionAccessRequest
        
        SessionAccessService-->>SessionAccessRoutes: trả về
        deactivate SessionAccessService
        
        SessionAccessRoutes-->>DocAgentService: trả về
        deactivate SessionAccessRoutes
        
        DocAgentService-->>DocAgentPage: trả về
        deactivate DocAgentService
        
        DocAgentPage-->>User: Hiển thị
    end
    
    deactivate DocAgentPage
```

---

## 9. Xem danh sách yêu cầu truy cập

```mermaid
sequenceDiagram
    actor User
    participant Header as Header.tsx
    participant SessionShareDialog as SessionShareDialog.tsx
    participant AuthApi as AuthApi
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes.py
    participant SessionAccessService as SessionAccessService
    participant SessionAccessRequest as SessionAccessRequest

    User->>Header: Click "Chia sẻ"
    activate Header
    
    Header->>SessionShareDialog: gọi
    deactivate Header
    activate SessionShareDialog
    SessionShareDialog-->>User: Hiển thị
    deactivate SessionShareDialog
    
    User->>SessionShareDialog: Click "Yêu cầu" tab
    activate SessionShareDialog
    
    SessionShareDialog->>SessionShareDialog: loadSessionRequests()
    SessionShareDialog->>DocAgentService: gọi
    deactivate SessionShareDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: getSessionRequestsBySession()
    DocAgentService->>SessionAccessRoutes: GET "/session-access/grant/by-session/{session_id}"
    activate SessionAccessRoutes
    
    SessionAccessRoutes->>SessionAccessService: get_session_access_requests_by_session()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: gọi
    SessionAccessService->>SessionAccessRequest: gọi
    activate SessionAccessRequest
    
    SessionAccessRequest->>SessionAccessRequest: SessionAccessRequest()
    SessionAccessRequest-->>SessionAccessService: trả về
    deactivate SessionAccessRequest
    
    SessionAccessService-->>SessionAccessRoutes: trả về
    deactivate SessionAccessService
    
    SessionAccessRoutes-->>DocAgentService: trả về
    deactivate SessionAccessRoutes
    
    alt Loop
        DocAgentService->>AuthApi: gọi
        activate AuthApi
        AuthApi->>AuthApi: getUserById()
        AuthApi-->>DocAgentService: trả về
        deactivate AuthApi
    end
    
    DocAgentService-->>User: Hiển thị
    deactivate DocAgentService
```
