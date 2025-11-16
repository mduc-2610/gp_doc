# Sequence Diagram - Quản lý Session Access

## 1. Chia sẻ Session với User cụ thể

```mermaid
sequenceDiagram
    actor Manager
    participant Header as Header.tsx
    participant ShareModal as SessionShareModal.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes
    participant SessionAccessService as SessionAccessService
    participant WSManager as WSSessionAccessManager
    participant WebSocket as WebSocket
    actor User

    Manager->>Header: Click nút "Share"
    activate Header
    Header->>ShareModal: Open modal (open=true)
    activate ShareModal
    
    ShareModal->>DocAgentService: getSessionGrants(sessionId)
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: GET /session-access/grant/by-session/{sessionId}
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: get_session_access_grants_by_session()
    activate SessionAccessService
    SessionAccessService-->>SessionAccessRoutes: [SessionAccessGrant]
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: [SessionAccessGrantResponse]
    deactivate SessionAccessRoutes
    DocAgentService-->>ShareModal: ServiceResult~SessionAccessGrant[]~
    deactivate DocAgentService
    
    ShareModal-->>Manager: Hiển thị danh sách User đã được chia sẻ
    
    Manager->>ShareModal: Click "Add people"
    ShareModal->>ShareModal: Open UserSearchDialog
    ShareModal-->>Manager: Hiển thị dialog tìm kiếm User
    
    Manager->>ShareModal: Chọn User và access level
    Manager->>ShareModal: Click "Add"
    
    ShareModal->>DocAgentService: createInvite({invited_to, invited_level})
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: POST /session-access/invite
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: create_session_access_invite(auto_grant=true)
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: Check existing grant/invite
    SessionAccessService->>SessionAccessService: Create SessionAccessInvite
    SessionAccessService->>SessionAccessService: Create SessionAccessGrant (auto_grant)
    SessionAccessService->>SessionAccessService: Update invite.granted_access_id
    SessionAccessService->>SessionAccessService: db.commit()
    
    SessionAccessService->>WSManager: broadcast_permission_granted(payload)
    activate WSManager
    WSManager->>WebSocket: Send PERMISSION_GRANTED event
    deactivate WSManager
    
    SessionAccessService-->>SessionAccessRoutes: SessionAccessInvite
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: SessionAccessInviteResponse
    deactivate SessionAccessRoutes
    DocAgentService-->>ShareModal: ServiceResult~SessionAccessInvite~
    deactivate DocAgentService
    
    ShareModal->>ShareModal: Reload grant list
    ShareModal-->>Manager: Hiển thị danh sách cập nhật + Toast "Chia sẻ thành công"
    deactivate ShareModal
    deactivate Header
    
    WebSocket-->>User: Receive PERMISSION_GRANTED event
    activate User
    User->>User: Toast "Bạn đã được cấp quyền"
    User->>User: Reload permission & Session data
    deactivate User
```

---

## 2. Cập nhật quyền truy cập

```mermaid
sequenceDiagram
    actor Manager
    participant ShareModal as SessionShareModal.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes
    participant SessionAccessService as SessionAccessService
    participant WSManager as WSSessionAccessManager
    participant WebSocket as WebSocket
    actor User

    Manager->>ShareModal: Trong modal, chọn quyền mới từ dropdown
    activate ShareModal
    
    ShareModal->>DocAgentService: updateGrant(grantId, {access_level})
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: PUT /session-access/grant/{grantId}
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: update_session_access_grant()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: Get grant from DB
    SessionAccessService->>SessionAccessService: Update access_level
    SessionAccessService->>SessionAccessService: db.commit()
    
    SessionAccessService->>WSManager: broadcast_permission_changed(payload)
    activate WSManager
    WSManager->>WebSocket: Send PERMISSION_CHANGED event
    deactivate WSManager
    
    SessionAccessService-->>SessionAccessRoutes: SessionAccessGrant
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: SessionAccessGrantResponse
    deactivate SessionAccessRoutes
    DocAgentService-->>ShareModal: ServiceResult~SessionAccessGrant~
    deactivate DocAgentService
    
    ShareModal->>ShareModal: Update grant in list
    ShareModal-->>Manager: Toast "Cập nhật quyền thành công"
    deactivate ShareModal
    
    WebSocket-->>User: Receive PERMISSION_CHANGED event
    activate User
    User->>User: Toast "Quyền của bạn đã được cập nhật"
    User->>User: Reload permission
    deactivate User
```

---

## 3. Thu hồi quyền truy cập

```mermaid
sequenceDiagram
    actor Manager
    participant ShareModal as SessionShareModal.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes
    participant SessionAccessService as SessionAccessService
    participant WSManager as WSSessionAccessManager
    participant WebSocket as WebSocket
    actor User
    participant RequestAccessState as RequestAccessState.tsx

    Manager->>ShareModal: Click icon "X" để xóa User
    activate ShareModal
    
    ShareModal->>DocAgentService: revokeGrant(grantId)
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: DELETE /session-access/grant/revoke/{grantId}
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: revoke_session_access_grant()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: Get grant from DB
    SessionAccessService->>SessionAccessService: Set is_active = false
    SessionAccessService->>SessionAccessService: db.commit()
    
    SessionAccessService->>WSManager: broadcast_permission_revoked(payload)
    activate WSManager
    WSManager->>WebSocket: Send PERMISSION_REVOKED event
    deactivate WSManager
    
    SessionAccessService-->>SessionAccessRoutes: SessionAccessGrant
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: MessageResponse
    deactivate SessionAccessRoutes
    DocAgentService-->>ShareModal: ServiceResult~MessageResponse~
    deactivate DocAgentService
    
    ShareModal->>ShareModal: Remove grant from list
    ShareModal-->>Manager: Toast "Thu hồi quyền thành công"
    deactivate ShareModal
    
    WebSocket-->>User: Receive PERMISSION_REVOKED event
    activate User
    User->>User: Toast "Quyền truy cập của bạn đã bị thu hồi"
    User->>User: Reload permission
    User->>RequestAccessState: Chuyển sang RequestAccessState
    activate RequestAccessState
    RequestAccessState-->>User: Hiển thị UI yêu cầu truy cập
    deactivate RequestAccessState
    deactivate User
```

---

## 4. Yêu cầu truy cập Session

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.tsx
    participant RequestAccessState as RequestAccessState.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes
    participant SessionAccessService as SessionAccessService
    participant WSManager as WSSessionAccessManager
    participant WebSocket as WebSocket
    actor Manager
    participant ShareModal as SessionShareModal.tsx

    User->>DocAgentPage: Truy cập Session URL
    activate DocAgentPage
    DocAgentPage->>DocAgentPage: Check permission
    DocAgentPage->>RequestAccessState: Hiển thị RequestAccessState (no access)
    activate RequestAccessState
    
    RequestAccessState->>DocAgentService: getUserSessionRequest(sessionId, userId)
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: GET /session-access/request/by-user-session/{sessionId}/{userId}
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: get_session_access_request_by_user_session()
    activate SessionAccessService
    SessionAccessService-->>SessionAccessRoutes: SessionAccessRequest | null
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: SessionAccessRequestResponse | null
    deactivate SessionAccessRoutes
    DocAgentService-->>RequestAccessState: ServiceResult~SessionAccessRequest | null~
    deactivate DocAgentService
    
    RequestAccessState-->>User: Hiển thị nút "Request Access"
    
    User->>RequestAccessState: Click "Request Access"
    RequestAccessState->>DocAgentService: createRequest({session_id, requested_by})
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: POST /session-access/request
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: create_session_access_request()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: Check existing request/grant
    SessionAccessService->>SessionAccessService: Create SessionAccessRequest (status=PENDING)
    SessionAccessService->>SessionAccessService: db.commit()
    
    SessionAccessService->>WSManager: broadcast_request_created(payload)
    activate WSManager
    WSManager->>WebSocket: Send REQUEST_CREATED event
    deactivate WSManager
    
    SessionAccessService-->>SessionAccessRoutes: SessionAccessRequest
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: SessionAccessRequestResponse
    deactivate SessionAccessRoutes
    DocAgentService-->>RequestAccessState: ServiceResult~SessionAccessRequest~
    deactivate DocAgentService
    
    RequestAccessState->>RequestAccessState: Update existingRequest
    RequestAccessState-->>User: Hiển thị trạng thái "Pending" + Toast "Yêu cầu đã được gửi"
    deactivate RequestAccessState
    deactivate DocAgentPage
    
    WebSocket-->>Manager: Receive REQUEST_CREATED event
    activate Manager
    Manager->>Manager: Toast "Có yêu cầu truy cập mới"
    Manager->>ShareModal: Increment requestTabReloadKey
    activate ShareModal
    ShareModal->>ShareModal: Reload request list
    deactivate ShareModal
    deactivate Manager
```

---

## 5. Chấp nhận yêu cầu truy cập

```mermaid
sequenceDiagram
    actor Manager
    participant ShareModal as SessionShareModal.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes
    participant SessionAccessService as SessionAccessService
    participant WSManager as WSSessionAccessManager
    participant WebSocket as WebSocket
    actor User
    participant DocAgentPage as DocAgentPage.tsx

    Manager->>ShareModal: Open modal và click tab "Request"
    activate ShareModal
    
    ShareModal->>DocAgentService: getSessionRequests(sessionId)
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: GET /session-access/request/by-session/{sessionId}
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: get_session_access_requests_by_session()
    activate SessionAccessService
    SessionAccessService-->>SessionAccessRoutes: [SessionAccessRequest]
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: [SessionAccessRequestResponse]
    deactivate SessionAccessRoutes
    DocAgentService-->>ShareModal: ServiceResult~SessionAccessRequest[]~
    deactivate DocAgentService
    
    ShareModal-->>Manager: Hiển thị danh sách yêu cầu
    
    Manager->>ShareModal: Chọn access level và click "Approve"
    
    ShareModal->>DocAgentService: acceptRequest(requestId, {requested_level})
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: POST /session-access/request/accept/{requestId}
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: accept_session_access_request()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: Get request from DB
    SessionAccessService->>SessionAccessService: Update status = APPROVED
    SessionAccessService->>SessionAccessService: Create SessionAccessGrant
    SessionAccessService->>SessionAccessService: Update request.granted_access_id
    SessionAccessService->>SessionAccessService: db.commit()
    
    SessionAccessService->>WSManager: broadcast_permission_granted(payload)
    activate WSManager
    WSManager->>WebSocket: Send PERMISSION_GRANTED event
    deactivate WSManager
    
    SessionAccessService-->>SessionAccessRoutes: SessionAccessRequest
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: SessionAccessRequestResponse
    deactivate SessionAccessRoutes
    DocAgentService-->>ShareModal: ServiceResult~SessionAccessRequest~
    deactivate DocAgentService
    
    ShareModal->>ShareModal: Remove request from list
    ShareModal-->>Manager: Toast "Đã chấp nhận yêu cầu"
    deactivate ShareModal
    
    WebSocket-->>User: Receive PERMISSION_GRANTED event
    activate User
    User->>DocAgentPage: Handle onPermissionGranted
    activate DocAgentPage
    DocAgentPage->>DocAgentPage: Toast "Bạn đã được cấp quyền truy cập"
    DocAgentPage->>DocAgentPage: setPermissionLoading(true)
    DocAgentPage->>DocAgentPage: Reload permission & Session data
    DocAgentPage-->>User: Hiển thị Session với quyền mới
    deactivate DocAgentPage
    deactivate User
```

---

## 6. Từ chối yêu cầu truy cập

```mermaid
sequenceDiagram
    actor Manager
    participant ShareModal as SessionShareModal.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes
    participant SessionAccessService as SessionAccessService
    participant WSManager as WSSessionAccessManager
    participant WebSocket as WebSocket
    actor User
    participant RequestAccessState as RequestAccessState.tsx

    Manager->>ShareModal: Trong tab "Request", click "Deny"
    activate ShareModal
    
    ShareModal->>DocAgentService: rejectRequest(requestId)
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: POST /session-access/request/reject/{requestId}
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: reject_session_access_request()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: Get request from DB
    SessionAccessService->>SessionAccessService: Update status = REJECTED
    SessionAccessService->>SessionAccessService: db.commit()
    
    SessionAccessService->>WSManager: broadcast_request_deleted(payload)
    activate WSManager
    WSManager->>WebSocket: Send REQUEST_DELETED event
    deactivate WSManager
    
    SessionAccessService-->>SessionAccessRoutes: MessageResponse
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: MessageResponse
    deactivate SessionAccessRoutes
    DocAgentService-->>ShareModal: ServiceResult~MessageResponse~
    deactivate DocAgentService
    
    ShareModal->>ShareModal: Remove request from list
    ShareModal-->>Manager: Toast "Đã từ chối yêu cầu"
    deactivate ShareModal
    
    WebSocket-->>User: Receive REQUEST_DELETED event
    activate User
    User->>RequestAccessState: Handle event
    activate RequestAccessState
    RequestAccessState->>RequestAccessState: Clear existingRequest
    RequestAccessState-->>User: Hiển thị lại nút "Request Access"
    deactivate RequestAccessState
    deactivate User
```

---

## 7. Hủy yêu cầu truy cập

```mermaid
sequenceDiagram
    actor User
    participant RequestAccessState as RequestAccessState.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes
    participant SessionAccessService as SessionAccessService
    participant WSManager as WSSessionAccessManager
    participant WebSocket as WebSocket
    actor Manager
    participant ShareModal as SessionShareModal.tsx

    User->>RequestAccessState: Trong trạng thái "Pending", click "Cancel Request"
    activate RequestAccessState
    
    RequestAccessState->>DocAgentService: deleteRequest(requestId, sessionId)
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: DELETE /session-access/request/{requestId}
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: delete_session_access_request()
    activate SessionAccessService
    
    SessionAccessService->>SessionAccessService: Get request from DB
    SessionAccessService->>SessionAccessService: Delete request
    SessionAccessService->>SessionAccessService: db.commit()
    
    SessionAccessService->>WSManager: broadcast_request_deleted(payload)
    activate WSManager
    WSManager->>WebSocket: Send REQUEST_DELETED event
    deactivate WSManager
    
    SessionAccessService-->>SessionAccessRoutes: MessageResponse
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: MessageResponse
    deactivate SessionAccessRoutes
    DocAgentService-->>RequestAccessState: ServiceResult~MessageResponse~
    deactivate DocAgentService
    
    RequestAccessState->>RequestAccessState: Clear existingRequest
    RequestAccessState-->>User: Hiển thị nút "Request Access" + Toast "Đã hủy yêu cầu"
    deactivate RequestAccessState
    
    WebSocket-->>Manager: Receive REQUEST_DELETED event
    activate Manager
    Manager->>ShareModal: Increment requestTabReloadKey
    activate ShareModal
    ShareModal->>ShareModal: Reload request list
    ShareModal-->>Manager: Cập nhật danh sách yêu cầu
    deactivate ShareModal
    deactivate Manager
```

---

## 8. Quản lý liên kết chia sẻ công khai

```mermaid
sequenceDiagram
    actor Manager
    participant ShareModal as SessionShareModal.tsx
    participant DocAgentService as DocAgentService
    participant SessionAccessRoutes as session_access_routes
    participant SessionAccessService as SessionAccessService
    participant WSManager as WSSessionAccessManager
    participant WebSocket as WebSocket

    Manager->>ShareModal: Open modal
    activate ShareModal
    
    ShareModal->>DocAgentService: getSessionLink(sessionId)
    activate DocAgentService
    DocAgentService->>SessionAccessRoutes: GET /session-access/link/by-session/{sessionId}
    activate SessionAccessRoutes
    SessionAccessRoutes->>SessionAccessService: get_session_access_link_by_session()
    activate SessionAccessService
    SessionAccessService-->>SessionAccessRoutes: SessionAccessLink
    deactivate SessionAccessService
    SessionAccessRoutes-->>DocAgentService: SessionAccessLinkResponse
    deactivate SessionAccessRoutes
    DocAgentService-->>ShareModal: ServiceResult~SessionAccessLink~
    deactivate DocAgentService
    
    ShareModal-->>Manager: Hiển thị toggle và dropdown quyền
    
    alt Bật/tắt link
        Manager->>ShareModal: Toggle switch on/off
        ShareModal->>DocAgentService: updateSessionLink(linkId, {is_active})
        activate DocAgentService
        DocAgentService->>SessionAccessRoutes: PUT /session-access/link/{linkId}
        activate SessionAccessRoutes
        SessionAccessRoutes->>SessionAccessService: update_session_access_link()
        activate SessionAccessService
        
        SessionAccessService->>SessionAccessService: Update is_active
        SessionAccessService->>SessionAccessService: db.commit()
        
        SessionAccessService->>WSManager: broadcast_link_updated(payload)
        activate WSManager
        WSManager->>WebSocket: Send LINK_UPDATED event
        deactivate WSManager
        
        SessionAccessService-->>SessionAccessRoutes: SessionAccessLink
        deactivate SessionAccessService
        SessionAccessRoutes-->>DocAgentService: SessionAccessLinkResponse
        deactivate SessionAccessRoutes
        DocAgentService-->>ShareModal: ServiceResult~SessionAccessLink~
        deactivate DocAgentService
        
        ShareModal-->>Manager: Toast "Link đã được bật/tắt"
    else Thay đổi quyền mặc định
        Manager->>ShareModal: Chọn quyền mới từ dropdown
        ShareModal->>DocAgentService: updateSessionLink(linkId, {access_level})
        activate DocAgentService
        DocAgentService->>SessionAccessRoutes: PUT /session-access/link/{linkId}
        activate SessionAccessRoutes
        SessionAccessRoutes->>SessionAccessService: update_session_access_link()
        activate SessionAccessService
        
        SessionAccessService->>SessionAccessService: Update access_level
        SessionAccessService->>SessionAccessService: db.commit()
        
        SessionAccessService->>WSManager: broadcast_link_updated(payload)
        activate WSManager
        WSManager->>WebSocket: Send LINK_UPDATED event
        deactivate WSManager
        
        SessionAccessService-->>SessionAccessRoutes: SessionAccessLink
        deactivate SessionAccessService
        SessionAccessRoutes-->>DocAgentService: SessionAccessLinkResponse
        deactivate SessionAccessRoutes
        DocAgentService-->>ShareModal: ServiceResult~SessionAccessLink~
        deactivate DocAgentService
        
        ShareModal-->>Manager: Toast "Quyền mặc định đã được cập nhật"
    end
    
    Manager->>ShareModal: Click "Copy link"
    ShareModal->>ShareModal: Copy Session URL to clipboard
    ShareModal-->>Manager: Toast "Đã copy link"
    deactivate ShareModal
```

---

## 9. Nhận cập nhật quyền realtime qua WebSocket

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.tsx
    participant DocSocketService as DocSocketService
    participant WebSocket as WebSocket Server
    participant WSManager as WSSessionAccessManager
    participant SessionAccessService as SessionAccessService

    User->>DocAgentPage: Truy cập Session
    activate DocAgentPage
    
    DocAgentPage->>DocSocketService: connectSessionSocket(sessionId, userId)
    activate DocSocketService
    DocSocketService->>WebSocket: Connect to ws://...session/ws/{sessionId}?user_id={userId}
    activate WebSocket
    WebSocket-->>DocSocketService: Connection established
    
    DocSocketService->>WSManager: connect(session_id, websocket, user_id)
    activate WSManager
    WSManager->>WSManager: Store connection in active_connections
    WSManager-->>DocSocketService: Connected
    deactivate WSManager
    
    DocSocketService->>WebSocket: Send {"action": "subscribe"}
    WebSocket-->>DocSocketService: WebSocket instance
    deactivate WebSocket
    DocSocketService-->>DocAgentPage: WebSocket instance
    deactivate DocSocketService
    
    DocAgentPage->>DocAgentPage: Start keep-alive ping (every 30s)
    DocAgentPage-->>User: Session loaded
    
    loop While User is on Session
        Note over DocAgentPage,WebSocket: User đang xem Session
        
        alt Permission Event từ Manager
            SessionAccessService->>WSManager: broadcast_permission_granted/changed/revoked()
            activate WSManager
            WSManager->>WebSocket: Send WsSessionAccessEnvelope
            deactivate WSManager
            activate WebSocket
            WebSocket-->>DocAgentPage: Receive event
            deactivate WebSocket
            
            alt Event = PERMISSION_GRANTED
                DocAgentPage->>DocAgentPage: Toast "Bạn đã được cấp quyền truy cập"
                DocAgentPage->>DocAgentPage: setPermissionLoading(true)
                DocAgentPage->>DocAgentPage: Reload permission & Session
            else Event = PERMISSION_CHANGED
                DocAgentPage->>DocAgentPage: Toast "Quyền của bạn đã được cập nhật"
                DocAgentPage->>DocAgentPage: setPermissionLoading(true)
            else Event = PERMISSION_REVOKED
                DocAgentPage->>DocAgentPage: Toast "Quyền truy cập của bạn đã bị thu hồi"
                DocAgentPage->>DocAgentPage: setPermissionLoading(true)
                DocAgentPage->>DocAgentPage: Show RequestAccessState
            end
            
        else Request Event từ User khác
            SessionAccessService->>WSManager: broadcast_request_created/deleted()
            activate WSManager
            WSManager->>WebSocket: Send WsSessionAccessEnvelope
            deactivate WSManager
            activate WebSocket
            WebSocket-->>DocAgentPage: Receive event (only if Manager)
            deactivate WebSocket
            
            alt Event = REQUEST_CREATED
                DocAgentPage->>DocAgentPage: Toast "Có yêu cầu truy cập mới"
                DocAgentPage->>DocAgentPage: Increment requestTabReloadKey
            else Event = REQUEST_DELETED
                DocAgentPage->>DocAgentPage: Increment requestTabReloadKey
            end
            
        else Link Update Event
            SessionAccessService->>WSManager: broadcast_link_updated()
            activate WSManager
            WSManager->>WebSocket: Send WsSessionAccessEnvelope
            deactivate WSManager
            activate WebSocket
            WebSocket-->>DocAgentPage: Receive event
            deactivate WebSocket
            
            DocAgentPage->>DocAgentPage: Reload link config (if modal open)
        end
    end
    
    User->>DocAgentPage: Rời khỏi Session (unmount)
    DocAgentPage->>DocSocketService: closeSessionSocket(ws)
    activate DocSocketService
    DocSocketService->>WebSocket: Close connection
    activate WebSocket
    WebSocket->>WSManager: disconnect(websocket)
    activate WSManager
    WSManager->>WSManager: Remove from active_connections
    deactivate WSManager
    WebSocket-->>DocSocketService: Closed
    deactivate WebSocket
    DocSocketService-->>DocAgentPage: Socket closed
    deactivate DocSocketService
    deactivate DocAgentPage
```

