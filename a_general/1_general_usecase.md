# Use Case Diagram - Tổng quan hệ thống DLab

```mermaid
graph TB
    Actor["👤 User"]
    
    Login["Đăng nhập"]
    ManageSession["Quản lý Session"]
    ManageDocument["Quản lý Document"]
    ManageQuestion["Quản lý Question và Flashcard"]
    Generation["Generation"]
    ManageModel["Quản lý ModelConfig"]
    ManageAccess["Quản lý Session Access"]
    
    Actor -->|Truy cập| Login
    Login -->|include| ManageSession
    ManageSession -->|Thực hiện| Actor
    
    Login -->|include| ManageDocument
    ManageDocument -->|Thực hiện| Actor
    
    Login -->|include| ManageQuestion
    ManageQuestion -->|Thực hiện| Actor
    
    Login -->|include| Generation
    Generation -->|Thực hiện| Actor
    
    Login -->|include| ManageModel
    ManageModel -->|Thực hiện| Actor
    
    Login -->|include| ManageAccess
    ManageAccess -->|Thực hiện| Actor
    
    style Actor fill:#e1f5ff
    style Login fill:#fff3e0
    style ManageSession fill:#f3e5f5
    style ManageDocument fill:#e8f5e9
    style ManageQuestion fill:#fce4ec
    style Generation fill:#e3f2fd
    style ManageModel fill:#fff9c4
    style ManageAccess fill:#f3e5f5
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập tất cả các chức năng của hệ thống DLab.
- Mỗi module quản lý đại diện cho một nhóm chức năng liên quan, được mô tả chi tiết trong các folder tương ứng.
- Các module được thiết kế để hoạt động độc lập nhưng có thể tương tác với nhau thông qua Session.