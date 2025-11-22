# Class Diagram - Quản lý ModelConfig và ModelWrapper

```mermaid
classDiagram
    class User {
        - id: UUID
        - username: String
        - email: String
        - phone: String
        - birthDay: LocalDate
        - firstName: String
        - lastName: String
        - avatar: String
        - userCode: String
        - userPrefix: String
        - isPremium: Boolean
        - providers: List~UserIdentity~
        + getId(): UUID
        + getRoles(): Set~Role~
    }

    class Role {
        - name: RoleType
        - users: List~User~
        + getName(): RoleType
    }

    class RoleType {
        <<enumeration>>
        STUDENT
        LECTURER
        ADMIN
    }

    class ModelWrapper {
        - id: UUID
        - name: String
        - instruction: String
        - task_type: TaskType
        - is_active: Boolean
        - created_at: DateTime
        - updated_at: DateTime
        + getId(): UUID
        + getName(): String
        + getTaskType(): TaskType
    }

    class ModelConfig {
        - id: UUID
        - user_id: UUID
        - name: String
        - wrapper_id: UUID
        - model_type: ModelType
        - is_used: Boolean
        - api_key: String
        - extra_config: JSON
        - lang: Lang
        - created_at: DateTime
        - updated_at: DateTime
        + getId(): UUID
        + getMaskedApiKey(): String
    }

    class TaskType {
        <<enumeration>>
        GENERATION
        EMBEDDING
        PARSE_DOC
        PARSE_AUDIO
        PARSE_IMAGE
    }

    class ModelType {
        <<enumeration>>
        REMOTE
        LOCAL
    }

    class Lang {
        <<enumeration>>
        EN
        VI
    }

    class ModelConfigTab {
        - models: ModelConfig[]
        - wrappers: ModelWrapper[]
        - loading: Boolean
        - selectedTask: TaskType
        - selectedModel: ModelConfig
        - dialogMode: String
        - selectedWrapperId: String
        + loadModels()
        + loadWrappers()
        + handleEdit(model)
        + handleCreate()
        + handleSave(id, data)
        + handleToggleActive(taskType, id, isUsed)
        + handleDelete(id)
        + handleShowInstruction(wrapper)
    }

    class ModelConfigBaseDialog {
        - open: Boolean
        - loading: Boolean
        - formData: Partial~ModelConfig~
        - rawExtraConfig: String
        - jsonError: String
        + handleSave()
        + isFormValid(): Boolean
        + handleExtraConfigChange(value)
    }

    class ModelWrapperTab {
        - wrappers: ModelWrapper[]
        - loading: Boolean
        - selectedWrapper: ModelWrapper
        - dialogMode: String
        + loadWrappers()
        + handleEdit(wrapper)
        + handleCreate()
        + handleSave(id, data)
        + handleDelete(id)
        + handleShowInstruction(wrapper)
    }

    class ModelWrapperBaseDialog {
        - open: Boolean
        - loading: Boolean
        - formData: Partial~ModelWrapper~
        + handleSave()
        + isFormValid(): Boolean
    }

    class ModelConfigService {
        + getModels(db, request): List~ModelConfig~
        + createModel(db, createData): ModelConfig
        + updateModel(db, modelId, updateData): ModelConfig
        + deleteModel(db, modelId)
        + preloadModelsForUser(db, userId, deleteExisting): List~ModelConfig~
        + getWrappers(db, taskType): List~ModelWrapper~
        + createWrapper(db, createData): ModelWrapper
        + updateWrapper(db, wrapperId, updateData): ModelWrapper
        + deleteWrapper(db, wrapperId)
        + preloadWrappers(db): List~ModelWrapper~
    }

    class ModelConfigRoutes {
        + getModels(request, db): List~ModelConfigResponse~
        + createModel(createData, db): ModelConfigResponse
        + updateModel(modelId, updateData, db): ModelConfigResponse
        + deleteModel(modelId, db): MessageResponse
        + preloadModels(userId, db): List~ModelConfigResponse~
        + getWrappers(taskType, db): List~ModelWrapperResponse~
        + createWrapper(createData, db): ModelWrapperResponse
        + updateWrapper(wrapperId, updateData, db): ModelWrapperResponse
        + deleteWrapper(wrapperId, db): MessageResponse
        + preloadWrappers(db): List~ModelWrapperResponse~
    }

    class ModelConfigResponse {
        - id: UUID
        - user_id: UUID
        - name: String
        - wrapper_id: UUID
        - model_type: ModelType
        - is_used: Boolean
        - api_key: String
        - extra_config: Dict
        - lang: Lang
        - created_at: DateTime
        - updated_at: DateTime
        - wrapper: ModelWrapperResponse
    }

    class ModelWrapperResponse {
        - id: UUID
        - name: String
        - instruction: String
        - task_type: TaskType
        - created_at: DateTime
        - updated_at: DateTime
    }

    class MessageResponse {
        - message: String
        - detail: String
    }

    User "1" -- "*" Role : has
    Role "*" -- "1" RoleType : has_type
    User "1" -- "*" ModelConfig : owns
    ModelConfig "*" -- "1" ModelWrapper : uses
    ModelConfig "*" -- "1" ModelType : has_type
    ModelConfig "*" -- "1" Lang : has_lang
    ModelWrapper "*" -- "1" TaskType : has_type
    
    ModelConfigTab ..> ModelConfigBaseDialog : uses
    ModelConfigTab ..> ModelConfigService : calls
    ModelWrapperTab ..> ModelWrapperBaseDialog : uses
    ModelWrapperTab ..> ModelConfigService : calls
    
    ModelConfigRoutes ..> ModelConfigService : uses
    ModelConfigRoutes ..> ModelConfigResponse : returns
    ModelConfigRoutes ..> ModelWrapperResponse : returns
    ModelConfigRoutes ..> MessageResponse : returns
    
    ModelConfigService ..> ModelConfig : manages
    ModelConfigService ..> ModelWrapper : manages
```

**Mô tả quan hệ:**
- **User**: Đại diện cho User của hệ thống, có thể có nhiều Role (STUDENT, LECTURER, ADMIN).
- **Role**: Đại diện cho vai trò của User trong hệ thống, được định danh bởi RoleType enum.
- **RoleType**: Enum định nghĩa các loại role: STUDENT, LECTURER, ADMIN.
- **ModelWrapper**: Đại diện cho một ModelWrapper, chứa thông tin về wrapper và instruction.
- **ModelConfig**: Đại diện cho cấu hình model của User, liên kết với ModelWrapper.
- **TaskType**: Enum định nghĩa các loại task: GENERATION, EMBEDDING, PARSE_DOC, PARSE_AUDIO, PARSE_IMAGE.
- **ModelType**: Enum định nghĩa loại model: REMOTE, LOCAL.
- **Lang**: Enum định nghĩa ngôn ngữ: EN, VI.
- **ModelConfigTab & ModelConfigBaseDialog**: Component Frontend để quản lý ModelConfig.
- **ModelWrapperTab & ModelWrapperBaseDialog**: Component Frontend để quản lý ModelWrapper.
- **ModelConfigService** (Backend): Service xử lý logic nghiệp vụ, truy vấn DB.
- **ModelConfigRoutes** (Backend): Route handlers xử lý HTTP requests.
- **ModelConfigResponse & ModelWrapperResponse**: DTO để trả về dữ liệu từ API.
- **MessageResponse**: Lớp dùng để trả về kết quả API.
