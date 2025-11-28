# Use Case Diagram - Quản lý ModelConfig và ModelWrapper

```mermaid
graph TB
    Actor["👤 User"]
    AdminActor["👤 Admin"]
    
    Login["Đăng nhập"]
    
    ViewModelConfigList["Xem danh sách ModelConfig<br/><b>extension points</b><br/>Tạo ModelConfig<br/>Chỉnh sửa ModelConfig<br/>Xóa ModelConfig<br/>Preload ModelConfig"]
    CreateModelConfig["Tạo ModelConfig"]
    EditModelConfig["Chỉnh sửa ModelConfig"]
    DeleteModelConfig["Xóa ModelConfig"]
    PreloadModelConfig["Preload ModelConfig"]
    
    ViewWrapperList["Xem danh sách ModelWrapper<br/><b>extension points</b><br/>Tạo ModelWrapper<br/>Chỉnh sửa ModelWrapper<br/>Xóa ModelWrapper"]
    CreateWrapper["Tạo ModelWrapper"]
    EditWrapper["Chỉnh sửa ModelWrapper"]
    DeleteWrapper["Xóa ModelWrapper"]
    
    Actor -->|Truy cập| Login
    AdminActor -->|Truy cập| Login
    
    Login -.->|<<Include>>| ViewModelConfigList
    Login -.->|<<Include>>| ViewWrapperList
    
    ViewModelConfigList -->|Thực hiện| Actor
    
    ViewModelConfigList -.->|<<Extend>>| CreateModelConfig
    CreateModelConfig -->|Thực hiện| Actor
    
    ViewModelConfigList -.->|<<Extend>>| EditModelConfig
    EditModelConfig -->|Thực hiện| Actor
    
    ViewModelConfigList -.->|<<Extend>>| DeleteModelConfig
    DeleteModelConfig -->|Thực hiện| Actor
    
    ViewModelConfigList -.->|<<Extend>>| PreloadModelConfig
    PreloadModelConfig -->|Thực hiện| Actor
    
    ViewWrapperList -->|Thực hiện| Actor
    
    ViewWrapperList -.->|<<Extend>>| CreateWrapper
    CreateWrapper -->|Thực hiện| AdminActor
    
    ViewWrapperList -.->|<<Extend>>| EditWrapper
    EditWrapper -->|Thực hiện| AdminActor
    
    ViewWrapperList -.->|<<Extend>>| DeleteWrapper
    DeleteWrapper -->|Thực hiện| AdminActor
    
    style Actor fill:#e1f5ff
    style AdminActor fill:#ffe1f5
    style Login fill:#fff3e0
    style ViewModelConfigList fill:#e0f2f1
    style ViewWrapperList fill:#e0f2f1
    style CreateModelConfig fill:#e8f5e9
    style EditModelConfig fill:#e8f5e9
    style DeleteModelConfig fill:#ffebee
    style PreloadModelConfig fill:#fff9c4
    style CreateWrapper fill:#e8f5e9
    style EditWrapper fill:#e8f5e9
    style DeleteWrapper fill:#ffebee
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập hệ thống, được thực hiện bởi cả User và Admin.
- Sau khi đăng nhập, hệ thống tự động include (bao gồm) việc hiển thị cả danh sách ModelConfig và ModelWrapper.
- Xem danh sách ModelConfig có các extension points cho Tạo, Chỉnh sửa, Xóa và Preload ModelConfig.
- Preload ModelConfig chỉ xuất hiện khi User chưa có ModelConfig nào trong hệ thống.
- Xem danh sách ModelWrapper có các extension points cho Tạo, Chỉnh sửa và Xóa ModelWrapper.
- Các thao tác với Wrapper (tạo, chỉnh sửa, xóa) chỉ dành cho Admin.
