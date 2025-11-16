````markdown

# Use Case Diagram - Quản lý Session Access

```mermaid
graph TB
    Manager["👤 Manager"]
    User["👤 User"]
    
    AccessSessionDetail["Truy cập chi tiết Session"]
    
    ShareSession["Chia sẻ Session với User"]
    UpdatePermission["Cập nhật quyền truy cập"]
    RevokePermission["Thu hồi quyền truy cập"]
    HandleRequest["Xử lý yêu cầu truy cập"]
    ManageLink["Quản lý liên kết chia sẻ"]
    
    RequestAccess["Yêu cầu truy cập Session"]
    CancelRequest["Hủy yêu cầu truy cập"]
    
    ReceiveUpdate["Nhận cập nhật quyền realtime"]
    
    Manager -->|Truy cập| AccessSessionDetail
    User -->|Truy cập| AccessSessionDetail
    
    AccessSessionDetail -->|include| ShareSession
    ShareSession -->|Thực hiện| Manager
    
    AccessSessionDetail -->|include| UpdatePermission
    UpdatePermission -->|Thực hiện| Manager
    
    AccessSessionDetail -->|include| RevokePermission
    RevokePermission -->|Thực hiện| Manager
    
    AccessSessionDetail -->|include| HandleRequest
    HandleRequest -->|Thực hiện| Manager
    
    AccessSessionDetail -->|include| ManageLink
    ManageLink -->|Thực hiện| Manager
    
    RequestAccess -->|Thực hiện| User
    
    CancelRequest -->|Thực hiện| User
    
    AccessSessionDetail -->|extend| ReceiveUpdate
    ReceiveUpdate -->|Nhận| User
    ReceiveUpdate -->|Nhận| Manager
    
    style Manager fill:#e1f5ff
    style User fill:#e1f5ff
    style AccessSessionDetail fill:#fff3e0
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
- Manager có toàn quyền quản lý chia sẻ Session.
- User có thể yêu cầu và hủy yêu cầu truy cập.
- Cả Manager và User đều nhận thông báo realtime khi quyền thay đổi.
- Truy cập chi tiết Session là điều kiện tiên quyết cho các thao tác quản lý.


````