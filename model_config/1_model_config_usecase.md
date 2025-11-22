# Use Case Diagram - Quản lý ModelConfig và ModelWrapper

```mermaid
graph TB
    Actor["👤 User"]
    AdminActor["👤 Admin"]
    
    AccessModelConfigPage["Truy cập trang cấu hình Model"]
    ViewModelConfigList["Xem danh sách ModelConfig"]
    ViewWrapperList["Xem danh sách ModelWrapper"]
    EditModelConfig["Chỉnh sửa ModelConfig"]
    CreateModelConfig["Tạo ModelConfig mới"]
    DeleteModelConfig["Xóa ModelConfig"]
    ToggleModelConfig["Toggle trạng thái ModelConfig"]
    
    EditWrapper["Chỉnh sửa ModelWrapper"]
    CreateWrapper["Tạo ModelWrapper mới"]
    DeleteWrapper["Xóa ModelWrapper"]
    PreloadModelConfig["Preload ModelConfig"]
    
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
- Truy cập trang cấu hình Model là điều kiện tiên quyết để sử dụng các chức năng ModelConfig và Wrapper.
- Xem danh sách là bước đầu tiên trước khi thực hiện các thao tác quản lý.
- Các thao tác với Wrapper (tạo, chỉnh sửa, xóa) chỉ dành cho Admin.
