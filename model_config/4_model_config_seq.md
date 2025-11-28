# Sequence Diagram - Quản lý ModelConfig và ModelWrapper

## 1. Xem danh sách ModelConfig

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes.py
    participant ModelConfigService as ModelConfigService
    participant ModelConfig as ModelConfig

    User->>ModelConfigTab: Click "Cấu hình mô hình" tab
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigTab: loadModelsFromUser()
    ModelConfigTab->>ModelConfigService: gọi
    deactivate ModelConfigTab
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: getModelsByUser()
    ModelConfigService->>ModelConfigRoutes: GET "/model-config/by-user/{user_id}"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: get_models_by_user()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelConfig: gọi
    activate ModelConfig
    
    ModelConfig->>ModelConfig: ModelConfig()
    ModelConfig-->>ModelConfigService: trả về
    deactivate ModelConfig
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigTab: trả về
    deactivate ModelConfigService
    
    ModelConfigTab-->>User: Hiển thị
```

---

## 2. Tạo ModelConfig mới

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigCreateDialog as ModelConfigCreateDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes.py
    participant ModelConfigService as ModelConfigService
    participant ModelConfig as ModelConfig
    participant ModelWrapper as ModelWrapper

    User->>ModelConfigTab: Click "Tạo mô hình"
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigCreateDialog: gọi
    deactivate ModelConfigTab
    activate ModelConfigCreateDialog
    
    ModelConfigCreateDialog->>ModelConfigCreateDialog: loadWrappers()
    ModelConfigCreateDialog->>ModelConfigService: gọi
    deactivate ModelConfigCreateDialog
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: getWrappers()
    ModelConfigService->>ModelConfigRoutes: GET "/model-config/wrapper"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: get_wrappers()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelWrapper: gọi
    activate ModelWrapper
    
    ModelWrapper->>ModelWrapper: ModelWrapper()
    ModelWrapper-->>ModelConfigService: trả về
    deactivate ModelWrapper
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>User: Hiển thị
    deactivate ModelConfigService
    
    User->>ModelConfigCreateDialog: Nhập thông tin và click "Lưu thay đổi"
    activate ModelConfigCreateDialog
    
    ModelConfigCreateDialog->>ModelConfigCreateDialog: validate()
    ModelConfigCreateDialog->>ModelConfigCreateDialog: handleCreateModel()
    ModelConfigCreateDialog->>ModelConfigService: gọi
    deactivate ModelConfigCreateDialog
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: createModel()
    ModelConfigService->>ModelConfigRoutes: POST "/model-config"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: create_model()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelConfig: gọi
    activate ModelConfig
    
    ModelConfig->>ModelConfig: ModelConfig()
    ModelConfig-->>ModelConfigService: trả về
    deactivate ModelConfig
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigTab: trả về
    deactivate ModelConfigService
    
    ModelConfigTab-->>User: Hiển thị
```

---

## 3. Chỉnh sửa ModelConfig

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigUpdateDialog as ModelConfigUpdateDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes.py
    participant ModelConfigService as ModelConfigService
    participant ModelConfig as ModelConfig
    participant ModelWrapper as ModelWrapper

    User->>ModelConfigTab: Click icon update trên một ModelConfig
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigUpdateDialog: gọi
    deactivate ModelConfigTab
    activate ModelConfigUpdateDialog
    
    ModelConfigUpdateDialog->>ModelConfigUpdateDialog: loadWrappers()
    ModelConfigUpdateDialog->>ModelConfigService: gọi
    deactivate ModelConfigUpdateDialog
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: getWrappers()
    ModelConfigService->>ModelConfigRoutes: GET "/model-config/wrapper"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: get_wrappers()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelWrapper: gọi
    activate ModelWrapper
    
    ModelWrapper->>ModelWrapper: ModelWrapper()
    ModelWrapper-->>ModelConfigService: trả về
    deactivate ModelWrapper
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>User: Hiển thị
    deactivate ModelConfigService
    
    User->>ModelConfigUpdateDialog: Nhập thông tin và click "Lưu thay đổi"
    activate ModelConfigUpdateDialog
    
    ModelConfigUpdateDialog->>ModelConfigUpdateDialog: validate()
    ModelConfigUpdateDialog->>ModelConfigUpdateDialog: handleUpdateModel()
    ModelConfigUpdateDialog->>ModelConfigService: gọi
    deactivate ModelConfigUpdateDialog
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: updateModel()
    ModelConfigService->>ModelConfigRoutes: PUT "/model-config/{model_id}"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: update_model()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelConfig: gọi
    activate ModelConfig
    
    ModelConfig->>ModelConfig: ModelConfig()
    ModelConfig-->>ModelConfigService: trả về
    deactivate ModelConfig
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigTab: trả về
    deactivate ModelConfigService
    
    ModelConfigTab-->>User: Hiển thị
```

---

## 4. Xóa ModelConfig

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigUpdateDialog as ModelConfigUpdateDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes.py
    participant ModelConfigService as ModelConfigService
    participant MessageResponse as MessageResponse

    User->>ModelConfigTab: Click icon delete trên một ModelConfig
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigUpdateDialog: gọi
    deactivate ModelConfigTab
    activate ModelConfigUpdateDialog
    ModelConfigUpdateDialog-->>User: Hiển thị
    deactivate ModelConfigUpdateDialog
    
    User->>ModelConfigUpdateDialog: Click "Xác nhận"
    activate ModelConfigUpdateDialog
    
    ModelConfigUpdateDialog->>ModelConfigUpdateDialog: handleDeleteModel()
    ModelConfigUpdateDialog->>ModelConfigService: gọi
    deactivate ModelConfigUpdateDialog
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: deleteModel()
    ModelConfigService->>ModelConfigRoutes: DELETE "/model-config/{model_id}"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: delete_model()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>MessageResponse: gọi
    activate MessageResponse
    
    MessageResponse->>MessageResponse: MessageResponse()
    MessageResponse-->>ModelConfigService: trả về
    deactivate MessageResponse
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigTab: trả về
    deactivate ModelConfigService
    
    ModelConfigTab-->>User: Hiển thị
```

---

## 5. Preload ModelConfig

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes.py
    participant ModelConfigService as ModelConfigService
    participant ModelConfig as ModelConfig
    participant MessageResponse as MessageResponse

    User->>ModelConfigPage: Click "Tải mô hình mẫu" và xác nhận
    activate ModelConfigPage
    
    ModelConfigPage->>ModelConfigPage: handlePreloadModels()
    ModelConfigPage->>ModelConfigService: gọi
    deactivate ModelConfigPage
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: preloadModels()
    ModelConfigService->>ModelConfigRoutes: GET "/model-config/preload/{user_id}"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: preload_models()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelConfig: gọi
    activate ModelConfig
    
    ModelConfig->>ModelConfig: ModelConfig()
    ModelConfig-->>ModelConfigService: trả về
    deactivate ModelConfig
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigPage: trả về
    deactivate ModelConfigService
    
    ModelConfigPage->>ModelConfigPage: loadModelsFromUser()
    ModelConfigPage->>ModelConfigService: gọi
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: getModelsByUser()
    ModelConfigService->>ModelConfigRoutes: GET "/model-config/by-user/{user_id}"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: get_models_by_user()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelConfig: gọi
    activate ModelConfig
    
    ModelConfig->>ModelConfig: ModelConfig()
    ModelConfig-->>ModelConfigService: trả về
    deactivate ModelConfig
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigPage: trả về
    deactivate ModelConfigService
    
    ModelConfigPage-->>User: Hiển thị
```

---

## 6. Xem danh sách ModelWrapper

```mermaid
sequenceDiagram
    actor User
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes.py
    participant ModelConfigService as ModelConfigService
    participant ModelWrapper as ModelWrapper

    User->>ModelWrapperTab: Click "Cấu hình Wrapper" tab
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelWrapperTab: loadWrappers()
    ModelWrapperTab->>ModelConfigService: gọi
    deactivate ModelWrapperTab
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: getWrappers()
    ModelConfigService->>ModelConfigRoutes: GET "/model-config/wrapper"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: get_wrappers()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelWrapper: gọi
    activate ModelWrapper
    
    ModelWrapper->>ModelWrapper: ModelWrapper()
    ModelWrapper-->>ModelConfigService: trả về
    deactivate ModelWrapper
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelWrapperTab: trả về
    deactivate ModelConfigService
    
    ModelWrapperTab-->>User: Hiển thị
```

---

## 7. Tạo ModelWrapper mới (Admin only)

```mermaid
sequenceDiagram
    actor Admin
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelWrapperCreateDialog as ModelWrapperCreateDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes.py
    participant ModelConfigService as ModelConfigService
    participant ModelWrapper as ModelWrapper

    Admin->>ModelWrapperTab: Click "Tạo Wrapper"
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelWrapperCreateDialog: gọi
    deactivate ModelWrapperTab
    activate ModelWrapperCreateDialog
    ModelWrapperCreateDialog-->>Admin: Hiển thị
    deactivate ModelWrapperCreateDialog
    
    Admin->>ModelWrapperCreateDialog: Nhập thông tin và click "Tạo"
    activate ModelWrapperCreateDialog
    
    ModelWrapperCreateDialog->>ModelWrapperCreateDialog: validate()
    ModelWrapperCreateDialog->>ModelWrapperCreateDialog: handleCreateWrapper()
    ModelWrapperCreateDialog->>ModelConfigService: gọi
    deactivate ModelWrapperCreateDialog
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: createWrapper()
    ModelConfigService->>ModelConfigRoutes: POST "/model-config/wrapper"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: create_wrapper()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelWrapper: gọi
    activate ModelWrapper
    
    ModelWrapper->>ModelWrapper: ModelWrapper()
    ModelWrapper-->>ModelConfigService: trả về
    deactivate ModelWrapper
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelWrapperTab: trả về
    deactivate ModelConfigService
    
    ModelWrapperTab-->>Admin: Hiển thị
```

---

## 8. Chỉnh sửa ModelWrapper (Admin only)

```mermaid
sequenceDiagram
    actor Admin
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelWrapperUdateDialog as ModelWrapperUdateDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes.py
    participant ModelConfigService as ModelConfigService
    participant ModelWrapper as ModelWrapper

    Admin->>ModelWrapperTab: Click icon update trên một ModelWrapper
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelWrapperUdateDialog: gọi
    deactivate ModelWrapperTab
    activate ModelWrapperUdateDialog
    ModelWrapperUdateDialog-->>Admin: Hiển thị
    deactivate ModelWrapperUdateDialog
    
    Admin->>ModelWrapperUdateDialog: Nhập thông tin và click "Cập nhật"
    activate ModelWrapperUdateDialog
    
    ModelWrapperUdateDialog->>ModelWrapperUdateDialog: validate()
    ModelWrapperUdateDialog->>ModelWrapperUdateDialog: handleUpdateWrapper()
    ModelWrapperUdateDialog->>ModelConfigService: gọi
    deactivate ModelWrapperUdateDialog
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: updateWrapper()
    ModelConfigService->>ModelConfigRoutes: PUT "/model-config/wrapper/{wrapper_id}"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: update_wrapper()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelWrapper: gọi
    activate ModelWrapper
    
    ModelWrapper->>ModelWrapper: ModelWrapper()
    ModelWrapper-->>ModelConfigService: trả về
    deactivate ModelWrapper
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelWrapperTab: trả về
    deactivate ModelConfigService
    
    ModelWrapperTab-->>Admin: Hiển thị
```

---

## 9. Xóa ModelWrapper (Admin only)

```mermaid
sequenceDiagram
    actor Admin
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelWrapperDeleteDialog as ModelWrapperDeleteDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes.py
    participant ModelConfigService as ModelConfigService
    participant MessageResponse as MessageResponse

    Admin->>ModelWrapperTab: Click icon delete trên một ModelWrapper
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelWrapperDeleteDialog: gọi
    deactivate ModelWrapperTab
    activate ModelWrapperDeleteDialog
    ModelWrapperDeleteDialog-->>Admin: Hiển thị
    deactivate ModelWrapperDeleteDialog
    
    Admin->>ModelWrapperDeleteDialog: Nhập thông tin và click "Cập nhật"
    activate ModelWrapperDeleteDialog
    
    ModelWrapperDeleteDialog->>ModelWrapperDeleteDialog: handleDeleteWrapper()
    ModelWrapperDeleteDialog->>ModelConfigService: gọi
    deactivate ModelWrapperDeleteDialog
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: deleteWrapper()
    ModelConfigService->>ModelConfigRoutes: DELETE "/model-config/wrapper/{wrapper_id}"
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: delete_wrapper()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>MessageResponse: gọi
    activate MessageResponse
    
    MessageResponse->>MessageResponse: MessageResponse()
    MessageResponse-->>ModelConfigService: trả về
    deactivate MessageResponse
    
    ModelConfigService-->>ModelConfigRoutes: trả về
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigService: trả về
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelWrapperTab: trả về
    deactivate ModelConfigService
    
    ModelWrapperTab-->>Admin: Hiển thị
```
