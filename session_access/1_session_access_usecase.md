# Use Case Diagram - Quản lý Session Access

```mermaid
graph TB
    User["👤 User"]
    
    Login["Đăng nhập"]
    ListSessions["Xem danh sách Session<br/><b>extension points</b><br/>Xem chi tiết Session"]
    ViewSessionDetail["Xem chi tiết Session<br/><b>extension points</b><br/>Cập nhật quyền realtime<br/>Chia sẻ Session với User<br/>Cập nhật quyền truy cập<br/>ExtensionPoint<br/>ExtensionPoint<br/>Quản lý liên kết chia sẻ"]
    
    ShareSession["Chia sẻ Session với User"]
    UpdatePermission["Cập nhật quyền truy cập"]
    RevokePermission["Thu hồi quyền truy cập"]
    HandleRequest["Xử lý yêu cầu truy cập"]
    ManageLink["Quản lý liên kết chia sẻ"]
    ReceiveUpdate["Cập nhật quyền realtime"]
    
    RequestAccess["Yêu cầu truy cập Session"]
    CancelRequest["Hủy yêu cầu truy cập Session"]
    
    User -->|Truy cập| Login
    Login -.->|<<Include>>| ListSessions
    ListSessions -->|Thực hiện| User
    
    ListSessions -.->|<<Extend>>| ViewSessionDetail
    ViewSessionDetail -->|Thực hiện| User
    
    ViewSessionDetail -.->|<<Extend>>| ShareSession
    ShareSession -->|Thực hiện| User
    
    ViewSessionDetail -.->|<<Extend>>| UpdatePermission
    UpdatePermission -->|Thực hiện| User
    
    ViewSessionDetail -.->|<<Extend>>| RevokePermission
    RevokePermission -->|Thực hiện| User
    
    ViewSessionDetail -.->|<<Extend>>| HandleRequest
    HandleRequest -->|Thực hiện| User
    
    ViewSessionDetail -.->|<<Extend>>| ManageLink
    ManageLink -->|Thực hiện| User
    
    ViewSessionDetail -.->|<<Extend>>| ReceiveUpdate
    ReceiveUpdate -->|Nhận| User
    
    User -->|Thực hiện| RequestAccess
    User -->|Thực hiện| CancelRequest
    
    style User fill:#e1f5ff
    style Login fill:#fff3e0
    style ListSessions fill:#f3e5f5
    style ViewSessionDetail fill:#fce4ec
    style ShareSession fill:#e8f5e9
    style UpdatePermission fill:#e8f5e9
    style RevokePermission fill:#ffebee
    style HandleRequest fill:#e8f5e9
    style ManageLink fill:#e8f5e9
    style RequestAccess fill:#fce4ec
    style CancelRequest fill:#ffebee
    style ReceiveUpdate fill:#f3e5f5
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập hệ thống.
- Xem danh sách Session là bước cần thiết để có thể xem chi tiết Session.
- Xem chi tiết Session có các extension points cho các thao tác quản lý Session Access.
- Phân quyền cụ thể được xử lý bởi hệ thống quản lý quyền truy cập.
- User có thể yêu cầu và hủy yêu cầu truy cập khi chưa có quyền.
- User nhận thông báo realtime khi quyền thay đổi.
