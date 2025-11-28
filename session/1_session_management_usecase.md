# Use Case Diagram - Quản lý Session

```mermaid
graph TB
    Actor["👤 User"]
    
    Login["Đăng nhập"]
    ListSessions["Xem danh sách Session<br/><b>extension points</b><br/>Xóa Session<br/>Tạo Session<br/>Xem chi tiết Session"]
    CreateSession["Tạo Session"]
    DeleteSession["Xóa Session"]
    ViewSessionDetail["Xem chi tiết Session<br/><b>extension points</b><br/>Chỉnh sửa Session"]
    UpdateSession["Chỉnh sửa Session"]
    
    Actor -->|Truy cập| Login
    Login -.->|<<Include>>| ListSessions
    ListSessions -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| CreateSession
    CreateSession -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| DeleteSession
    DeleteSession -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| ViewSessionDetail
    ViewSessionDetail -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| UpdateSession
    UpdateSession -->|Thực hiện| Actor
    
    style Actor fill:#e1f5ff
    style Login fill:#fff3e0
    style ListSessions fill:#f3e5f5
    style CreateSession fill:#e8f5e9
    style DeleteSession fill:#ffebee
    style ViewSessionDetail fill:#fce4ec
    style UpdateSession fill:#e0f2f1
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập các chức năng quản lý Session.
- Use Case "Xem danh sách Session" là điểm bắt đầu sau khi đăng nhập.
- Use Case "Xem danh sách Session" có các extension points: Tạo Session, Xóa Session, Xem chi tiết Session.
- Use Case "Xem chi tiết Session" có extension point: Chỉnh sửa Session.
