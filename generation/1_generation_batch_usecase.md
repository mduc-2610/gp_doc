# Use Case Diagram - Generation với Batch

```mermaid
graph TB
    Actor["👤 User"]
    
    Login["Đăng nhập"]
    ListSessions["Xem danh sách Session<br/><b>extension points</b><br/>Xem chi tiết Session"]
    ViewSessionDetail["Xem chi tiết Session<br/><b>extension points</b><br/>ExtensionPoint<br/>Xem kết quả batch"]
    
    CreateGenRequest["Tạo yêu cầu Generation<br/><b>extension points</b><br/>Hủy tiến trình Generation"]
    TrackProgress["Theo dõi tiến trình Generation"]
    CancelProcess["Hủy tiến trình Generation"]
    
    ViewBatchResult["Xem kết quả batch<br/><b>extension points</b><br/>Duyệt kết quả Batch<br/>Tạo yêu cầu regenerate<br/>Từ chối kết quả Batch"]
    ApproveBatch["Duyệt kết quả Batch"]
    RejectBatch["Từ chối kết quả Batch"]
    RegenerateRequest["Tạo yêu cầu regenerate"]
    
    Actor -->|Truy cập| Login
    Login -.->|<<Include>>| ListSessions
    ListSessions -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| ViewSessionDetail
    ViewSessionDetail -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| CreateGenRequest
    CreateGenRequest -->|Thực hiện| Actor
    
    CreateGenRequest -.->|<<Include>>| TrackProgress
    TrackProgress -->|Thực hiện| Actor
    
    CreateGenRequest -.->|<<Extend>>| CancelProcess
    CancelProcess -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| ViewBatchResult
    ViewBatchResult -->|Thực hiện| Actor
    
    ViewBatchResult -.->|<<Extend>>| ApproveBatch
    ApproveBatch -->|Thực hiện| Actor
    
    ViewBatchResult -.->|<<Extend>>| RejectBatch
    RejectBatch -->|Thực hiện| Actor
    
    ViewBatchResult -.->|<<Extend>>| RegenerateRequest
    RegenerateRequest -->|Thực hiện| Actor
    
    style Actor fill:#e1f5ff
    style Login fill:#fff3e0
    style ListSessions fill:#f3e5f5
    style ViewSessionDetail fill:#fce4ec
    style CreateGenRequest fill:#e8f5e9
    style TrackProgress fill:#e3f2fd
    style CancelProcess fill:#ffebee
    style ViewBatchResult fill:#f3e5f5
    style ApproveBatch fill:#e8f5e9
    style RejectBatch fill:#ffebee
    style RegenerateRequest fill:#fff9c4
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập hệ thống.
- Xem danh sách Session là bước cần thiết để có thể xem chi tiết Session.
- Xem chi tiết Session có các extension points cho các chức năng generation.
- Tạo yêu cầu Generation bao gồm Theo dõi tiến trình và có extension point Hủy tiến trình.
- Xem kết quả batch có các extension points cho Duyệt, Từ chối và Regenerate.
- User có thể reject và regenerate để cải thiện kết quả.
