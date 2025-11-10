# Use Case Diagram - Quản lý các Session

```mermaid
graph TB
    Actor["👤 User"]
    
    Login["Đăng nhập"]
    ListSessions["Xem danh sách Session"]
    CreateSession["Tạo Session mới"]
    ViewSessionDetail["Xem chi tiết Session"]
    EditSession["Chỉnh sửa Session"]
    DeleteSession["Xóa Session"]
    
    Actor -->|Truy cập| Login
    Login -->|include| ListSessions
    ListSessions -->|Thực hiện| Actor
    
    ListSessions -->|include| CreateSession
    CreateSession -->|Thực hiện| Actor
    
    ListSessions -->|include| ViewSessionDetail
    ViewSessionDetail -->|Thực hiện| Actor
    
    ViewSessionDetail -->|include| EditSession
    EditSession -->|Thực hiện| Actor
    
    ListSessions -->|include| DeleteSession
    DeleteSession -->|Thực hiện| Actor
    
    style Actor fill:#e1f5ff
    style Login fill:#fff3e0
    style ListSessions fill:#f3e5f5
    style CreateSession fill:#e8f5e9
    style ViewSessionDetail fill:#fce4ec
    style EditSession fill:#e0f2f1
    style DeleteSession fill:#ffebee
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập các chức năng quản lý Session.
- Use Case "Xem danh sách Session" là trung tâm, từ đó User có thể thực hiện các thao tác: Tạo, Xem chi tiết, Chỉnh sửa, và Xóa Session.
