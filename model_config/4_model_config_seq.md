# Sequence Diagram - Cấu hình Model

## 1. Xem danh sách Model

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigBackendService as ModelConfigService (Backend)

    User->>ModelConfigTab: Truy cập tab Models
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigService: getModels()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigRoutes: GET /model-config
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigBackendService: get_models_by_user()
    activate ModelConfigBackendService
    
    ModelConfigBackendService->>ModelConfigBackendService: Query models<br/>Order by updated_at DESC
    
    ModelConfigBackendService-->>ModelConfigRoutes: [ModelConfig]
    deactivate ModelConfigBackendService
    
    ModelConfigRoutes-->>ModelConfigService: [ModelConfigResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigTab: ServiceResult~ModelConfig[]~
    deactivate ModelConfigService
    
    ModelConfigTab->>ModelConfigTab: Update state & Render models
    ModelConfigTab-->>User: Hiển thị danh sách Model phân loại theo Task Type
    deactivate ModelConfigTab
```

---

## 2. Tạo Model mới

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigCreateDialog as ModelConfigCreateDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigBackendService as ModelConfigService (Backend)

    User->>ModelConfigTab: Click nút "Tạo Model mới"
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigCreateDialog: Mở dialog (mode=create)
    activate ModelConfigCreateDialog
    
    ModelConfigCreateDialog->>ModelConfigService: getWrappers()
    activate ModelConfigService
    ModelConfigService->>ModelConfigRoutes: GET /model-config/wrapper
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigBackendService: get_wrappers()
    activate ModelConfigBackendService
    ModelConfigBackendService-->>ModelConfigRoutes: [ModelWrapper]
    deactivate ModelConfigBackendService
    ModelConfigRoutes-->>ModelConfigService: [ModelWrapperResponse]
    deactivate ModelConfigRoutes
    ModelConfigService-->>ModelConfigCreateDialog: ServiceResult~ModelWrapper[]~
    deactivate ModelConfigService
    
    ModelConfigCreateDialog-->>User: Hiển thị danh sách Wrapper và form tạo Model
    
    User->>ModelConfigCreateDialog: Chọn Wrapper, nhập thông tin và click "Tạo"
    
    ModelConfigCreateDialog->>ModelConfigCreateDialog: Validate form<br/>- name, wrapper_id, model_type, api_key
    
    ModelConfigCreateDialog->>ModelConfigService: createModel(data)
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigRoutes: POST /model-config
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigBackendService: create_model()
    activate ModelConfigBackendService
    
    ModelConfigBackendService->>ModelConfigBackendService: Create ModelConfig<br/>Encrypt API key<br/>db.add(), db.commit()
    
    ModelConfigBackendService-->>ModelConfigRoutes: ModelConfig
    deactivate ModelConfigBackendService
    
    ModelConfigRoutes-->>ModelConfigService: ModelConfigResponse
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigCreateDialog: ServiceResult~ModelConfig~
    deactivate ModelConfigService
    
    ModelConfigCreateDialog->>ModelConfigTab: Update danh sách
    deactivate ModelConfigCreateDialog
    
    ModelConfigTab->>ModelConfigTab: Refresh models list
    ModelConfigTab-->>User: Hiển thị thông báo thành công<br/>Danh sách được cập nhật
    deactivate ModelConfigTab
```

---

## 3. Chỉnh sửa Model

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigUpdateDialog as ModelConfigUpdateDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigBackendService as ModelConfigService (Backend)

    User->>ModelConfigTab: Click icon chỉnh sửa trên Model
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigUpdateDialog: Mở dialog (mode=edit, model=selected)
    activate ModelConfigUpdateDialog
    
    ModelConfigUpdateDialog->>ModelConfigService: getWrappers()
    activate ModelConfigService
    ModelConfigService->>ModelConfigRoutes: GET /model-config/wrapper
    activate ModelConfigRoutes
    ModelConfigRoutes->>ModelConfigBackendService: get_wrappers()
    activate ModelConfigBackendService
    ModelConfigBackendService-->>ModelConfigRoutes: [ModelWrapper]
    deactivate ModelConfigBackendService
    ModelConfigRoutes-->>ModelConfigService: [ModelWrapperResponse]
    deactivate ModelConfigRoutes
    ModelConfigService-->>ModelConfigUpdateDialog: ServiceResult~ModelWrapper[]~
    deactivate ModelConfigService
    
    ModelConfigUpdateDialog->>ModelConfigUpdateDialog: Populate form với dữ liệu hiện tại
    ModelConfigUpdateDialog-->>User: Hiển thị danh sách Wrapper và form chỉnh sửa
    
    User->>ModelConfigUpdateDialog: Chọn Wrapper, cập nhật thông tin và click "Lưu"
    
    ModelConfigUpdateDialog->>ModelConfigUpdateDialog: Validate form
    
    ModelConfigUpdateDialog->>ModelConfigService: updateModel()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigRoutes: PUT /model-config/{modelId}
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigBackendService: update_model()
    activate ModelConfigBackendService
    
    ModelConfigBackendService->>ModelConfigBackendService: Get existing model<br/>Update fields<br/>db.commit()
    
    ModelConfigBackendService-->>ModelConfigRoutes: ModelConfig
    deactivate ModelConfigBackendService
    
    ModelConfigRoutes-->>ModelConfigService: ModelConfigResponse
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigUpdateDialog: ServiceResult~ModelConfig~
    deactivate ModelConfigService
    
    ModelConfigUpdateDialog->>ModelConfigTab: Update danh sách
    deactivate ModelConfigUpdateDialog
    
    ModelConfigTab->>ModelConfigTab: Refresh models list
    ModelConfigTab-->>User: Hiển thị thông báo thành công<br/>Danh sách được cập nhật
    deactivate ModelConfigTab
```

---

## 4. Xóa Model

```mermaid
sequenceDiagram
    actor User
    participant ModelConfigTab as ModelConfigTab.tsx
    participant ModelConfigDeleteDialog as ModelConfigDeleteDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigBackendService as ModelConfigService (Backend)

    User->>ModelConfigTab: Click icon xóa trên Model
    activate ModelConfigTab
    
    ModelConfigTab->>ModelConfigDeleteDialog: Mở dialog xác nhận
    activate ModelConfigDeleteDialog
    
    ModelConfigDeleteDialog-->>User: Hiển thị thông báo xác nhận xóa
    
    User->>ModelConfigDeleteDialog: Click nút "Xóa"
    
    ModelConfigDeleteDialog->>ModelConfigService: deleteModel()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigRoutes: DELETE /model-config/{modelId}
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigBackendService: delete_model()
    activate ModelConfigBackendService
    
    ModelConfigBackendService->>ModelConfigBackendService: Get model<br/>Delete related usages<br/>db.delete(model)<br/>db.commit()
    
    ModelConfigBackendService-->>ModelConfigRoutes: MessageResponse
    deactivate ModelConfigBackendService
    
    ModelConfigRoutes-->>ModelConfigService: ServiceResult~void~
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelConfigDeleteDialog: ServiceResult~void~
    deactivate ModelConfigService
    
    ModelConfigDeleteDialog->>ModelConfigTab: Update danh sách
    deactivate ModelConfigDeleteDialog
    
    ModelConfigTab->>ModelConfigTab: Remove from list<br/>Refresh UI
    ModelConfigTab-->>User: Hiển thị thông báo thành công<br/>Model bị xóa khỏi danh sách
    deactivate ModelConfigTab
```

---

## 5. Xem danh sách Model Wrapper

```mermaid
sequenceDiagram
    actor Admin
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigBackendService as ModelConfigService (Backend)

    Admin->>ModelWrapperTab: Click tab "Model Wrappers"
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelConfigService: getWrappers()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigRoutes: GET /model-config/wrapper
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigBackendService: get_wrappers()
    activate ModelConfigBackendService
    
    ModelConfigBackendService->>ModelConfigBackendService: Query wrappers<br/>Order by created_at
    
    ModelConfigBackendService-->>ModelConfigRoutes: [ModelWrapper]
    deactivate ModelConfigBackendService
    
    ModelConfigRoutes-->>ModelConfigService: [ModelWrapperResponse]
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelWrapperTab: ServiceResult~ModelWrapper[]~
    deactivate ModelConfigService
    
    ModelWrapperTab->>ModelWrapperTab: Update state & Render wrappers
    ModelWrapperTab-->>Admin: Hiển thị danh sách Model Wrapper
    deactivate ModelWrapperTab
```

---

## 6. Tạo Model Wrapper mới

```mermaid
sequenceDiagram
    actor Admin
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelWrapperCreateDialog as ModelWrapperCreateDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigBackendService as ModelConfigService (Backend)

    Admin->>ModelWrapperTab: Click nút "Tạo Model Wrapper mới"
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelWrapperCreateDialog: Mở dialog
    activate ModelWrapperCreateDialog
    
    ModelWrapperCreateDialog-->>Admin: Hiển thị form tạo Wrapper
    
    Admin->>ModelWrapperCreateDialog: Nhập thông tin và click "Tạo"
    
    ModelWrapperCreateDialog->>ModelWrapperCreateDialog: Validate form<br/>- name, task_type, instruction
    
    ModelWrapperCreateDialog->>ModelConfigService: createWrapper()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigRoutes: POST /model-config/wrapper
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigBackendService: create_wrapper()
    activate ModelConfigBackendService
    
    ModelConfigBackendService->>ModelConfigBackendService: Create ModelWrapper<br/>db.add(), db.commit()
    
    ModelConfigBackendService-->>ModelConfigRoutes: ModelWrapper
    deactivate ModelConfigBackendService
    
    ModelConfigRoutes-->>ModelConfigService: ModelWrapperResponse
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelWrapperCreateDialog: ServiceResult~ModelWrapper~
    deactivate ModelConfigService
    
    ModelWrapperCreateDialog->>ModelWrapperTab: Update danh sách
    deactivate ModelWrapperCreateDialog
    
    ModelWrapperTab->>ModelWrapperTab: Refresh wrappers list
    ModelWrapperTab-->>Admin: Hiển thị thông báo thành công<br/>Danh sách được cập nhật
    deactivate ModelWrapperTab
```

---

## 7. Chỉnh sửa Model Wrapper

```mermaid
sequenceDiagram
    actor Admin
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelWrapperUpdateDialog as ModelWrapperUpdateDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigBackendService as ModelConfigService (Backend)

    Admin->>ModelWrapperTab: Click icon chỉnh sửa trên Wrapper
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelWrapperUpdateDialog: Mở dialog với dữ liệu Wrapper
    activate ModelWrapperUpdateDialog
    
    ModelWrapperUpdateDialog->>ModelWrapperUpdateDialog: Populate form với thông tin hiện tại
    ModelWrapperUpdateDialog-->>Admin: Hiển thị form chỉnh sửa
    
    Admin->>ModelWrapperUpdateDialog: Cập nhật thông tin và click "Lưu"
    
    ModelWrapperUpdateDialog->>ModelWrapperUpdateDialog: Validate form
    
    ModelWrapperUpdateDialog->>ModelConfigService: updateWrapper()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigRoutes: PUT /model-config/wrapper/{wrapperId}
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigBackendService: update_wrapper()
    activate ModelConfigBackendService
    
    ModelConfigBackendService->>ModelConfigBackendService: Get existing wrapper<br/>Update fields<br/>db.commit()
    
    ModelConfigBackendService-->>ModelConfigRoutes: ModelWrapper
    deactivate ModelConfigBackendService
    
    ModelConfigRoutes-->>ModelConfigService: ModelWrapperResponse
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelWrapperUpdateDialog: ServiceResult~ModelWrapper~
    deactivate ModelConfigService
    
    ModelWrapperUpdateDialog->>ModelWrapperTab: Update danh sách
    deactivate ModelWrapperUpdateDialog
    
    ModelWrapperTab->>ModelWrapperTab: Refresh wrappers list
    ModelWrapperTab-->>Admin: Hiển thị thông báo thành công<br/>Danh sách được cập nhật
    deactivate ModelWrapperTab
```

---

## 8. Xóa Model Wrapper

```mermaid
sequenceDiagram
    actor Admin
    participant ModelWrapperTab as ModelWrapperTab.tsx
    participant ModelWrapperDeleteDialog as ModelWrapperDeleteDialog.tsx
    participant ModelConfigService as ModelConfigService
    participant ModelConfigRoutes as model_config_routes
    participant ModelConfigBackendService as ModelConfigService (Backend)

    Admin->>ModelWrapperTab: Click icon xóa trên Wrapper
    activate ModelWrapperTab
    
    ModelWrapperTab->>ModelWrapperDeleteDialog: Mở dialog xác nhận
    activate ModelWrapperDeleteDialog
    
    ModelWrapperDeleteDialog-->>Admin: Hiển thị thông báo xác nhận xóa
    
    Admin->>ModelWrapperDeleteDialog: Click nút "Xóa"
    
    ModelWrapperDeleteDialog->>ModelConfigService: deleteWrapper()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigRoutes: DELETE /model-config/wrapper/{wrapperId}
    activate ModelConfigRoutes
    
    ModelConfigRoutes->>ModelConfigBackendService: delete_wrapper()
    activate ModelConfigBackendService
    
    ModelConfigBackendService->>ModelConfigBackendService: Get wrapper<br/>Check if in use<br/>Delete if not<br/>db.commit()
    
    ModelConfigBackendService-->>ModelConfigRoutes: MessageResponse
    deactivate ModelConfigBackendService
    
    ModelConfigRoutes-->>ModelConfigService: ServiceResult~void~
    deactivate ModelConfigRoutes
    
    ModelConfigService-->>ModelWrapperDeleteDialog: ServiceResult~void~
    deactivate ModelConfigService
    
    ModelWrapperDeleteDialog->>ModelWrapperTab: Update danh sách
    deactivate ModelWrapperDeleteDialog
    
    ModelWrapperTab->>ModelWrapperTab: Remove from list<br/>Refresh UI
    ModelWrapperTab-->>Admin: Hiển thị thông báo thành công<br/>Wrapper bị xóa khỏi danh sách
    deactivate ModelWrapperTab
```
