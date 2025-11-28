# Use Case Diagram - Quản lý Session Access

```mermaid
graph TB
    User["👤 User"]
    
    Login["Đăng nhập"]
    ListSessions["Xem danh sách Session<br/><b>extension points</b><br/>Xem chi tiết Session"]
    ViewSessionDetail["Xem chi tiết Session<br/><b>extension points</b><br/>Xem danh sách yêu cầu truy cập<br/>Xem các thông tin về quyền truy cập"]
    ViewRequestList["Xem danh sách yêu cầu truy cập<br/><b>extension points</b><br/>Chấp nhận yêu cầu truy cập<br/>Từ chối yêu cầu truy cập"]
    ViewPermissionInfo["Xem các thông tin về quyền truy cập<br/><b>extension points</b><br/>Chia sẻ Session với User<br/>Cập nhật quyền truy cập<br/>Thu hồi quyền truy cập<br/>Quản lý liên kết chia sẻ"]
    
    AcceptRequest["Chấp nhận yêu cầu truy cập"]
    RejectRequest["Từ chối yêu cầu truy cập"]
    
    ShareSession["Chia sẻ Session với User"]
    UpdatePermission["Cập nhật quyền truy cập"]
    RevokePermission["Thu hồi quyền truy cập"]
    ManageLink["Quản lý liên kết chia sẻ"]
    
    RequestAccess["Yêu cầu truy cập Session"]
    CancelRequest["Hủy yêu cầu truy cập Session"]
    
    User -->|Truy cập| Login
    Login -.->|<<Include>>| ListSessions
    ListSessions -->|Thực hiện| User
    
    ListSessions -.->|<<Extend>>| ViewSessionDetail
    ViewSessionDetail -->|Thực hiện| User
    
    ViewSessionDetail -.->|<<Extend>>| ViewRequestList
    ViewRequestList -->|Thực hiện| User
    
    ViewRequestList -.->|<<Extend>>| AcceptRequest
    AcceptRequest -->|Thực hiện| User
    
    ViewRequestList -.->|<<Extend>>| RejectRequest
    RejectRequest -->|Thực hiện| User
    
    ViewSessionDetail -.->|<<Extend>>| ViewPermissionInfo
    ViewPermissionInfo -->|Thực hiện| User
    
    ViewPermissionInfo -.->|<<Extend>>| ShareSession
    ShareSession -->|Thực hiện| User
    
    ViewPermissionInfo -.->|<<Extend>>| UpdatePermission
    UpdatePermission -->|Thực hiện| User
    
    ViewPermissionInfo -.->|<<Extend>>| RevokePermission
    RevokePermission -->|Thực hiện| User
    
    ViewPermissionInfo -.->|<<Extend>>| ManageLink
    ManageLink -->|Thực hiện| User
    
    User -->|Thực hiện| RequestAccess
    User -->|Thực hiện| CancelRequest
    
    style User fill:#e1f5ff
    style Login fill:#fff3e0
    style ListSessions fill:#f3e5f5
    style ViewSessionDetail fill:#fce4ec
    style ViewRequestList fill:#e0f2f1
    style ViewPermissionInfo fill:#e0f2f1
    style AcceptRequest fill:#e8f5e9
    style RejectRequest fill:#ffebee
    style ShareSession fill:#e8f5e9
    style UpdatePermission fill:#e8f5e9
    style RevokePermission fill:#ffebee
    style ManageLink fill:#e8f5e9
    style RequestAccess fill:#fce4ec
    style CancelRequest fill:#ffebee
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập hệ thống.
- Xem danh sách Session là bước cần thiết để có thể xem chi tiết Session.
- Xem chi tiết Session có hai extension points:
  - Xem danh sách yêu cầu truy cập (với extension points: Chấp nhận và Từ chối yêu cầu)
  - Xem các thông tin về quyền truy cập (với extension points: Chia sẻ, Cập nhật, Thu hồi quyền và Quản lý liên kết)
- User có thể yêu cầu và hủy yêu cầu truy cập khi chưa có quyền.
- Phân quyền cụ thể được xử lý bởi hệ thống quản lý quyền truy cập.
