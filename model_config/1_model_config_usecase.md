# Use Case Diagram - Quản lý ModelConfig và ModelWrapper

```mermaid
graph TB
    Actor["👤 Actor"]
    AdminActor["👤 Admin"]
    
    Login["Đăng nhập"]
    AccessModelPage["Xem trang Model<br/><b>extension points</b><br/>Xem danh sách ModelWrapper<br/>Xem danh sách ModelConfig"]
    
    ViewModelConfigList["Xem danh sách ModelConfig<br/><b>extension points</b><br/>Xóa ModelConfig<br/>Chỉnh sửa ModelConfig<br/>Tạo ModelConfig"]
    ViewWrapperList["Xem danh sách ModelWrapper<br/><b>extension points</b><br/>ExtensionPoint<br/>Chỉnh sửa ModelWrapper<br/>Xóa ModelWrapper"]
    
    CreateModelConfig["Tạo ModelConfig"]
    EditModelConfig["Chỉnh sửa ModelConfig"]
    DeleteModelConfig["Xóa ModelConfig"]
    
    CreateWrapper["Tạo ModelWrapper"]
    EditWrapper["Chỉnh sửa ModelWrapper"]
    DeleteWrapper["Xóa ModelWrapper"]
    
    Actor -->|Truy cập| Login
    AdminActor -->|Truy cập| Login
    
    Login -.->|<<Include>>| AccessModelPage
    AccessModelPage -->|Thực hiện| Actor
    AccessModelPage -->|Thực hiện| AdminActor
    
    AccessModelPage -.->|<<Extend>>| ViewModelConfigList
    ViewModelConfigList -->|Thực hiện| Actor
    
    AccessModelPage -.->|<<Extend>>| ViewWrapperList
    ViewWrapperList -->|Thực hiện| Actor
    ViewWrapperList -->|Thực hiện| AdminActor
    
    ViewModelConfigList -.->|<<Extend>>| CreateModelConfig
    CreateModelConfig -->|Thực hiện| Actor
    
    ViewModelConfigList -.->|<<Extend>>| EditModelConfig
    EditModelConfig -->|Thực hiện| Actor
    
    ViewModelConfigList -.->|<<Extend>>| DeleteModelConfig
    DeleteModelConfig -->|Thực hiện| Actor
    
    ViewWrapperList -.->|<<Extend>>| CreateWrapper
    CreateWrapper -->|Thỹc hiện| AdminActor
    
    ViewWrapperList -.->|<<Extend>>| EditWrapper
    EditWrapper -->|Thực hiện| AdminActor
    
    ViewWrapperList -.->|<<Extend>>| DeleteWrapper
    DeleteWrapper -->|Thực hiện| AdminActor
    
    style Actor fill:#e1f5ff
    style AdminActor fill:#ffe1f5
    style Login fill:#fff3e0
    style AccessModelPage fill:#f3e5f5
    style ViewModelConfigList fill:#e0f2f1
    style ViewWrapperList fill:#e0f2f1
    style EditModelConfig fill:#e8f5e9
    style CreateModelConfig fill:#e8f5e9
    style DeleteModelConfig fill:#ffebee
    style EditWrapper fill:#e8f5e9
    style CreateWrapper fill:#e8f5e9
    style DeleteWrapper fill:#ffebee
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập hệ thống.
- Xem trang Model có các extension points cho phép xem danh sách ModelConfig và ModelWrapper.
- Xem danh sách là bước đầu tiên trước khi thực hiện các thao tác quản lý.
- Các thao tác với Wrapper (tạo, chỉnh sửa, xóa) chỉ dành cho Admin.
