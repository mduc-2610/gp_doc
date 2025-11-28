# Use Case Diagram - Quản lý Document

```mermaid
graph TB
    Actor["👤 User"]
    
    Login["Đăng nhập"]
    ListSessions["Xem danh sách Session<br/><b>extension points</b><br/>Xem chi tiết Session"]
    ViewSessionDetail["Xem chi tiết Session<br/><b>extension points</b><br/>Upload Document<br/>Xem danh sách Document"]
    UploadDocument["Upload Document"]
    ListDocuments["Xem danh sách Document<br/><b>extension points</b><br/>Xóa Document<br/>Cập nhật Document"]
    DeleteDocument["Xóa Document"]
    UpdateDocument["Cập nhật Document"]
    
    Actor -->|Truy cập| Login
    Login -.->|<<Include>>| ListSessions
    ListSessions -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| ViewSessionDetail
    ViewSessionDetail -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| UploadDocument
    UploadDocument -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| ListDocuments
    ListDocuments -->|Thực hiện| Actor
    
    ListDocuments -.->|<<Extend>>| DeleteDocument
    DeleteDocument -->|Thực hiện| Actor
    
    ListDocuments -.->|<<Extend>>| UpdateDocument
    UpdateDocument -->|Thực hiện| Actor
    
    style Actor fill:#e1f5ff
    style Login fill:#fff3e0
    style ListSessions fill:#f3e5f5
    style ViewSessionDetail fill:#fce4ec
    style UploadDocument fill:#e8f5e9
    style ListDocuments fill:#e0f7fa
    style DeleteDocument fill:#ffebee
    style UpdateDocument fill:#e0f2f1
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập các chức năng quản lý Document.
- Use Case "Xem danh sách Session" là điểm bắt đầu sau khi đăng nhập.
- Use Case "Xem chi tiết Session" có extension points cho phép User thực hiện: Upload Document và Xem danh sách Document.
- Use Case "Xem danh sách Document" có extension points cho phép Xóa và Cập nhật Document.
