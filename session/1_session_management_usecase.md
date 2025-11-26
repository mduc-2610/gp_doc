# Use Case Diagram - Quản lý các Session

```mermaid
graph TB
    Actor["👤 User"]
    
    Login["Đăng nhập"]
    ListSessions["Xem danh sách Session<br/><b>extension points</b><br/>Xóa Session<br/>Tạo Session<br/>Xem chi tiết Session"]
    CreateSession["Tạo Session"]
    ViewSessionDetail["Xem chi tiết Session<br/><b>extension points</b><br/>Chỉnh sửa Session"]
    EditSession["Chỉnh sửa Session"]
    DeleteSession["Xóa Session"]
    
    Actor -->|Truy cập| Login
    Login -.->|<<Include>>| ListSessions
    ListSessions -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| CreateSession
    CreateSession -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| ViewSessionDetail
    ViewSessionDetail -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| EditSession
    EditSession -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| DeleteSession
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
- Use Case "Xem danh sách Session" là trung tâm với các extension points cho phép User thực hiện: Tạo, Xem chi tiết, và Xóa Session.
- Use Case "Xem chi tiết Session" có extension point cho phép Chỉnh sửa Session.
