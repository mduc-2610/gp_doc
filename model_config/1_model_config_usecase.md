# Use Case Diagram - Cấu hình Model

```mermaid
graph TB
    UserActor["👤 User"]
    AdminActor["🔐 Admin (User + ADMIN role)"]
    
    ListModels["Xem danh sách Model"]
    CreateModel["Tạo Model mới"]
    EditModel["Chỉnh sửa Model"]
    DeleteModel["Xóa Model"]
    
    ListWrappers["Xem danh sách Model Wrapper"]
    CreateWrapper["Tạo Model Wrapper mới"]
    EditWrapper["Chỉnh sửa Model Wrapper"]
    DeleteWrapper["Xóa Model Wrapper"]
    
    %% User có thể xem và quản lý Model của mình
    UserActor -->|Truy cập| ListModels
    ListModels -->|Thực hiện| UserActor
    
    ListModels -->|include| CreateModel
    CreateModel -->|Thực hiện| UserActor
    
    ListModels -->|include| EditModel
    EditModel -->|Thực hiện| UserActor
    
    ListModels -->|include| DeleteModel
    DeleteModel -->|Thực hiện| UserActor
    
    %% User có thể xem danh sách Model Wrapper
    UserActor -->|Truy cập| ListWrappers
    ListWrappers -->|Thực hiện| UserActor
    
    %% Admin có thể tạo, sửa, xóa Wrapper (Admin inherits User permissions)
    AdminActor -->|Truy cập| ListWrappers
    
    ListWrappers -->|include| CreateWrapper
    CreateWrapper -->|Thực hiện| AdminActor
    
    ListWrappers -->|include| EditWrapper
    EditWrapper -->|Thực hiện| AdminActor
    
    ListWrappers -->|include| DeleteWrapper
    DeleteWrapper -->|Thực hiện| AdminActor
    
    style UserActor fill:#e1f5ff
    style AdminActor fill:#fff3e0
    style ListModels fill:#f3e5f5
    style CreateModel fill:#e8f5e9
    style EditModel fill:#e0f2f1
    style DeleteModel fill:#ffebee
    style ListWrappers fill:#f3e5f5
    style CreateWrapper fill:#e8f5e9
    style EditWrapper fill:#e0f2f1
    style DeleteWrapper fill:#ffebee
```

**Ghi chú:**
- **User** có thể thực hiện các thao tác:
  - Xem, Tạo, Chỉnh sửa, Xóa Model của mình
  - Xem danh sách Model Wrapper
- **Admin** (User có role ADMIN) có thể thực hiện:
  - Tất cả quyền của User
  - Tạo, Chỉnh sửa, Xóa Model Wrapper
