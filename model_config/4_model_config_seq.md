# Sequence Diagram - Quản lý Model Config và Model Wrapper

## 1. Xem danh sách Model Config

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    User->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    
    ModelConfigPage-->>User: Hiển thị giao diện với tab "Cấu Hình Mô Hình"
    
    ModelConfigPage->>ModelConfigTab: Khởi tạo ModelConfigTab
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigRoutes: GET /model-config
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: get_models_by_user()
    activate ModelConfigService
    
    ModelConfigService-->>ModelConfigRoutes: List[ModelConfig]
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelConfigTab: List[ModelConfigResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigTab->>ModelConfigTab: Cập nhật state models với dữ liệu nhận được
    ModelConfigTab-->>User: Hiển thị danh sách Model Config
    deactivate ModelConfigTab
    deactivate ModelConfigPage
```

---

## 2. Tạo Model Config mới

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigCreateDialog as ModelConfigCreateDialog.tsx
    participant ModelWrapperTable as ModelWrapperTable.tsx
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    User->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    ModelConfigPage-->>User: Hiển thị giao diện
    
    User->>ModelConfigPage: Click tab "Model Configurations"
    ModelConfigPage->>ModelConfigTab: Khởi tạo ModelConfigTab
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigRoutes: GET /model-config
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_models_by_user()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelConfig]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTab: List[ModelConfigResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigTab-->>User: Hiển thị danh sách Model Config
    
    User->>ModelConfigTab: Chọn task type
    ModelConfigTab-->>User: Lọc và hiển thị models theo task type
    
    User->>ModelConfigTab: Click "Add Model"
    ModelConfigTab->>ModelConfigCreateDialog: Mở dialog
    activate ModelConfigCreateDialog
    
    ModelConfigCreateDialog->>ModelConfigRoutes: GET /model-config/wrapper?task_type=GENERATION
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_wrappers()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelWrapper]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigCreateDialog: List[ModelWrapperResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigCreateDialog->>ModelWrapperTable: Hiển thị bảng Wrapper
    activate ModelWrapperTable
    ModelWrapperTable-->>User: Hiển thị danh sách Wrapper
    
    User->>ModelWrapperTable: Chọn Wrapper (click Switch)
    ModelWrapperTable->>ModelConfigCreateDialog: Update selectedWrapperId
    deactivate ModelWrapperTable
    
    User->>ModelConfigCreateDialog: Nhập thông tin và click "Save"
    ModelConfigCreateDialog->>ModelConfigCreateDialog: Validate form
    ModelConfigCreateDialog->>ModelConfigTab: handleSave()
    
    ModelConfigTab->>ModelConfigRoutes: POST /model-config
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: create_model(create_data)
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: Create ModelConfig<br/>db.commit()
    
    ModelConfigService-->>ModelConfigRoutes: ModelConfig
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTab: ModelConfigResponse
    deactivate ModelConfigRoutes
    
    ModelConfigTab->>ModelConfigTab: Cập nhật danh sách với model mới
    ModelConfigTab-->>User: Hiển thị danh sách mới và thông báo "Model created successfully"
    
    deactivate ModelConfigCreateDialog
    deactivate ModelConfigTab
    deactivate ModelConfigPage
```

---

## 3. Chỉnh sửa Model Config

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigUpdateDialog as ModelConfigUpdateDialog.tsx
    participant ModelWrapperTable as ModelWrapperTable.tsx
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    User->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    ModelConfigPage-->>User: Hiển thị giao diện
    
    User->>ModelConfigPage: Click tab "Model Configurations"
    ModelConfigPage->>ModelConfigTab: Khởi tạo ModelConfigTab
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigRoutes: GET /model-config
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_models_by_user()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelConfig]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTab: List[ModelConfigResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigTab-->>User: Hiển thị danh sách Model Config
    
    User->>ModelConfigTab: Chọn task type và xem danh sách Model Config
    ModelConfigTab-->>User: Lọc và hiển thị models theo task type
    
    User->>ModelConfigTab: Click "Edit" trên Model Config
    ModelConfigTab->>ModelConfigUpdateDialog: Mở dialog với dữ liệu hiện tại
    activate ModelConfigUpdateDialog
    
    ModelConfigUpdateDialog->>ModelConfigRoutes: GET /model-config/wrapper?task_type=GENERATION
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_wrappers()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelWrapper]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigUpdateDialog: List[ModelWrapperResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigUpdateDialog->>ModelWrapperTable: Hiển thị bảng với Wrapper hiện tại
    activate ModelWrapperTable
    ModelWrapperTable-->>User: Hiển thị danh sách Wrapper
    
    opt Thay đổi Wrapper
        User->>ModelWrapperTable: Chọn Wrapper khác
        ModelWrapperTable->>ModelConfigUpdateDialog: Update selectedWrapperId
    end
    deactivate ModelWrapperTable
    
    User->>ModelConfigUpdateDialog: Cập nhật thông tin và click "Save"
    ModelConfigUpdateDialog->>ModelConfigUpdateDialog: Validate form
    ModelConfigUpdateDialog->>ModelConfigTab: handleSave()
    
    ModelConfigTab->>ModelConfigRoutes: PUT /model-config/{model_id}
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: update_model()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: Get ModelConfig<br/>Update fields<br/>db.commit()
    
    ModelConfigService-->>ModelConfigRoutes: ModelConfig
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTab: ModelConfigResponse
    deactivate ModelConfigRoutes
    
    ModelConfigTab->>ModelConfigTab: Cập nhật item trong danh sách
    ModelConfigTab-->>User: Hiển thị danh sách mới và thông báo "Model updated successfully"
    
    deactivate ModelConfigUpdateDialog
    deactivate ModelConfigTab
    deactivate ModelConfigPage
```

---

## 4. Xóa Model Config

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigDeleteDialog as ModelConfigDeleteDialog.tsx
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    User->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    ModelConfigPage-->>User: Hiển thị giao diện
    
    User->>ModelConfigPage: Click tab "Model Configurations"
    ModelConfigPage->>ModelConfigTab: Khởi tạo ModelConfigTab
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigRoutes: GET /model-config
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_models_by_user()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelConfig]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTab: List[ModelConfigResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigTab-->>User: Hiển thị danh sách Model Config
    
    User->>ModelConfigTab: Chọn task type và xem danh sách Model Config
    ModelConfigTab-->>User: Lọc và hiển thị models theo task type
    
    User->>ModelConfigTab: Click "Delete" trên Model Config
    ModelConfigTab->>ModelConfigDeleteDialog: Mở dialog xác nhận
    activate ModelConfigDeleteDialog
    ModelConfigDeleteDialog-->>User: Hiển thị xác nhận với tên Model
    
    User->>ModelConfigDeleteDialog: Click "Delete" xác nhận
    ModelConfigDeleteDialog->>ModelConfigDeleteDialog: handleConfirm()
    ModelConfigDeleteDialog->>ModelConfigTab: onConfirm(modelId)
    
    ModelConfigTab->>ModelConfigRoutes: DELETE /model-config/{model_id}
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: delete_model()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: Success
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTab: MessageResponse
    deactivate ModelConfigRoutes
    
    ModelConfigTab->>ModelConfigTab: Xóa item khỏi danh sách
    ModelConfigTab-->>User: Hiển thị danh sách mới và thông báo "Model deleted successfully"
    
    deactivate ModelConfigDeleteDialog
    deactivate ModelConfigTab
    deactivate ModelConfigPage
```

---

## 5. Toggle trạng thái sử dụng Model Config

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigTable as ModelConfigTable.tsx
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    User->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    ModelConfigPage-->>User: Hiển thị giao diện
    
    User->>ModelConfigPage: Click tab "Model Configurations"
    ModelConfigPage->>ModelConfigTab: Khởi tạo ModelConfigTab
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigRoutes: GET /model-config
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_models_by_user()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelConfig]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTab: List[ModelConfigResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigTab-->>User: Hiển thị danh sách Model Config
    
    User->>ModelConfigTab: Chọn task type và xem danh sách Model Config
    ModelConfigTab-->>User: Lọc và hiển thị models theo task type
    
    User->>ModelConfigTab: Click toggle switch "Is Used" trên Model Config
    ModelConfigTab->>ModelConfigTable: handleToggleActive(taskType, id, isUsed)
    activate ModelConfigTable
    
    ModelConfigTable->>ModelConfigRoutes: PUT /model-config/{model_id}
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: update_model(model_id, {is_used})
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: Update is_used<br/>Set other configs is_used=false<br/>db.commit()
    
    ModelConfigService-->>ModelConfigRoutes: ModelConfig
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTable: ModelConfigResponse
    deactivate ModelConfigRoutes
    
    ModelConfigTable->>ModelConfigTab: Cập nhật danh sách models
    deactivate ModelConfigTable
    
    ModelConfigTab-->>User: Hiển thị trạng thái mới và thông báo "Model status updated successfully"
    
    deactivate ModelConfigTab
    deactivate ModelConfigPage
```

---

## 6. Preload Model Config

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ConfirmDialog as AlertDialog
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    User->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    ModelConfigPage-->>User: Hiển thị giao diện
    
    User->>ModelConfigPage: Click tab "Model Configurations"
    ModelConfigPage->>ModelConfigTab: Khởi tạo ModelConfigTab
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigRoutes: GET /model-config
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_models_by_user()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelConfig]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTab: List[ModelConfigResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigTab-->>User: Hiển thị danh sách Model Config (có thể rỗng)
    
    User->>ModelConfigTab: Chọn task type
    ModelConfigTab-->>User: Lọc và hiển thị models theo task type
    
    User->>ModelConfigTab: Click "Preload Models"
    ModelConfigTab->>ConfirmDialog: Hiển thị confirm dialog
    activate ConfirmDialog
    ConfirmDialog-->>User: "Do you want to preload default models?"
    
    User->>ConfirmDialog: Xác nhận preload
    ConfirmDialog->>ModelConfigTab: Confirmed
    deactivate ConfirmDialog
    
    ModelConfigTab->>ModelConfigRoutes: POST /model-config/preload/{user_id}
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: preload_models_for_user(user_id)
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: Create default ModelConfigs<br/>db.commit()
    
    ModelConfigService-->>ModelConfigRoutes: List[ModelConfig]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelConfigTab: List[ModelConfigResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigTab->>ModelConfigTab: Cập nhật danh sách với items mới
    ModelConfigTab-->>User: Hiển thị danh sách mới và thông báo "Models preloaded successfully"
    
    deactivate ModelConfigTab
    deactivate ModelConfigPage
```

---

## 7. Xem danh sách Model Wrapper

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    User->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    
    User->>ModelConfigPage: Click tab "Model Wrappers"
    ModelConfigPage->>ModelWrapperTab: Khởi tạo ModelWrapperTab
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelConfigRoutes: GET /model-config/wrapper
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigService: get_wrappers()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: Query all wrappers<br/>Order by task_type, updated_at DESC
    
    ModelConfigService-->>ModelConfigRoutes: List[ModelWrapper]
    deactivate ModelConfigService
    
    ModelConfigRoutes-->>ModelWrapperTab: List[ModelWrapperResponse]
    deactivate ModelConfigRoutes
    
    ModelWrapperTab->>ModelWrapperTab: Render wrapper list
    ModelWrapperTab-->>User: Hiển thị danh sách Model Wrapper
    deactivate ModelWrapperTab
    deactivate ModelConfigPage
```

---

## 8. Tạo Model Wrapper mới (Admin only)

```mermaid
sequenceDiagram
    actor Admin
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelWrapperCreateDialog as ModelWrapperCreateDialog.tsx
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    Admin->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    ModelConfigPage-->>Admin: Hiển thị giao diện
    
    Admin->>ModelConfigPage: Click tab "Model Wrappers"
    ModelConfigPage->>ModelWrapperTab: Khởi tạo ModelWrapperTab
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelConfigRoutes: GET /model-config/wrapper
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_wrappers()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelWrapper]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelWrapperTab: List[ModelWrapperResponse]
    deactivate ModelConfigRoutes
    
    ModelWrapperTab-->>Admin: Hiển thị danh sách Wrapper
    
    Admin->>ModelWrapperTab: Click "Add Wrapper"
    ModelWrapperTab->>ModelWrapperCreateDialog: Mở dialog
    activate ModelWrapperCreateDialog
    ModelWrapperCreateDialog-->>Admin: Hiển thị form trống
    
    Admin->>ModelWrapperCreateDialog: Nhập thông tin và click "Save"
    ModelWrapperCreateDialog->>ModelWrapperCreateDialog: Validate form
    ModelWrapperCreateDialog->>ModelWrapperTab: handleSave()
    
    ModelWrapperTab->>ModelConfigRoutes: POST /model-config/wrapper
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: create_wrapper()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: Create ModelWrapper<br/>db.commit()
    
    ModelConfigService-->>ModelConfigRoutes: ModelWrapper
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelWrapperTab: ModelWrapperResponse
    deactivate ModelConfigRoutes
    
    ModelWrapperTab->>ModelWrapperTab: Cập nhật danh sách với wrapper mới
    ModelWrapperTab-->>Admin: Hiển thị danh sách mới và thông báo "Wrapper created successfully"
    
    deactivate ModelWrapperCreateDialog
    deactivate ModelWrapperTab
    deactivate ModelConfigPage
```

---

## 9. Chỉnh sửa Model Wrapper (Admin only)

```mermaid
sequenceDiagram
    actor Admin
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelWrapperUpdateDialog as ModelWrapperUpdateDialog.tsx
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    Admin->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    ModelConfigPage-->>Admin: Hiển thị giao diện
    
    Admin->>ModelConfigPage: Click tab "Model Wrappers"
    ModelConfigPage->>ModelWrapperTab: Khởi tạo ModelWrapperTab
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelConfigRoutes: GET /model-config/wrapper
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_wrappers()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelWrapper]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelWrapperTab: List[ModelWrapperResponse]
    deactivate ModelConfigRoutes
    
    ModelWrapperTab-->>Admin: Hiển thị danh sách Wrapper
    
    Admin->>ModelWrapperTab: Click "Edit" trên Wrapper
    ModelWrapperTab->>ModelWrapperUpdateDialog: Mở dialog với dữ liệu hiện tại
    activate ModelWrapperUpdateDialog
    ModelWrapperUpdateDialog-->>Admin: Hiển thị form với thông tin Wrapper
    
    Admin->>ModelWrapperUpdateDialog: Cập nhật thông tin và click "Save"
    ModelWrapperUpdateDialog->>ModelWrapperUpdateDialog: Validate form
    ModelWrapperUpdateDialog->>ModelWrapperTab: handleSave()
    
    ModelWrapperTab->>ModelConfigRoutes: PUT /model-config/wrapper/{wrapper_id}
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: update_wrapper(wrapper_id, update_data)
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: Get ModelWrapper<br/>Update fields<br/>db.commit()
    
    ModelConfigService-->>ModelConfigRoutes: ModelWrapper
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelWrapperTab: ModelWrapperResponse
    deactivate ModelConfigRoutes
    
    ModelWrapperTab->>ModelWrapperTab: Cập nhật item trong danh sách
    ModelWrapperTab-->>Admin: Hiển thị danh sách mới và thông báo "Wrapper updated successfully"
    
    deactivate ModelWrapperUpdateDialog
    deactivate ModelWrapperTab
    deactivate ModelConfigPage
```

---

## 10. Xóa Model Wrapper (Admin only)

```mermaid
sequenceDiagram
    actor Admin
    participant ModelConfigPage as ModelConfigPage.tsx
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelWrapperDeleteDialog as ModelWrapperDeleteDialog.tsx
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigService as ModelConfigService

    Admin->>ModelConfigPage: Click nút "Mô hình" tại sidebar
    activate ModelConfigPage
    ModelConfigPage-->>Admin: Hiển thị giao diện
    
    Admin->>ModelConfigPage: Click tab "Model Wrappers"
    ModelConfigPage->>ModelWrapperTab: Khởi tạo ModelWrapperTab
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelConfigRoutes: GET /model-config/wrapper
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: get_wrappers()
    activate ModelConfigService
    ModelConfigService-->>ModelConfigRoutes: List[ModelWrapper]
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelWrapperTab: List[ModelWrapperResponse]
    deactivate ModelConfigRoutes
    
    ModelWrapperTab-->>Admin: Hiển thị danh sách Wrapper
    
    Admin->>ModelWrapperTab: Click "Delete" trên Wrapper
    ModelWrapperTab->>ModelWrapperDeleteDialog: Mở dialog xác nhận
    activate ModelWrapperDeleteDialog
    ModelWrapperDeleteDialog-->>Admin: Hiển thị xác nhận với tên Wrapper
    
    Admin->>ModelWrapperDeleteDialog: Click "Delete" xác nhận
    ModelWrapperDeleteDialog->>ModelWrapperDeleteDialog: handleConfirm()
    ModelWrapperDeleteDialog->>ModelWrapperTab: onConfirm(wrapperId)
    
    ModelWrapperTab->>ModelConfigRoutes: DELETE /model-config/wrapper/{wrapper_id}
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigService: delete_wrapper(wrapper_id)
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: Delete ModelWrapper<br/>Cascade delete ModelConfigs
    
    ModelConfigService-->>ModelConfigRoutes: Success
    deactivate ModelConfigService
    ModelConfigRoutes-->>ModelWrapperTab: MessageResponse
    deactivate ModelConfigRoutes
    
    ModelWrapperTab->>ModelWrapperTab: Xóa item khỏi danh sách
    ModelWrapperTab-->>Admin: Hiển thị danh sách mới và thông báo "Wrapper deleted successfully"
    
    deactivate ModelWrapperDeleteDialog
    deactivate ModelWrapperTab
    deactivate ModelConfigPage
```
