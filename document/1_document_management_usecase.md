# Use Case Diagram - Quản lý Document

```mermaid
graph TB
    Actor["👤 User"]
    
    Login["Đăng nhập"]
    ListSessions["Xem danh sách Session<br/><b>extension points</b><br/>Xem chi tiết Session"]
    ViewSessionDetail["Xem chi tiết Session<br/><b>extension points</b><br/>Upload Document<br/>Xem danh sách Document"]
    
    UploadDocument["Upload Document"]
    
    ViewDocumentList["Xem danh sách Document<br/><b>extension points</b><br/>Xóa Document<br/>Cập nhật Document"]
    UpdateDocument["Cập nhật Document"]
    DeleteDocument["Xóa Document"]
    
    Actor -->|Truy cập| Login
    Login -.->|<<Include>>| ListSessions
    ListSessions -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| ViewSessionDetail
    ViewSessionDetail -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| UploadDocument
    UploadDocument -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| ViewDocumentList
    ViewDocumentList -->|Thực hiện| Actor
    
    ViewDocumentList -.->|<<Extend>>| UpdateDocument
    UpdateDocument -->|Thực hiện| Actor
    
    ViewDocumentList -.->|<<Extend>>| DeleteDocument
    DeleteDocument -->|Thực hiện| Actor
    
    style Actor fill:#e1f5ff
    style Login fill:#fff3e0
    style ListSessions fill:#f3e5f5
    style ViewSessionDetail fill:#fce4ec
    style UploadDocument fill:#e8f5e9
    style ViewDocumentList fill:#e8f5e9
    style UpdateDocument fill:#e8f5e9
    style DeleteDocument fill:#ffebee
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập hệ thống.
- Xem danh sách Session là bước cần thiết để có thể xem chi tiết Session.
- Xem chi tiết Session có các extension points cho Upload và Xem danh sách Document.
- Xem danh sách Document có các extension points cho Cập nhật và Xóa Document.
- Các thao tác được thực hiện trong SideBarLeft và các dialog tương ứng.
- Upload Document hỗ trợ cả file và URL.
