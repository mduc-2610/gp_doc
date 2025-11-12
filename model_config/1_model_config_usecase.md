# Use Case Diagram - Quản lý Model Config và Model Wrapper

```mermaid
graph TB
    Actor["👤 User"]
    AdminActor["👤 Admin"]
    
    AccessModelConfigPage["Truy cập trang cấu hình Model"]
    ViewModelConfigList["Xem danh sách Model Config"]
    ViewWrapperList["Xem danh sách Model Wrapper"]
    EditModelConfig["Chỉnh sửa Model Config"]
    CreateModelConfig["Tạo Model Config mới"]
    DeleteModelConfig["Xóa Model Config"]
    ToggleModelConfig["Toggle trạng thái Model Config"]
    
    EditWrapper["Chỉnh sửa Model Wrapper"]
    CreateWrapper["Tạo Model Wrapper mới"]
    DeleteWrapper["Xóa Model Wrapper"]
    PreloadModelConfig["Preload Model Config"]
    
    Actor -->|Truy cập| AccessModelConfigPage
    AdminActor -->|Truy cập| AccessModelConfigPage
    
    AccessModelConfigPage -->|include| ViewModelConfigList
    ViewModelConfigList -->|Thực hiện| Actor
    
    AccessModelConfigPage -->|include| ViewWrapperList
    ViewWrapperList -->|Thực hiện| Actor
    ViewWrapperList -->|Thực hiện| AdminActor
    
    ViewModelConfigList -->|include| EditModelConfig
    EditModelConfig -->|Thực hiện| Actor
    
    ViewModelConfigList -->|include| CreateModelConfig
    CreateModelConfig -->|Thực hiện| Actor
    
    ViewModelConfigList -->|include| DeleteModelConfig
    DeleteModelConfig -->|Thực hiện| Actor
    
    ViewModelConfigList -->|include| ToggleModelConfig
    ToggleModelConfig -->|Thực hiện| Actor
    
    ViewModelConfigList -->|include| PreloadModelConfig
    PreloadModelConfig -->|Thực hiện| Actor
    
    ViewWrapperList -->|include| EditWrapper
    EditWrapper -->|Thực hiện| AdminActor
    
    ViewWrapperList -->|include| CreateWrapper
    CreateWrapper -->|Thực hiện| AdminActor
    
    ViewWrapperList -->|include| DeleteWrapper
    DeleteWrapper -->|Thực hiện| AdminActor
    
    style Actor fill:#e1f5ff
    style AdminActor fill:#ffe1f5
    style AccessModelConfigPage fill:#fff3e0
    style ViewModelConfigList fill:#e0f2f1
    style ViewWrapperList fill:#e0f2f1
    style EditModelConfig fill:#e8f5e9
    style CreateModelConfig fill:#e8f5e9
    style DeleteModelConfig fill:#ffebee
    style ToggleModelConfig fill:#e3f2fd
    style EditWrapper fill:#e8f5e9
    style CreateWrapper fill:#e8f5e9
    style DeleteWrapper fill:#ffebee
    style PreloadModelConfig fill:#e0f2f1
```

**Ghi chú:**
- Truy cập trang cấu hình Model là điều kiện tiên quyết để sử dụng các chức năng Model Config và Wrapper.
- Xem danh sách là bước đầu tiên trước khi thực hiện các thao tác quản lý.
- Các thao tác với Wrapper (tạo, chỉnh sửa, xóa) chỉ dành cho Admin.
